# XPLOIT-PS — Team Submission
## XPECTO 2026 · IIT Mandi
## 👥 Team: **Circuits Labs**

---

## Team Members
*(Add your names here if needed)*

---

## Summary of Completed Challenges

### 1. ✅ DOOM Companion Bot (`dooooom`)
- Added a companion bot to Chocolate DOOM that follows the player and fights enemies
- Used existing DOOM AI systems (`p_enemy.c`) — minimal new code
- Bot spawns with 500 HP, follows player, engages enemies, no friendly fire
- **See:** `dooooom/DOOM_WRITEUP.md`

### 2. ✅ Bad Compiler (`bad_compiler`)
- Reverse-engineered the broken `.wut` compiler and identified 2 critical bugs:
  - `^` (print) was incorrectly popping the stack
  - `$` (swap/dup) was duplicating values instead of swapping
- Built a working replacement compiler (`compiler.py`) and demo program (`circuits_labs.wut`)
- **See:** `bad_compiler/WRITEUP.md`

### 3. ✅ Vault Challenge (`vault_chal`)
- Reverse-engineered the ELF binary to bypass authentication and vault unlock checks
- Applied two minimal binary patches to produce `VAULT SYSTEM CLEARED`
- **See:** `vault_chal/VAULT_WRITEUP.md`

### 4. 🔄 Dungeon Challenge (`dungeon_challenge`)
- Extracted Python bytecode from `xploit.exe` (PyInstaller bundle, Python 3.10)
- Identified 6 game stages (`_S1`–`_S6`) from the bytecode structure
- Located suspicious constants and applied initial patches to the bytecode
- **See:** `dungeon_challenge/` for analysis scripts and patched files

---

*XPECTO 2026 · IIT Mandi · 14–16 March*
