# Settlements & Ports

This document defines the design rules that govern all settlements in Sailio — what every settlement must contain, how economy roles work, how settlements are placed in a world, and how the design should scale as new maps are added. Specific settlements are defined in map-specific documents (e.g., [prototype/settlements.md](../prototype/settlements.md)).

---

## Design Intent

Settlements are the **anchors of the world**: the places players depart from, arrive at, and mentally navigate between. Every settlement must be immediately recognisable from the water, clearly distinct from every other settlement, and feel like a place that exists for reasons independent of the player.

A settlement is not just a trading post. It is a **character** — it has a type, a personality, a visual identity, and an economic role that shapes its relationship to every other settlement on the map.

To whatever extent makes sense, the area immediately surrounding the settlement should reflect the settlement's specific character. For example, a weaving port should have more grassland than trees, and a forest town should have more trees than grassland.

---

## Required Components

Every settlement, in every version of the game, must have all of the following:

### 1. Dock Zone
A circular trigger area on the water, centred on the settlement's pier or anchorage. The dock zone:
- Starts and stops the route timer on entry and exit
- Triggers the merchant UI when entered
- Is visible on the minimap as a distinct icon
- Has a default radius that is tunable globally; individual settlements may override this radius where geography requires it (e.g., a bay settlement whose default zone would protrude beyond a peninsula into open water)

See [racing-and-timers.md](racing-and-timers.md) for timer behaviour on dock zone entry/exit.

### 2. Visual Landmark
One primary visual feature that makes the settlement identifiable from the water at a distance. The landmark should be:
- **Unique** — no two settlements in the same map should share a landmark type
- **Tall or prominent** — visible before the dock zone is reachable
- **Thematically consistent** with the settlement's type (e.g., a chart house for a navigator's outpost, a smokestack for a foundry harbour)
- **Geared towards personality and character**, and not necessarily towards an actual function. A Foundry does not necessarily have to have a landmark related to mining, for example.

The landmark is the player's primary navigation aid. Signage and UI labels are secondary.

### 3. Merchant
Every settlement has exactly one merchant. The merchant:
- Buys and sells all five goods (in the prototype; expandable later)
- Displays buy and sell prices for all goods
- Allows partial quantity selection (player chooses how many units to buy or sell)
- Is accessed via the dock zone UI, not by navigating a character to an NPC

See [economy.md](economy.md) for the full price model and goods list.

### 4. Boat Yard
Every settlement offers upgrades. The player can purchase any available upgrade tier from any settlement — they are not locked to a specific port for specific upgrades.

> **Rationale:** Forcing players to upgrade only at designated ports creates arbitrary routing constraints and punishes players who are mid-route. The boat yard is ambient infrastructure, not a destination mechanic.

See [progression.md](progression.md) for upgrade tiers and costs.

### 5. Name and Economic Role
Every settlement has:
- A **name** — memorable, distinct, and appropriate to a maritime world
- An **economic role** — drawn from the Economy Role Taxonomy below — which determines what the settlement produces, what it wants, and informs its visual style and personality

---

## Economy Role Taxonomy

Every settlement has exactly one **primary economy role**. The role determines which good it produces (selling cheap) and, by implication, which goods it needs (buying expensive). A settlement that produces Rope does not need Rope; it will likely need something it cannot make.

The five roles defined in the prototype are the canonical starting set. New roles can be added for future maps, but each role must:
- Produce exactly one good at a discount
- Have a plausible real-world analogue (coastal industry, trade post, etc.)
- Feel visually distinct from existing roles

| Role | Produces | Typical Wants | Visual character |
|---|---|---|---|
| Fishing Village | Rope | Charts | Weathered docks, nets, small boats |
| Navigator's Outpost | Charts | Rope | Chart house, flags, instruments |
| Weaving Port | Sailcloth | Timber | Looms visible, colourful textiles |
| Foundry Harbour | Ironwork | Sailcloth | Industrial, smoke, forge sounds |
| Forest Town | Timber | Ironwork | Trees, sawmill, lumber stacks |

