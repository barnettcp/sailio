# UI Flows & UX

This document catalogues every screen and UI state in Sailio, describes what each one shows, how the player reaches it, what actions are available, and how they leave. It does not specify visual design or pixel layout — that belongs in [art/direction.md](../art/direction.md). It is a functional specification for what must be built.

Screens marked **[PROTOTYPE]** are required in v0.1. Screens marked **[FULL]** are deferred.

---

## Screen Map

```
Main Menu
  ├── New Game → Tutorial Overlay → In-Game (Sailing)
  └── Continue → In-Game (Sailing)

In-Game (Sailing)  ←──────────────────────────────────────┐
  ├── [enter dock zone] → Settlement Screen                │
  │     ├── Merchant Tab                                   │
  │     ├── Boat Yard Tab                                  │
  │     ├── Leaderboard Tab                                │
  │     └── [depart] → Arrival Overlay → In-Game ─────────┘
  └── [open map] → Map Screen
        └── [close] → In-Game (Sailing)
```

---

## Screen: Main Menu [PROTOTYPE]

**How the player gets here:** Game launch, or returning from a session.

### Elements
- Game title / logo
- **New Game** button
- **Continue** button (greyed out if no save exists)
- Version number (small, unobtrusive)

### Actions
| Action | Result |
|---|---|
| New Game | Clears any existing save, begins tutorial flow, loads In-Game |
| Continue | Loads existing save, drops player directly into In-Game at last known position |

### Notes
- No settings screen in the prototype. Settings are deferred.
- No credits screen in the prototype.

---

## Screen: Tutorial Overlay [PROTOTYPE]

**How the player gets here:** New Game only. Overlays the In-Game view — the world is live underneath it.

The tutorial is a short linear sequence of hint cards that introduce the core mechanics. Cards are dismissed one at a time by the player. The tutorial does not pause the world — wind is real, the boat can move.

### Tutorial sequence
1. **Sailing controls** — introduce steering (A/D), sail config selection (1/2/3), and the no-go zone. Player is prompted to sail out of Rockaway.
2. **Wind instruments** — point to the wind direction and boat-relative wind angle in the HUD. Prompt player to select the appropriate sail config.
3. **Timer** — on departure from Rockaway, the timer starts. A card acknowledges this and explains it will record their time to Gopher's Bay.
4. **Arrival** — on entering Gopher's Bay dock zone, the timer stops. The arrival overlay plays. A card explains the recorded time and leaderboard.
5. **Merchant** — inside Gopher's Bay, a card opens the Merchant tab and walks through buying and selling.
6. **Boat Yard** — a card introduces the Boat Yard tab (player is not required to purchase anything).
7. **Done** — tutorial ends. A card suggests Woollie needs Timber as a loose next destination. No further hand-holding.

Cards should not block the UI or force the player to complete them in order — each card can be dismissed early. A player who already understands the game should be able to skip through in under 30 seconds.

---

## Screen: In-Game (Sailing) [PROTOTYPE]

**How the player gets here:** From Main Menu (New Game or Continue), or on departing a settlement.

This is the primary game state. The player is at the helm. The 3D world is fully rendered and interactive.

### HUD Layout

The HUD uses **corner anchoring** — each corner owns one category of information. The centre of the screen is kept as clear as possible; that is where the boat is and where sailing happens.

```
┌─────────────────────────────────────────────┐
│ [Timer]                     [Compass Rose]  │
│ [Wind speed · True wind direction]          │
│                                             │
│                                             │
│            (game world)                     │
│       [Apparent wind indicator]             │
│                                             │
│ [Cargo · Money]              [Minimap]      │
│ [Sail config selector]                      │
└─────────────────────────────────────────────┘
```

| Corner | Contents |
|---|---|
| **Top-left** | Active timer, wind speed, true wind direction |
| **Top-right** | Compass rose with boat heading bearing |
| **Bottom-left** | Current cargo inventory, current gold, sail configuration selector |
| **Bottom-right** | Minimap |
| **Near boat (in-world)** | Apparent wind angle indicator (near-diegetic) |

