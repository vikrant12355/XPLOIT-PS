# XPLOIT-PS: DOOM Companion Bot Writeup

## 1. Project Overview
The objective was to modify the Chocolate DOOM source code to introduce a "Companion Bot" that assists the player. Unlike standard monsters, this bot needed to intelligently follow the player, engage hostile monsters, and respect the player's presence (preventing friendly fire).

## 2. Core Architecture & Engine Integration
Instead of building a separate AI system, I integrated the companion logic directly into the existing DOOM "Thinker" and "Action" systems.

### A. The "Friendly" Identity (`p_mobj.h`)
I introduced a new object flag, `MF_FRIENDLY (0x40000000)`, to the `mobjflag_t` enum. This allows any object in the game world to be identified as an ally. Every logic check for "Should I attack?" or "Is this a target?" was updated to respect this flag.

### B. The Companion Brain (`p_enemy.c`)
I implemented two new core routines:
- **`P_LookForEnemies`**: A customized version of DOOM's `P_LookForPlayers`. Instead of finding a player, it iterates through the `thinkercap` to find any object that is `SHOOTABLE`, alive, and **not** friendly.
- **`A_CompanionChase`**: The primary state-machine for the bot.
    - **Follow State**: If no enemies are nearby, it calculates the `P_AproxDistance` to the player. If the distance exceeds 128 units, it uses `P_Move` and `P_NewChaseDir` to navigate toward the player.
    - **Combat State**: It periodically triggers `P_LookForEnemies`. If a monster is detected, the bot switches its target and utilizes standard `A_Chase` logic to engage.

### C. Preventing "Betrayal" (`p_inter.c`)
In a standard DOOM engine, hitting a monster makes it target you. For a companion, this is a bug. I modified `P_DamageMobj` to include a **Friendly Fire Bypass**:
```c
if (source && (source->player || (source->flags & MF_FRIENDLY))) {
    if (target->player || (target->flags & MF_FRIENDLY)) {
        return; // No damage or hostility triggered between allies
    }
}
```

### D. Automated Deployment (`p_setup.c`)
To ensure the bot is always available, I hooked into `P_SetupLevel`. At the start of every map, the engine now automatically spawns a `MT_POSSESSED` actor at the player's start coordinate, applies the `MF_FRIENDLY` flag, and sets its base health to 500.

## 3. Challenges & Fixes during Development
- **Corridor Gridlock**: Initially, the bot stayed too close to the player, blocking doorways. I fixed this by implementing a "dead zone" (128 units) where the bot ceases all movement if it is close enough to the player.
- **Target Confusion**: The bot originally inherited standard monster behavior, meaning it would try to "chase" the player to attack them. I had to explicitly separate the "Follow movement" logic from the "Attack" logic inside the state handler.
- **Stair Navigation**: By hooking into the existing `P_Move` system rather than writing a custom coordinate-slider, the bot automatically inherits DOOM's sector-height and stair-climbing logic, preventing it from getting stuck on geometry.

## 4. Summary of Changes
- `p_mobj.h`: Flag definition.
- `p_enemy.c`: `A_CompanionChase` logic and enemy search.
- `p_inter.c`: Friendly fire protection.
- `p_setup.c`: Persistent level spawning.
- `info.c`/`h`: Added `A_CompanionChase` to the internal action table.
