# DOOM — Friendly Fire

## What You Are Given

A modified Chocolate DOOM source code folder (`chocolate-doom.zip`).

## Setup Instructions

1. Extract `chocolate-doom.zip`.
2. Download the **Freedoom Phase 1** WAD (`freedoom1.wad`) from their official GitHub releases: [https://github.com/freedoom/freedoom/releases](https://github.com/freedoom/freedoom/releases)
3. Place `freedoom1.wad` inside the extracted `chocolate-doom` folder.
4. Open a terminal in the `chocolate-doom` folder and run the game:
   ```bash
   ./src/chocolate-doom -iwad freedoom1.wad
   ```

---

## The Task

DOOM's monsters already know how to move, navigate, shoot, and react. All of that logic exists in the codebase.

Right now everything in the game wants to kill you.

**Add a companion bot that follows you through the level and helps you fight.**

How you do it, how deep you go, and how well it behaves is entirely up to you.

---

## How to Submit

Submit your entire modified Chocolate DOOM folder along with:

- A short writeup (PDF or Markdown) covering what you built, which parts of the existing codebase you worked with, and what broke during development that was only visible by playing
- A Google Drive link to a screen recording of approximately one minute showing the bot in action

If you cannot explain something you did, do not include it.

---

## Scoring

There is no checklist. Judges will build your folder, play your submission, and read your writeup.

Better marks go to submissions where:

- The bot uses existing DOOM systems rather than new code written from scratch
- Edge cases are handled — the bot behaves correctly in corridors, at doors, near stairs, under fire
- The writeup shows genuine understanding of how the engine works
- The recording shows a bot that actually feels like it belongs in the game

---

## ✅ Our Solution — Team **Circuits Labs**

**Approach:** Used DOOM's existing monster AI systems (`p_enemy.c`) rather than writing new code from scratch.

**What We Built:**
- `MF_FRIENDLY` flag added to `p_mobj.h` to tag ally entities
- `A_CompanionChase()` in `p_enemy.c` — bot follows player and attacks enemies
- `P_LookForEnemies()` helper — bot scans for nearby hostile targets
- Modified `P_DamageMobj()` in `p_inter.c` — prevents friendly fire between player and bot
- Auto-spawn in `P_SetupLevel()` in `p_setup.c` — bot appears at start of every level with **500 HP**

**Key Files Modified:**

| File | Change |
|---|---|
| `p_mobj.h` | Added `MF_FRIENDLY` flag |
| `p_enemy.c` | Added `A_CompanionChase`, `P_LookForEnemies` |
| `p_inter.c` | Friendly fire prevention |
| `p_setup.c` | Auto-spawn companion at level start |
| `info.c` / `info.h` | Registered companion bot states |

**See:** `DOOM_WRITEUP.md` for full documentation.

---