### Top-Left: Timer & Wind
- **Timer:** Elapsed time since last departure, running in real time. Format: `MM:SS.T` (minutes, seconds, tenths). Displays `--:--.--` when not on a timed leg (e.g., inside a dock zone or not yet departed).
- **Wind speed:** Current wind speed at the boat's position, sampled from the noise field. Units: knots.
- **True wind direction:** Compass bearing of the wind at the boat's position. Displayed as a cardinal/intercardinal label (e.g., "NNW") or a small directional arrow.

### Top-Right: Compass Rose
- Fixed compass rose graphic showing cardinal directions.
- A needle or boat icon rotates to show the boat's current heading.
- Does not move — the rose is orientation-fixed to the map, not to the boat.

### Bottom-Left: Cargo, Money, Sail Config
- **Cargo slots:** A grid showing occupied and empty cargo slots. Each occupied slot shows the good name and quantity (e.g., "Rope ×2"). Slots are visual — the player can see at a glance how full the hold is.
- **Gold:** Current gold balance, always visible.
- **Sail config selector:** Three buttons or labels: Jib / Genoa / Spinnaker. The active config is highlighted. Selecting a different config triggers a brief switching delay. Buttons are always visible and tappable.

### Bottom-Right: Minimap
- Always-visible minimap showing the full playable area at reduced scale.
- Contents:
  - Shoreline outline
  - Settlement markers (icons, always visible)
  - Dock zone rings around each settlement
  - Player position and heading (small boat icon)
  - Track line: the path the boat has sailed since last departure (resets on arrival)
  - Wind arrow: current wind direction at the boat's position
- The minimap is not interactive in the prototype (no clicking to set waypoints).

### Apparent Wind Angle Indicator
- A near-diegetic indicator placed in the 3D view close to the boat — not in a corner.
- Shows the angle of the wind relative to the boat's current heading.
- This is the primary real-time sail selection cue. It drives tacking decisions.
- Exact form: a small arc, needle, or streamer rendered in the world view. Determined during implementation.

### Actions available in-game
| Action | Input |
|---|---|
| Steer | A / D keys |
| Select sail config | 1 (Jib) / 2 (Genoa) / 3 (Spinnaker) |
| Open map screen | M key or map button |
| Enter dock zone | Sail into dock zone radius (automatic) |

---

## Overlay: Arrival [PROTOTYPE]

**How the player gets here:** Automatically on entering a settlement's dock zone while on a timed leg.

Displayed as a brief overlay on top of the In-Game view. Auto-dismisses after a few seconds, or immediately on any input.

### Contents
- Settlement name (e.g., "You have arrived at Gopher's Bay")
- Elapsed time for the completed leg
- **Personal best indicator:** "New personal best!" if the time beats the player's previous best on this route
- **Leaderboard position:** Raw rank among all stored local times (e.g., "#2 on this route")
- A prompt to open the Settlement Screen or continue sailing

### Notes
- If a run was abandoned (player re-entered departure settlement), no overlay is shown — the timer silently resets.
- Percentile rank is available on the full leaderboard inside the settlement, not shown here.

---

## Screen: Settlement Screen [PROTOTYPE]

**How the player gets here:** On entering any settlement's dock zone. The boat is considered docked.

A full-screen overlay replacing the sailing view. Three tabs: Merchant, Boat Yard, Leaderboard.

### Tab: Merchant [PROTOTYPE]

Displays all five goods with buy and sell prices for this settlement.

| Element | Description |
|---|---|
| Goods list | Name, buy price, sell price for each good |
| Player cargo | Current hold contents and available slots |
| Quantity selector | +/- control for how many units to buy or sell |
| Buy / Sell button | Confirms transaction; gold and cargo update immediately |
| Gold balance | Always visible, updates live |

- The player can buy and sell in any order, in any quantity, limited by cargo slots and gold balance.
- Selling a good the player doesn't have, or buying beyond slot capacity, is prevented — not penalised.
- No time limit. The player can sit in the merchant screen indefinitely.

### Tab: Boat Yard [PROTOTYPE]

Displays all six upgrade categories with current and upgraded values.

| Element | Description |
|---|---|
| Upgrade list | All six categories; each shows current multiplier, upgraded multiplier, cost |
| Purchase button | Available if player has sufficient gold and upgrade not yet purchased |
| Already purchased indicator | Clear visual if upgrade is already owned |
| Gold balance | Always visible |

