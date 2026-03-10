---
"@stoneforge/smithy": patch
---

Fix opencode spawn ENOENT on Windows by adding `shell: true` to the spawn call for proper PATH resolution
