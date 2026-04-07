# Prototype Scope — v0.1

This document defines exactly what is built in the prototype. It is a contract, not a wishlist. Any feature not listed in the **In Scope** section is deferred, regardless of how small it seems. When in doubt, defer it.

The prototype's sole purpose is to prove the core loop: **sail, trade, time, rank.** If that loop is fun, the game is worth building further.

---

## In Scope

### World & Map
- A single hand-built map with **4–5 settlements** positioned on open water
- Water boundaries defining the playable area (from a simplified or placeholder shape; real lake shapefile import may be used if straightforward, otherwise a hand-shaped polygon is fine)
- Out-of-bounds areas marked and enforced (boat cannot leave the playable area)
- No dynamic weather, tides, or hazards

### Sailing Mechanics
- A player-controlled sailboat with **realistic-but-simplified physics**
- **Wind system:** wind direction and speed are defined by a 2D noise map covering the entire world. Each position on the map samples the noise field to get a base wind angle and speed for that location, meaning different parts of the world have consistently different wind character (e.g., one passage tends to be a beat, another tends to be a reach). On top of the noise field, a small random perturbation of up to ±10° is applied per session, so players generally experience familiar conditions on known routes but each trip may offer slightly more favorable or less favorable angles. Wind changes smoothly as the boat moves through the field.
- **Wind display:** wind direction is shown to the player at all times in two forms:
  - **Map-relative:** the true compass direction of the wind at the boat's current position (e.g., wind arrow on the HUD or minimap)
  - **Boat-relative:** the angle of the wind relative to the boat's heading (used for sail selection decisions), displayed as part of the sailing HUD
- **Three sail configurations:** Jib (upwind), Genoa (reaching), Spinnaker (downwind) — selectable by the player
- A **brief delay** when switching sail configurations (representing crew work)
- Boat speed calculated per sail configuration — only the upgrades relevant to the active sail apply:
  > **Jib:** `speed = (headsail × mainsail × hull × weight_reduction) × f(jib, θ)`
  > **Genoa:** `speed = (headsail × mainsail × hull × weight_reduction) × f(genoa, θ)`
  > **Spinnaker:** `speed = (mainsail × spinnaker × hull × weight_reduction) × f(spinnaker, θ)`
  >
  > See [sailing-mechanics.md](../design/sailing-mechanics.md) for the full wind angle factor table.
- The boat is steerable but subject to wind — sailing directly into the wind results in minimal forward progress (no-go zone)
- No penalty for carrying cargo — speed is cargo-agnostic

### Settlements & Ports
- **4–5 distinct named settlements**, each with:
  - A docking area where the player can stop and interact
  - A **merchant** offering a small set of goods to buy and sell
  - A **boat yard** where upgrades can be purchased
- Each settlement carries different goods at different prices, creating natural trade routes
- No dockmaster, quest giver, or NPC dialogue beyond simple merchant UI

### Economy
- A small set of **tradeable goods** (target: 4–6 items) with prices that vary by settlement
- Players buy goods at one port and sell at another for profit
- Prices are **fixed per settlement** in the prototype (no dynamic supply/demand)
- A simple **cargo hold** with a slot or weight limit (one upgrade tier increases capacity)

### Boat Upgrades
- **One upgrade tier** available per category (prototype does not implement full tier progression)
- Upgrades are purchased with money earned from trading
- Six upgrade categories, each with a base value and one upgraded value:

| Upgrade | Applies to | Affects |
|---|---|---|
| Headsail | Jib, Genoa | Upwind and reaching speed factor |
| Mainsail | Jib, Genoa, Spinnaker | Speed factor in all configurations |
| Spinnaker | Spinnaker | Downwind speed factor |
| Hull | All | Universal speed factor |
| Weight Reduction | All | Universal speed factor |
| Cargo Capacity | — | Maximum cargo slots |

- Upgrades persist across play sessions (saved to disk)