- In the prototype, only Tier 1 upgrades exist. Purchased upgrades are permanently applied.
- All upgrades available at every settlement — no port-locked upgrades.
- Upgrades take effect immediately on purchase.

### Tab: Leaderboard [PROTOTYPE]

Shows times for routes arriving at this settlement.

| Element | Description |
|---|---|
| Route selector | List of all routes ending at this settlement |
| Top times | Stored times for the selected route, in order |
| Player's time | Most recent time highlighted |
| Personal best | Player's all-time best on this route |
| Raw rank | Position by time (e.g., "#2") |
| Percentile rank | Position among all stored times (e.g., "Top 12%") |

- Both raw rank and percentile are shown.
- In the prototype, all times are local only.

### Leaving the Settlement Screen
| Action | Result |
|---|---|
| Depart / close | Returns to In-Game view; timer begins when boat exits dock zone |
| Switch tabs | Stays inside settlement screen |

---

## Screen: Map Screen [PROTOTYPE]

**How the player gets here:** M key or map button while In-Game (sailing or docked).

**How the player leaves:** Same key/button, or a Close button.

A full-screen or large overlay showing the full game world.

### Contents
| Element | Description |
|---|---|
| World map | Full playable area with shoreline and island |
| Settlement markers | All settlements labelled by name |
| Player position | Current boat position |
| Dock zone rings | Visible circles around each settlement |
| Route leaderboard access | Tap/click a settlement to view its arriving routes and top times |

### Notes
- The map does not pause the world — wind and time continue while the map is open. The boat drifts.
- No waypoint setting in the prototype.
- Wind overlay (coarse grid of wind direction arrows across the map) is a desirable addition if straightforward to implement, otherwise deferred.

---

## Overlay: Tutorial Cards [PROTOTYPE]

Each card has:
- A brief title
- 1–3 sentences of instruction
- A dismiss button ("Got it" or similar)
- An optional arrow or highlight pointing to the relevant UI element

Cards do not block input. The player can sail, change sails, or interact with the HUD while a card is showing.

See Tutorial Overlay section above for the full card sequence.

---

## Deferred Screens [FULL]

The following screens are not built in the prototype but should be anticipated in the screen map:

| Screen | Notes |
|---|---|
| Settings | Sound volume, reset records, keybindings |
| Credits | Authorship |
| Global Leaderboard | Cross-player times; requires online infrastructure |
| World Activities UI | Fishing panel, whittling progress, instrument player — each activity introduces its own UI state within the in-game view |
| Tier 2–4 Boat Yard | Extended upgrade screen when multiple tiers exist |
| Messages in a Bottle (compose) | Text entry + gift selection interface |
| Floating Cargo recovery prompt | Brief overlay when a floating item is nearby |


```
┌─────────────────────────────────────────────┐
│ [Timer]                     [Compass Rose]  │
│ [Wind speed · True wind direction]          │
│                                             │
│                                             │
│            (game world)                     │
│                                             │
│                                             │
│ [Cargo · Money]              [Minimap]      │
│ [Sail config selector]                      │
└─────────────────────────────────────────────┘
```

| Corner | Contents | Notes |
|---|---|---|
| **Top-left** | Active timer, wind speed, true wind direction | Timer is the most urgent read; wind instruments sit with it since both inform tacking decisions |
| **Top-right** | Compass rose (with boat heading bearing) | Orientation tool; stacked above minimap on the same side |
| **Bottom-left** | Current cargo, money, sail configuration selector | Lower-urgency inventory; sail config is changed deliberately, not reactively |
| **Bottom-right** | Minimap (track line, wind arrow, settlement markers, dock zone rings) | Standard convention; pairs with compass above |

### Apparent Wind Angle

The apparent wind angle (boat-relative) is distinct from the true wind direction displayed in the top-left. Because this instrument is checked constantly during a sail \u2014 it drives the core decision of which sail to set and when to tack \u2014 it should be placed where the player's eye already is: near the boat on screen. Consider a small arc or needle indicator overlaid just above or below the boat in the 3D view (a near-diegetic element), rather than in a corner. Exact placement to be determined during implementation.

---


