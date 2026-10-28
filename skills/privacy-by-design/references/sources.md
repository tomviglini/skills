# Sources

The practices in privacy-by-design summarize these sources in the skill's own words. They were reviewed on 2026-10-26.

| Source | What the skill takes from it |
| --- | --- |
| [Regulation (EU) 2016/679 (GDPR), Article 5](https://gdpr-info.eu/art-5-gdpr/) | Purpose limitation, data minimisation, storage limitation, integrity and confidentiality |
| [GDPR, Article 25](https://gdpr-info.eu/art-25-gdpr/) | Data protection by design and by default: only the data each purpose needs, by default |
| [Privacy by Design: The 7 Foundational Principles (Ann Cavoukian)](https://www.ietf.org/slides/slides-privacyws-privacy-by-design-the-seven-foundational-principles-00.pdf) | Privacy as the default setting, embedded in the design, proactive rather than remedial |
| [Mozilla: Lean Data Practices](https://blog.mozilla.org/netpolicy/2017/01/26/dpd-lean-data-practices/) | Stay lean, build in security, engage users; retention limits that are followed |
| [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html) | Data to exclude from logs; file paths and personal data needing special handling; retention and protection of log files |
| [OWASP User Privacy Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html) | Third-party content revealing the user's address; telling users the truth about what is kept |
| [MITRE CWE-532: Insertion of Sensitive Information into Log File](https://cwe.mitre.org/data/definitions/532.html) | No secrets in logs; no debug logs in shipped builds |
| [Android: Security tips](https://developer.android.com/privacy-and-security/security-tips) | No personal data in the shared system log; private internal storage; app-created identifiers instead of device identifiers; fewer permissions; HTTPS |
| [RFC 5737](https://www.rfc-editor.org/rfc/rfc5737), [RFC 7042](https://www.rfc-editor.org/rfc/rfc7042), [RFC 2606](https://www.rfc-editor.org/rfc/rfc2606) | Address, MAC, and domain ranges reserved for documentation and examples |
