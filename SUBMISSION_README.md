# XPLOIT-PS — Team Submission
## XPECTO 2026 · IIT Mandi · Team: Antigravity

---

## Summary of Completed Challenges

### 1. DOOM Companion Bot (`dooooom`)
- Added a companion bot to Chocolate DOOM that follows the player and fights enemies.
- Key files: `DOOM_WRITEUP.md`, modified C source files in `dooooom/`.

### 2. Bad Compiler (`bad_compiler`)
- Reverse-engineered the broken `.wut` compiler, fixed two critical bugs:
  - `^` (print) was incorrectly popping the stack.
  - `$` (swap/dup) was duplicating values incorrectly.
- Built a working replacement compiler (`compiler.py`) and a demo program (`antigravity.wut`).
- Key files: `WRITEUP.md`, `compiler.py`, `antigravity.wut`.

### 3. Vault Challenge (`vault_chal`)
- Reverse-engineered the ELF binary to bypass authentication and vault unlock checks.
- Applied two binary patches to the compiled binary (`chal`).
- Key files: `VAULT_WRITEUP.md`, patched `chal`.

### 4. Dungeon Challenge (`dungeon_challenge`)
- Extracted and analyzed source bytecode from `xploit.exe` (PyInstaller bundle).
- Identified 6 stages (`_S1`–`_S6`) and located suspicious constants.
- Applied initial patches to the bytecode.
- Key files: `RESEARCH_LOG.md`, analysis scripts.

---

*XPECTO 2026 · IIT Mandi · 14–16 March*