### Timer & Racing System
- A timer **starts automatically** when the player departs a settlement
- The timer **stops and is recorded** when the player enters another settlement's **dock zone** (a circular trigger area around each settlement, visible on the minimap)
- Times are stored **locally** per route (origin → destination, directional)
- A **top-3 leaderboard** per route is displayed at each settlement (or on the in-game map/HUD)
- Records persist across play sessions (saved to disk)
- **Single-leg routes only** in the prototype (no multi-stop tracking yet)
- No record reset interval in the prototype — records are permanent until cleared manually or on a new save

### UI & HUD
- In-game HUD showing:
  - **Wind direction (map-relative):** compass bearing of wind at the boat's current position
  - **Wind angle (boat-relative):** angle of wind to the boat's heading, to aid sail selection
  - **Wind speed:** current wind speed at the boat's position
  - Active sail configuration with the option to switch
  - Active timer (running elapsed time for the current leg)
  - Current cargo and money
- Settlement interaction screen with tabs for **Merchant** and **Boat Yard**
- A simple **map screen** showing settlement locations, the player's current position, and route leaderboard access
- A **minimap** (always visible, corner of the HUD) showing:
  - The full playable area with settlement markers
  - The player's current position and heading
  - A **track line** of the boat's path since it last departed a settlement (resets on arrival)
- Main menu with **New Game** and **Continue**
- Basic save/load (auto-save on arriving at a settlement)

### Presentation
- 3D Godot 4 scene with a **fixed camera angle** (top-down or slight isometric tilt)
- Placeholder 3D models for boat, water, and settlements (Blender primitives or simple meshes are acceptable)
- Basic water material (flat color or simple shader — no deep ocean simulation)
- Minimal sound: ambient wind/water loop, a UI interaction sound. No music required.
- Runs in a browser build (HTML5 export) for itch.io

---

## Explicitly Deferred

The following are **not** built in the prototype. They are recorded here so they are not forgotten and not accidentally started.

| Feature | Notes |
|---|---|
| Multiple upgrade tiers (tiers 2–4) | Prototype uses one tier only |
| Dynamic pricing / supply and demand | Prices are fixed per settlement |
| Multi-stop route timing | Single legs only in v0.1 |
| Record reset interval | No monthly wipe in prototype |
| Online leaderboards | Local records only |
| Ghost boat replays | Deferred to post-prototype online feature |
| Multiplayer / async other players | Deferred entirely |
| Idle activities (fishing, instruments, messages in a bottle, floating cargo recovery) | Full system deferred; no placeholder needed. See [design/world-activities.md](../design/world-activities.md) |
| Idle activity economy (selling fish, floating cargo rewards) | Deferred |
| Animal / character theme | Deferred; placeholder models are character-agnostic |
| NPC dialogue or quests | No narrative layer in prototype |
| Day/night cycle | Deferred |
| Music | Deferred |
| Waves and wave physics | Visual and physical effect; world boundary and boat physics should not assume flat calm water permanently |
| Tides and variable water depth | World boundary geometry should be designed to support future tidal variation; do not hardcode as a fixed static polygon |
| Visual polish (lighting, shaders, particles) | Deferred; placeholder visuals only |
| Mobile / gamepad input | Prototype targets keyboard/mouse only |
| Settings menu | Deferred (no audio sliders, keybinding, etc.) |
| Camera zoom | World is small enough for a fixed distance; minimap covers navigation needs |
| Player-adjustable camera angle | Angle is fixed in the prototype but should be implemented as a parameter; a player-facing tilt slider is a future option |

---

## Success Criteria

The prototype is considered complete when a player can:

1. Launch the game and start a new save
2. Sail from one settlement to another, with wind visibly affecting their speed and heading
3. Switch between sail configurations during a voyage
4. Dock at a settlement, buy goods, and sell them at a different settlement for profit
5. Purchase a boat upgrade and notice its effect on the water
6. See their voyage time recorded and ranked on a route leaderboard
7. Close the game and return to find their progress (upgrades, money, records) intact

