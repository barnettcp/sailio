# Sailing Mechanics

This document defines how sailing works in Sailio: wind, boat movement, sail selection, and speed calculation. It is the most mechanically critical document in the project. Developers implementing boat physics should treat this as the primary reference.

The guiding principle is **arcade feel grounded in real sailing logic.** A player who has never sailed should find the boat intuitive. A player who has sailed should recognize every decision they make.

---

## Wind Model

### Spatial Wind Field

Wind is not uniform across the map. A 2D noise map covers the entire playable area. At any position $(x, z)$ on the map, the wind field yields:

- **Base wind direction:** a compass bearing in degrees (0–360°)
- **Base wind speed:** a value in knots (suggested prototype range: 8–20 kn)

The noise map is authored once and baked into the game. It gives each region of the map a consistent wind character — some passages will reliably favour upwind sailing, others will typically offer a reach. Players who know the map can use this to plan routes.

### Session Perturbation

At the start of each session, a small random offset is generated:

- **Direction offset:** uniform random in the range **±10°**, applied globally to the entire noise field
- **Speed offset:** uniform random in the range **±2 kn**, applied globally

This means the wind field is never identical between sessions but never wildly different. A player who knows a route will recognise it. A player trying to set a record time may find one session more favourable than another.

### Spatial Smoothing

As the boat moves through the field, wind direction and speed update **smoothly** by interpolating between sampled positions. Wind does not snap or jump. The player should feel the wind gradually shifting as they sail into a new region.

### Wind Display (HUD)

Two instruments are always visible:

| Instrument | What it shows |
|---|---|
| **True wind indicator** | Compass direction of wind at the boat's current position. Shown as an arrow on the minimap and/or compass rose on the HUD. |
| **Apparent wind angle** | Angle of wind relative to the boat's current heading. 0° = dead ahead, 90° = wind abeam, 180° = dead downwind. This is the primary instrument for sail selection. |
| **Wind speed** | Current wind speed at the boat's position, in knots. |

> **Design note:** True wind direction helps with navigation and route planning. Apparent wind angle is what a real sailor watches to decide when to tack or which sail to set. Both are shown because they serve different decisions.

---

## Boat Movement

### Heading and Steering

The player steers the boat by turning left or right (keyboard: `A`/`D` or arrow keys). The boat has a **maximum turn rate** — heading changes are not instantaneous. The turn rate is constant regardless of boat speed (simplified; no speed-dependent turning in prototype).

The boat always moves in the direction it is currently heading. There is no leeway (sideways drift) in the prototype. This simplification keeps controls readable.

### Speed Calculation

Boat speed at any moment is:

$$\text{speed} = \underbrace{B(\text{sail})}_\text{boat factor} \times \underbrace{f(\text{sail}, \theta)}_{\text{wind angle factor}}$$

The boat factor $B$ depends on which sail configuration is active. Only the upgrades relevant to that configuration are applied:

$$B(\text{jib}) = H \times M \times U \times W$$
$$B(\text{genoa}) = H \times M \times U \times W$$
$$B(\text{spinnaker}) = M \times S \times U \times W$$

Where:
- $H$ = Headsail upgrade multiplier *(jib and genoa configs only)*
- $M$ = Mainsail upgrade multiplier *(all configs)*
- $S$ = Spinnaker upgrade multiplier *(spinnaker config only)*
- $U$ = Hull upgrade multiplier *(all configs)*
- $W$ = Weight Reduction upgrade multiplier *(all configs)*
- $\theta$ = angle between the boat's heading and the true wind direction (0–180°)
- $f(\text{sail}, \theta)$ = wind angle factor for the active sail configuration at angle $\theta$

All upgrade multipliers are ≥ 1.0 (base = 1.0, upgraded = a value > 1.0). This means a Spinnaker upgrade has no effect on upwind or reaching performance, and a Headsail upgrade has no effect when the kite is flying. Upgrade choices are meaningful because of this.

Cargo has **no effect** on speed. The formula does not include a cargo term by design.

### The No-Go Zone

Sailing directly into the wind is not possible. Wind angles between **0° and ~40°** (40° either side of dead upwind) define the **no-go zone**. Inside this zone, the wind angle factor approaches zero — the boat makes minimal forward progress and rapidly loses speed.

This is the fundamental constraint of sailing. Players must tack (zigzag) to make progress upwind. The no-go zone is a feature, not a punishment — it is the source of the game's primary skill expression.

