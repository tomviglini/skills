# Build Script Example

Read this when adding checksum naming to a Node.js build script. Adapt paths, variant names, and the build command to the project; keep its existing process helpers.

API target: Node.js 18 or newer built-in modules (`node:crypto`, `node:fs`, `node:path`). Documentation reviewed on 2026-09-30 against the [Node.js crypto API](https://nodejs.org/api/crypto.html#class-hash). The pattern was executed with Node.js 22 in Expo Android builds on 2026-09-30.

```js
import { createHash } from 'node:crypto';
import { copyFileSync, mkdirSync, readFileSync, rmSync } from 'node:fs';
import { join } from 'node:path';

// Remove the previous conventional output first, so a failed build cannot leave
// a stale file to be named.
rmSync(source, { force: true });
runBuild(); // The project's build command; it must throw or exit on failure.

const digest = createHash('sha256').update(readFileSync(source)).digest('hex');
const name = `${project}-${variant}-${target}-${digest.slice(0, 8)}.apk`;

// Use a stable directory when regeneration or another variant's build deletes
// the conventional output directory.
mkdirSync(artifactDirectory, { recursive: true });
const destination = join(artifactDirectory, name);
copyFileSync(source, destination);

console.log(`\nAPK: ${destination}`);
console.log(`SHA-256: ${digest}`);
```

Reading the whole artifact into memory is adequate for typical mobile packages and installers. For multi-gigabyte artifacts, stream the file through the hash instead.
