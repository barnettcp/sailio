# World & Map

This document describes the world of Sailio: its geography, visual character, scale, and how physical space is constructed both for the prototype and future versions.

---

## Design Intent

The world should feel **bright, open, and inviting**. Players spend most of their time on the water, and the water should be the dominant visual. Landmasses frame the sailing area and give it character, but they are scenery — the sea is the stage.

The world is **fictional but geographically grounded**. The prototype map is derived from a real lake's shape, simplified, rotated, and modified enough that it reads as invented. This is an intentional easter egg for players who recognise the origin — not something that needs to be called out.

---

## Prototype Map

### Source Geometry

The prototype map is derived from a **simplified shapefile of Lake Washington** (Seattle, WA), processed in Python using GeoPandas:

1. Load the lake shapefile
2. Simplify the polygon to reduce vertex count (target: a clean, game-suitable outline with no excess detail)
3. Rotate the shape to obscure its geographic origin
4. Trim the northern and southern ends so the water appears to open into a larger body — suggesting sea or sound beyond the playable boundary
5. Scale to match the intended in-game travel times (see Scale section below)

The resulting shape is the primary water boundary for the prototype. It does not need to look like Lake Washington — it should look like a plausible coastal inlet or channel.

### Shape Characteristics

The trimmed, rotated Lake Washington shape provides:
- A **long central channel** suitable for multi-settlement routes
- **Natural bays and inlets** where settlements can be placed with a sense of shelter
- **Open ends** that imply connection to a wider world (and allow future expansion)
- Enough variation in width to create passages that feel distinct from each other
- A **central island** (corresponding to Mercer Island) that divides the southern portion of the channel, creating two distinct passages and a natural settlement site

### Out-of-Bounds

The playable water area is bounded by the shoreline polygon. The open ends are also bounded by invisible barriers (or shallow-water shoals, visually — see Deferred).

**Prototype behaviour at the boundary:**
- The boat is stopped or deflected at the shoreline — it cannot run aground in the prototype
- The open ends of the map (where the lake was trimmed) are marked with a **floating dotted line** — a simple, readable signal that this is the edge of the current world, not a permanent coast
- No grounding physics, damage, or stranding in the prototype

> **Future:** The "end of the world" boundary should eventually feel like a larger world exists beyond it — sea mist, open ocean suggestion, or a sense of distance rather than a hard wall. The dotted line is a prototype placeholder for this concept. World boundary geometry should not be hardcoded as a static immovable polygon; tidal variation and navigational hazards are future features requiring a dynamic boundary. See [prototype/scope.md](../prototype/scope.md).

---

## Scale

The world is **not** real-world scale. It is compressed so that sailing feels active and routes feel achievable in a play session.

| Route type | Target travel time (base boat, average wind) |
|---|---|
| Short (close neighbours) | 1–3 minutes |
| Standard (most routes) | 5–10 minutes |
| Long (end-to-end crossing) | 10–15 minutes |

Map scale, boat speed, and world size should be tuned together during implementation. The authoritative target is **travel time**, not a geographic unit. Start with travel time and work backwards to determine the map's physical dimensions in Godot world units.

> See [sailing-mechanics.md](sailing-mechanics.md) Tuning Notes for the suggested base boat speed of 8 kn and how this relates to map scale.

---

## Islands

The prototype map includes at least one significant island corresponding to the **Mercer Island** feature of the source lake geometry. This island is represented as a **hole in the water polygon** — land that divides the channel into two distinct passages.

- The island creates meaningful route decisions: which side to take, and whether one side offers a better wind angle
- The island is a candidate site for one of the 4–5 prototype settlements
- The island's passages should have noticeably different widths, giving each a distinct character

Future maps may include additional smaller islands, rocks, and navigable gaps as the world grows.

---

## Navigational Landmarks

The world includes permanent landmarks that help players orient without consulting the minimap. These are visible from the water and distinctive enough to be recognisable at a glance.

### Settlement Landmarks (Prototype)

Each of the five prototype settlements has a defined primary landmark — the visual feature that makes it identifiable from the water at distance. These are specified in [prototype/settlements.md](../prototype/settlements.md). Examples from the prototype:

| Settlement | Landmark |
|---|---|
| **Rockaway** | Long timber dock extending into the bay, with market tents at the end |
| **Woollie** | Windmills on the island high ground, blades turning |
| **Lincolnston** | Wide foundry headquarters building with a chimney stack |
| **Greenway** | Giant old-growth trees, tallest natural feature on the map |
| **Gopher's Bay** | Tall chart house on the waterfront, distinctive and colourful |

