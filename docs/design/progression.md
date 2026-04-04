# Boat Progression & Upgrades

This document defines how players improve their boat over time: the upgrade categories, tiers, costs, and the mechanical effect of each upgrade. It is the reference for both the economy system (how much things cost) and the sailing system (what the multiplier values actually are).

---

## Design Intent

Upgrades are the primary reward loop. A player earns gold through trading, spends gold at a boat yard, and sails faster or carries more cargo as a result. Every upgrade is a meaningful decision because each one only applies to specific sailing configurations — there is no single "best" upgrade, only upgrades that suit the player's preferred routes and sailing style.

No upgrade penalises cargo. A player who upgrades Headsail and then sails a loaded boat upwind is faster than they were before, not slower than an empty boat.

---

## Upgrade Categories

Six upgrade categories exist. Each has a base value of **1.0** and each tier increases the multiplier. The prototype implements one upgrade tier per category.

| Category | Applies to configs | Effect | Base value |
|---|---|---|---|
| **Headsail** | Jib, Genoa | Upwind and reaching speed factor | 1.0 |
| **Mainsail** | Jib, Genoa, Spinnaker | Speed factor in all configurations | 1.0 |
| **Spinnaker** | Spinnaker | Downwind speed factor | 1.0 |
| **Hull** | All | Universal speed factor (reduced drag) | 1.0 |
| **Weight Reduction** | All | Universal speed factor (reduced drag) | 1.0 |
| **Cargo Capacity** | — | Maximum cargo hold size | 4 slots |

See [sailing-mechanics.md](sailing-mechanics.md) for how multipliers compose into the speed formula per configuration.

---

## Upgrade Tiers

### Prototype (1 tier per category)

In the prototype each upgrade has exactly two states: **base** and **upgraded**. There is no further progression.

| Category | Base | Tier 1 (upgraded) | Effect at Tier 1 |
|---|---|---|---|
| Headsail | 1.00 | 1.125 | +12.5% speed on Jib and Genoa configurations |
| Mainsail | 1.00 | 1.125 | +12.5% speed on all configurations |
| Spinnaker | 1.00 | 1.125 | +12.5% speed on Spinnaker configuration |
| Hull | 1.00 | 1.125 | +12.5% speed on all configurations |
| Weight Reduction | 1.00 | 1.125 | +12.5% speed on all configurations |
| Cargo Capacity | 4 slots | 6 slots | +50% cargo (12 units → 18 units) |

> **Combined effect at Tier 1:** With all five speed upgrades purchased, the four stacking multipliers (Headsail or Spinnaker, Mainsail, Hull, Weight Reduction) produce **1.125⁴ ≈ 1.60×** the speed of a base boat in ideal sail conditions. This is noticeable and rewarding in the prototype without being overwhelming. Later tiers will feel like refinements rather than step-changes.

### Full Game (4 tiers, deferred)

The full game targets four tiers per category. The curve is deliberately front-loaded: Tier 1 delivers the largest perceptible improvement, with each subsequent tier giving a smaller but still meaningful gain. The gap between Tier 3 and Tier 4 is intentionally narrow — top-tier upgrades are an endgame luxury, and skilled route-planning should remain competitive against a fully upgraded opponent.

All speed upgrade categories use the same multiplier per tier. Cost differentials (see below) handle the value difference between universal upgrades (Mainsail, Hull) and configuration-specific ones (Headsail, Spinnaker).

| Tier | Per-upgrade multiplier | Combined effect (4 stacked) | Notes |
|---|---|---|---|
| Base | 1.000 | 1.00× | Unupgraded boat |
| Tier 1 | 1.125 | ~1.60× | Prototype maximum; clearly noticeable |
| Tier 2 | 1.180 | ~1.95× | Mid-game target |
| Tier 3 | 1.220 | ~2.21× | Late-game |
| Tier 4 | 1.230 | ~2.29× | Endgame; marginal gain over Tier 3 |

The Tier 3→Tier 4 step (~+0.08× combined) is a deliberate design statement: at this level, a skilled player on a Tier 3 boat should still be able to challenge a fully upgraded opponent on a well-chosen route. Upgrades reward investment but do not eliminate skill expression.

These are targets for planning. Actual values require in-game tuning.

---

## Upgrade Costs

Upgrades are purchased with gold at any settlement's **boat yard**. All upgrades are available at all boat yards — there is no settlement-exclusive upgrade in the prototype.

### Prototype Costs

| Category | Tier 1 Cost | Notes |
|---|---|---|
| Headsail | 250 gold | First speed-oriented upgrade most players will buy |
| Mainsail | 300 gold | High value — applies to all configurations |
| Spinnaker | 250 gold | Most useful for experienced players who run downwind routes |
| Hull | 300 gold | Universal benefit; strong second purchase |
| Weight Reduction | 200 gold | Lowest cost; smallest gain; good first upgrade |
| Cargo Capacity | 175 gold | Lowest cost overall; strong early ROI for trade-focused players |

> **Tuning target:** A player starting with 150 gold and completing 2–3 early trade runs (earning roughly 150–250 gold per run) should be able to afford their first upgrade comfortably. Cargo Capacity and Weight Reduction are intentionally the cheapest — they are accessible early rewards that change how the player plays without dramatically affecting times.

### Full Game Cost Scaling (deferred)

In the full game, later tiers should cost significantly more, creating a progression arc that spans many play sessions. Suggested rough multipliers: Tier 2 costs 2.5× Tier 1; Tier 3 costs 5× Tier 1. Exact values are a tuning concern, not a design one.

---

## Boat Yard

The boat yard is available at every settlement. It presents:

- A list of all six upgrade categories
- For each: the current tier, the next tier's multiplier (or slot count for Cargo), the cost, and a **Buy** button
- Already fully upgraded categories are shown as such (no buy button)
- The player's current gold is always visible

In the prototype, all Tier 1 upgrades are available immediately from the start of the game. There are no unlock conditions, prerequisite purchases, or level gates.

> **Future:** Later tiers could require visiting a specific settlement's boat yard (e.g., only Lincolnston can apply Hull upgrades beyond Tier 2 because it has the best forge). This creates travel incentives beyond trade, but is firmly deferred.

---

## Persistence

All upgrade state is saved automatically when the player docks at any settlement. Upgrades are never lost between sessions. There is no prestige reset or season-based wipe of upgrades in the prototype.

> **Future:** A seasonal economy reset (wiping leaderboard records monthly) is planned. Upgrade state is **not** reset with records — they are separate save data concerns.

---

## Upgrade Interaction with Racing

Because upgrades directly affect boat speed, they affect route times. A player who upgrades Mainsail will naturally set faster times on every route. The leaderboard does not differentiate between upgrade levels — a time set on a fully upgraded boat competes directly with a time set on a base boat.

This is intentional. The leaderboard rewards both sailing skill and investment in the boat. A new player with a fast route knowledge can still compete with an upgraded player who takes a suboptimal line. Upgrades compress the gap over time but do not make the leaderboard inaccessible to new players.

