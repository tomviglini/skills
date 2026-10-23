# NSIS

Read this reference when the installer is built with NSIS. Target: NSIS 3.11 with MUI2, LogicLib, and nsDialogs. Reviewed on 2026-10-23, and exercised in one consuming project on Windows x64 (new install, update, uninstall). Adapt names, paths, and the app's window class.

## Pages

```nsis
!define MUI_PAGE_CUSTOMFUNCTION_LEAVE PlanData
!insertmacro MUI_PAGE_DIRECTORY
Page custom DataPage DataLeave        ; shown only when something would be lost
Page custom OptionsPage OptionsLeave  ; start with Windows, running-copy notice
!insertmacro MUI_PAGE_INSTFILES
```

## Plan and warning

```nsis
Function PlanData
  StrCpy $DataPlan 0
  StrCpy $DataAccepted 0
  ${IfNot} ${FileExists} "$INSTDIR\data\*.*"
    Return
  ${EndIf}
  InitPluginsDir
  File "/oname=$PLUGINSDIR\${APP_EXE}" "stage\${APP_EXE}"
  ExecWait '"$PLUGINSDIR\${APP_EXE}" --data-plan "$INSTDIR\data" "$PLUGINSDIR\data-plan.txt"' $DataPlan
  ${If} $DataPlan == 2
    ClearErrors
    FileOpen $0 "$PLUGINSDIR\data-plan.txt" r
    FileRead $0 $BackupLabel
    StrCpy $BackupLabel $BackupLabel -2   ; drop the CRLF
    StrCpy $DataLines ""
    ${Do}
      FileRead $0 $1
      ${If} ${Errors}
        ${Break}
      ${EndIf}
      StrCpy $DataLines "$DataLines$1"
    ${Loop}
    FileClose $0
  ${EndIf}
FunctionEnd

Function DataPage
  ${If} $DataPlan != 2
    Abort   ; skips the page
  ${EndIf}
  !insertmacro MUI_HEADER_TEXT "Saved data" "This version cannot keep part of the data the installed copy saved."
  nsDialogs::Create 1018
  Pop $0
  ${NSD_CreateLabel} 0 0 100% 70% "If you continue:$\r$\n$\r$\n$DataLines$\r$\nEverything else in the data folder is kept. Cancel leaves the installed version as it is."
  Pop $0
  ${NSD_CreateCheckbox} 0 75% 100% 14u "$BackupLabel"
  Pop $BackupCheckbox
  ${NSD_Check} $BackupCheckbox
  nsDialogs::Show
FunctionEnd

Function DataLeave
  ${NSD_GetState} $BackupCheckbox $KeepBackup
  StrCpy $DataAccepted 1
FunctionEnd
```

## Options and running copies

```nsis
Function .onInit
  ; the current choice: off for a new install, kept on an update
  ReadRegStr $0 HKCU "Software\Microsoft\Windows\CurrentVersion\Run" "${RUN_VALUE}"
  ${If} $0 != ""
    StrCpy $StartWithWindows ${BST_CHECKED}
  ${Else}
    StrCpy $StartWithWindows ${BST_UNCHECKED}
  ${EndIf}
FunctionEnd

Function OptionsLeave
  ${NSD_GetState} $StartWithWindowsCheckbox $StartWithWindows
  FindWindow $0 "${APP_WINDOW_CLASS}"
  ${If} $0 <> 0
    MessageBox MB_OKCANCEL|MB_ICONINFORMATION "${PRODUCT_NAME} is running. Setup closes it to continue." IDOK close_it
    Abort
    close_it:
  ${EndIf}
FunctionEnd
```

Prefer a named label to relative jumps inside LogicLib blocks. Close the copy in the install section, by sending `WM_CLOSE` to the app's window first and then running `taskkill` as a fallback.

## Apply

```nsis
  StrCpy $1 ""
  ${If} $DataAccepted == 1
    StrCpy $1 " --accept"
    ${If} $KeepBackup == ${BST_CHECKED}
      StrCpy $1 "$1 --backup"
    ${EndIf}
  ${EndIf}
  ExecWait '"$INSTDIR\${APP_EXE}" --data-apply "$INSTDIR\data"$1' $0
```

Grant users write access to the data folder (for example with `icacls ... *S-1-5-32-545:(OI)(CI)(M)`) before running the apply mode, so the files it writes inherit the permission.

## Uninstall

```nsis
  FindFirst $0 $1 "$INSTDIR\data\*"
  ${DoWhile} $1 != ""
    ${If} $1 != "."
    ${AndIf} $1 != ".."
    ${AndIf} $1 != "backup"
      RMDir /r "$INSTDIR\data\$1"
      Delete "$INSTDIR\data\$1"
    ${EndIf}
    FindNext $0 $1
  ${Loop}
  FindClose $0
  ${If} ${FileExists} "$INSTDIR\data\backup\*.*"
    MessageBox MB_YESNO|MB_ICONQUESTION|MB_DEFBUTTON2 "Also delete the backup copies in $INSTDIR\data\backup?" /SD IDNO IDNO keep_backups
    RMDir /r "$INSTDIR\data\backup"
    keep_backups:
  ${EndIf}
  RMDir "$INSTDIR\data"
```

## Size in Installed Apps

Measure the installed files without the data folder, then add a bounded data estimate in KiB. Write the result as `EstimatedSize` under the uninstall key:

```nsis
  ${GetSize} "$INSTDIR" "/S=0K /G=0" $0 $1 $2
  IntOp $0 $0 + ${DATA_ESTIMATE_KB}
  WriteRegDWORD HKLM "${PRODUCT_KEY}" "EstimatedSize" $0
```