Landmarks are **static scene objects** — placed by hand, not generated. They do not have gameplay function in the prototype.

**Landmark design guidance for future settlements:** Landmarks should be imaginative and specific to the settlement's character. The goal is that a player seeing a landmark for the first time can make an educated guess about what kind of place they are approaching. A windmill, a smokestack, a giant tree all communicate something about the settlement's identity. Prefer landmark types that are novel to this world and its tone over generic maritime structures.

> **Future:** Landmarks may eventually have mechanical associations — a race turning mark, a settlement expansion, a point of interest tied to world activities.

### Buoys

Buoys serve both visual and functional roles. They are placed by hand (or procedurally with manual override) and fall into three categories:

**Channel/shipping lane buoys:**
Large utility buoys in the centre of the main channel, marking rotation points similar to the yellow buoys used in Puget Sound shipping lanes. 1–2 in the prototype. Purely visual in the prototype.

**Decorative settlement buoys:**
Small buoys near each settlement for visual dressing and a sense of place. Purely decorative in the prototype — they do not serve as entry triggers (see Dock Zones below).

> **Future:** Buoys are intended to eventually be functional. Players may choose to cut inside a navigational buoy to shorten a route, at the risk of grounding in shallower water — requiring water depth, shoals, and grounding mechanics, all deferred. Red/green entrance buoys and larger race-course buoys (round-the-buoy racing) are Tier 3 concepts.

---

## Dock Zones

Each settlement has two concentric circular zones centred on the settlement's pier or anchorage.

### Harbour Zone (inner)

The harbour zone is the inner, smaller zone representing the settlement itself:

- **Docking is automatic.** The arrival timer stops and the time is recorded the moment the boat crosses this ring inward. No player confirmation is required.
- A brief **cooldown window** (suggested: 5–10 seconds) begins on zone entry. During this window the player can sail back out without committing to a docking interaction — useful if the player entered the zone incidentally while passing through.
- If the boat remains in the zone after the cooldown expires, the Settlement Screen opens automatically.
- The player can also open the Settlement Screen manually at any point while inside the harbour zone.
- Both zones have global radius defaults, independently tunable per settlement where geography requires it.

### Start Line (outer)

The start line is the outer, larger zone defining the race boundary:

- The **timer starts** when the boat crosses this ring **outward** on departure.
- The start line has no effect on inbound crossings — it is transparent to an arriving boat. Only the harbour zone records arrival.
- Sized to give the player comfortable room to set heading and sail configuration before the clock begins.

### Minimap Representation

On the minimap each settlement marker shows a **filled inner circle** (harbour zone) with a **thin outer ring** (start line) — together they read as a single marker with two distinct interaction boundaries.

This approach is deliberately arcade-friendly. No threading through buoy gates, no precise manoeuvring into a slip. You sail to a settlement, you arrive. The skill is in getting there fast, not docking.

> **Future:** Dock zone shape may eventually be replaced or supplemented by a more detailed harbour entry (breakwater, channel, timed approach). Red/green buoys at harbour entrances are a possible future embellishment, but will remain visual-only unless grounding mechanics are implemented.

See [design/racing-and-timers.md](racing-and-timers.md) for full timer behaviour and [design/settlements.md](settlements.md) for per-settlement zone sizing guidance.

---

## Camera & Map Orientation

- The map has a **fixed North**. North is consistent between the minimap, the full map screen, and the wind compass.
- A **compass rose** is displayed in the **top-right corner** of the HUD at all times, showing true North and the player's current heading.
- The player's **current heading** is always visible, either as part of the compass rose or as a bearing readout alongside it.
- **In the prototype:** the camera angle and minimap are fixed — no rotation, no panning. The world is viewed from a consistent fixed angle. The **minimap is in the bottom-right corner**.
- **Future:** the player will be able to pan the camera and optionally lock it to follow the boat. The minimap may optionally rotate to boat-relative orientation as a player setting.

> **HUD layout:** The compass rose (top-right) and minimap (bottom-right) are intentionally stacked on the same side — both are orientation tools and grouping them keeps the left side of the screen clear for the timer, wind instruments, and cargo info. See [ux/ui-flows.md](../ux/ui-flows.md) for the full HUD layout specification.

---

## Settlements

**Five settlements** are placed on the prototype map. Settlement placement follows these principles:

