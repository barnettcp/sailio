# UI Flows & UX

## HUD Layout

The HUD uses **corner anchoring** — each corner owns one category of information. The centre of the screen is kept as clear as possible; that is where the boat is and where sailing happens.

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