**Behaviour in the no-go zone:**
- The boat does not stop instantly — it carries momentum briefly before slowing
- There is no automatic bear-away or tack; the player must steer out themselves
- A subtle visual or audio cue (sails flapping, speed indicator dropping) should signal that the player has entered the no-go zone

---

## Sail Configurations

The player has three sail configurations. Only one is active at a time. Selecting a new configuration triggers a **switching delay** of approximately **2 seconds**, during which the boat continues at its previous speed before transitioning to the new configuration's performance curve.

The switching delay simulates crew work (furling one sail, setting another) and creates a small timing decision: is it worth switching now, or will the route change again before the switch completes?

| Configuration | Key | Optimal angle | Description |
|---|---|---|---|
| **Jib** | `1` | ~45° | Small headsail and main, close-hauled. Best for beating upwind. Headsail and Mainsail upgrades apply. |
| **Genoa** | `2` | ~90° | Large overlapping headsail and main. Best for reaching across the wind. Headsail and Mainsail upgrades apply. |
| **Spinnaker** | `3` | ~150° | Spinnaker deployed, main eased. Best for running downwind. Spinnaker and Mainsail upgrades apply. |

A sail configuration indicator is always shown on the HUD, including a brief visual state for "switching."

### Wind Angle Factor Table

The table below defines the wind angle factor $f(\text{sail}, \theta)$ at key angles. Intermediate values are smoothly interpolated. These are starting values for implementation — tuning should be expected during development.

| Wind Angle (θ) | Jib config | Genoa config | Spinnaker config |
|---|---|---|---|
| 0° (head to wind) | 0.00 | 0.00 | 0.00 |
| 20° (no-go zone) | 0.05 | 0.05 | 0.00 |
| 40° (edge of no-go) | 0.30 | 0.20 | 0.00 |
| 45° (close-hauled) | 1.00 | 0.60 | 0.10 |
| 60° (close reach) | 0.90 | 0.85 | 0.30 |
| 90° (beam reach) | 0.70 | 1.00 | 0.65 |
| 120° (broad reach) | 0.50 | 0.85 | 0.90 |
| 150° (broad reach / run) | 0.25 | 0.60 | 1.00 |
| 180° (dead downwind) | 0.15 | 0.40 | 0.85 |

> **Design note:** Dead downwind (180°) is slightly penalised even on the downwind config — jibing angles (slightly off dead downwind) are faster in real life too, which creates route-finding decisions.

The table is **symmetric** — a wind angle of 45° on the port side behaves identically to 45° on the starboard side.

---

## Tacking and Gybing

These are the two manoeuvres for changing the wind from one side of the boat to the other. They emerge naturally from the mechanics above but are worth describing explicitly.

**Tacking (turning through the wind, upwind):**
The player steers through the no-go zone to bring the wind from one side to the other. Speed drops sharply through the no-go zone. A well-executed tack loses less speed than a hesitant one (the player should minimise time in the no-go zone). This is the primary skill expression in upwind sailing.

**Gybing (turning through dead downwind):**
The player steers through 180° (dead downwind). There is no no-go zone on this side, so gybing is faster and smoother than tacking. Real gybing has its own risks (the boom swings violently), but in the prototype this is not modelled — gybing is simply a smooth turn through 180°.

---

## Controls Summary

| Input | Action |
|---|---|
| `A` / `←` | Turn left |
| `D` / `→` | Turn right |
| `1` | Select Jib (upwind) |
| `2` | Select Genoa (reaching) |
| `3` | Select Spinnaker (downwind) |

No throttle input exists. Speed is entirely determined by heading relative to wind and sail selection.

---

## Tuning Notes

The following values will require in-engine tuning during development. Defaults are reasonable starting points, not final values.

| Parameter | Suggested starting value | Notes |
|---|---|---|
| Max boat speed (base, no upgrades) | 8 kn | Relative to map scale |
| Turn rate | 30°/sec | Should feel responsive but not twitchy |
| No-go zone boundary | ±40° from upwind | Adjust based on feel |
| Sail switch delay | 2 seconds | Enough to matter, not frustrating |
| Wind speed range | 6–20 kn | Affects speed variance across the map |
| Session perturbation (direction) | ±10° | See Wind Model section |
| Session perturbation (speed) | ±2 kn | See Wind Model section |

Map scale and boat speed should be set together so that settlement-to-settlement crossings take **5–10 minutes** at average conditions with a base (unupgraded) boat.