- Each settlement sits on a **natural feature** — a sheltered bay, a headland, the mouth of a channel, a wider basin
- No two settlements should feel physically identical; each position should have a distinct read on the minimap and from the water
- Placement creates **a variety of route characters**: some routes are a straight reach, some require tacking up a channel, some pass through a narrows
- Settlements are spaced to produce the target travel times above — not evenly distributed if uneven spacing makes for better routes

The five prototype settlements and their map positions, landmarks, and economic roles are defined in [prototype/settlements.md](../prototype/settlements.md).

---

## Terrain & Landmass

### Prototype

Land in the prototype is **minimal but present**. The goal is to establish the geography without building art assets for it.

- Land is represented by a simple mesh filling the non-water polygon
- A **heightmap** (greyscale image) drives basic elevation on the landmass. The prototype heightmap is generated from **Perlin noise** (or equivalent), scaled so that elevations feel geographically plausible — gentle coastal lowlands rising to modest hills inland. Manual tweaks can be applied on top of the noise base to shape specific areas (e.g., a prominent headland, a flat bay shore near a settlement).
- The goal is a believable silhouette when viewed from the water, not detailed terrain art
- No vegetation, no beach material variation, no detailed terrain features in the prototype
- Land colour is a flat or very simply shaded neutral tone (grey, muted green, or sand — subject to art direction)
- The water–land edge (shoreline) should be clean and readable

> **Why include a heightmap at all in the prototype?** Because implementing a flat-land prototype and then adding a heightmap later requires reworking the terrain mesh. Including a basic heightmap in the prototype — even a very simple noise-driven one — means the infrastructure exists for polish without a rebuild.

### Future Terrain

Post-prototype terrain development is planned in two layers:

**Manual landscape assignment:**
- Beach zones along the shoreline
- Rocky headlands and cliffs
- Hillside and mountain areas
- Flat lowland/marsh areas near settlement sites

**Procedural elements (placed within manually assigned zones):**
- Trees and forest cover (density by zone type)
- Rocks and boulders
- Dock structures and harbour detail near settlements

The terrain system should be designed so that manual zone masks and procedural generation are separate concerns — artists assign zones, the engine populates detail within those zones.

> **Note on buoys:** Buoys near settlements are intentionally **not** treated as purely procedural terrain detail. They have gameplay significance (entry/exit gates, future grounding risk) and must be individually placeable and adjustable. See the Buoys section above.

---

## Future World Expansion

The prototype map is explicitly a **contained proof of concept**, not the final world. Two future directions are under consideration:

### Option A — Expanded Fictional Lake
Grow the lake-derived map with more settlements, more interesting geography (islands, narrows, open crossings), and added landmass detail. Remains entirely fictional.

### Option B — Puget Sound Coastline
Replace or supplement the prototype map with a simplified version of the Puget Sound coastline. This provides:
- Rich natural geography with real place-name inspiration
- Multiple distinct sailing characters (protected south sound vs. open central sound vs. narrow passages)
- Enough area for a large number of settlements without feeling sparse
- A real-world easter egg for players familiar with the Pacific Northwest

Both options are non-exclusive and could coexist as different regions. No commitment is made in the prototype — but the world architecture (scale system, boundary polygon, settlement definition format) should not make either option harder to implement.

### Real-Time Multiplayer
Future versions may show other players' live positions on the map. This is an architectural concern primarily for the backend and netcode, not the world design — but the world should not make any assumptions about being single-player only. Player position is a property of a session, not of the world itself.

---

## Visual Character

- **Time of day:** Always daytime in the prototype. Day/night cycle is deferred.
- **Atmosphere:** Bright, clear, no fog or storm effects in prototype. Suggests a sunny day on calm-ish water.
- **Water material:** Flat colour or a simple animated shader (gentle UV-scrolled normal map). No deep simulation.
- **Horizon:** Because the camera is fixed at an angle, the "horizon" is the far edge of the map. Landmasses or the boundary should be visible to give spatial orientation.
- **Colour palette:** Deferred to [art/direction.md](../art/direction.md), but warm and saturated is the intent — this world should feel like a postcard.

---

## Technical Notes for Implementation

- The water boundary polygon is imported from a processed shapefile (GeoJSON or similar) and converted to a Godot collision shape and mesh
- The boundary polygon should be stored as a data asset (not hardcoded geometry) so it can be replaced or scaled without code changes
- The heightmap for land terrain is a greyscale PNG, applied to a subdivided plane mesh clipped to the land polygon
- Settlement positions are defined as named 2D coordinates in a data file (JSON or GDScript resource), not hardcoded in scene nodes — this makes it straightforward to adjust layout without touching the scene tree