"Wants" in this table are typical — final per-settlement prices are defined in the map-specific settlement document, not derived automatically from role. The role is a strong prior, not a constraint.

### Neutral Goods
For every good a settlement neither produces nor heavily wants, its price is near baseline (±10%). A settlement's price modifiers for all five goods must be set explicitly in the map-specific document; the role only informs the designer's starting point.

---

## Placement Principles

When placing settlements on a new map, the following principles should guide decisions:

### Spacing
- No two settlements should be so close that the route between them takes less than **1 minute** at base boat speed. Routes shorter than 1 minute do not generate meaningful timer competition.
- At least one route on every map should require **10+ minutes** — a long crossing that rewards route-finding, sail selection, and attentive boat handling.
- The distribution of distances should create a range of route difficulties. Not every route should be a medium crossing.

### Navigational Interest
- Routes between settlements should pass through or near at least one navigational feature (island, narrow channel, headland). Straight open-water crossings are acceptable but should not be the majority.
- Each settlement should be reachable from at least two other settlements via meaningfully different paths.

### Visual Separation
- No two settlements should be visually similar from the water. The combination of landmark + shoreline shape + approach angle should make each port instantly distinguishable at play distance.
- Settlements should not be clustered on the same stretch of shore if possible. Aim for variety in compass bearing from any given port.

### Map Coverage
- Settlements should be distributed across the map such that travel to the most distant settlement from any starting point is roughly the same in all directions. A map should not have a "rich corner" where all settlements are densely packed and a "dead corner" with nothing.
- As a rough guide: one settlement per distinct geographic feature (bay, island, inlet, headland) on the map. This keeps placement feeling motivated rather than arbitrary.

### Minimum Viable Count
A map needs **at least 4 settlements** to generate a meaningful economy. With 4 settlements:
- There are 12 directional routes
- There are enough produce/wants relationships to create non-trivial cargo routing decisions
- The leaderboard has enough routes to feel competitive

The prototype's 5 settlements across a map with a 10–15 minute maximum crossing is the reference density. Larger maps should scale settlement count proportionally — a map where the longest crossing is 30 minutes should support roughly 8–10 settlements to maintain the same sense of activity and route variety.

---

## Minimum Viable Design Checklist

Before a settlement can be considered complete and ready for implementation, the following must be defined in the map-specific settlement document:

- [ ] Name and economic role
- [ ] Map position (described relative to geography, not pixel coordinates)
- [ ] Dock zone anchor point (the pier or anchorage the dock zone centres on)
- [ ] Visual landmark (name, description, and approximate scale)
- [ ] Economy role (from taxonomy above, or new role defined with full rationale)
- [ ] Price modifiers for all goods (as a price table, not just percentages)
- [ ] Personality note (2–4 sentences describing tone, appearance, and feel)
- [ ] Any special gameplay role (start, tutorial destination, etc.)

A settlement without all of the above is not ready for a developer to build.

---

## Scaling Notes for Future Maps

As the game expands beyond the prototype:

- **New map regions** (e.g., a Puget Sound expansion) should be treated as self-contained trading worlds that connect at border points. Cross-region routes should exist but be longer and carry proportionally higher trade value.
- **New economy roles** can be added per region. Roles are not globally unique — a second Forest Town can exist in a new region, but it should feel visually and culturally distinct from the first.
- **Settlement count per region** should be enough to keep prices competitive and routes diverse. A region with fewer than 4 settlements should be treated as a transitional area, not a full trading zone.
- **World tiling:** If the world map becomes large enough to require tiled loading, settlement positions should be defined in world-space coordinates from the start, not as offsets relative to a single map. This is a technical concern for [world.md](world.md) and future rendering documentation, not a settlement design concern.
