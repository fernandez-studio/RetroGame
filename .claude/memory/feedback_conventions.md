---
name: Project conventions — versioning and file structure
description: How Bryan wants new versions and features handled in this project
type: feedback
---
Each new milestone gets a new numbered file (`gunslinger_03.html`, `gunslinger_04.html`, etc.) — never overwrite a previous version.

**Why:** Preserves a working history of the game at each stage; easy to roll back or reference older builds.

**How to apply:** When starting a new feature batch or milestone, copy the current file to the next number and work in that new file. Always commit and push to `main` after each meaningful version.
