# Game Assets — 3D Model Checklist

This document lists every 3D asset needed for the prototype, organized by priority tier. Each tier builds on the last — the game is playable at P0, recognizable at P1, populated at P2, and polished at P3.

All models are built in Blender and exported as `.glb` (binary glTF) for Godot 4.

---

## Guiding Principles

- **Model for the camera.** The game uses a fixed isometric-style camera. Players never see assets up close. Silhouette readability matters more than surface detail.
- **Gray-box first.** Every asset starts as a rough blockout. Get it into Godot, confirm scale and framing, then refine. Do not detail-pass anything until the world is playable.
- **Solid colors over textures.** Vertex colors or flat materials suit the cartoonish aesthetic and avoid texture production overhead. Textures are a future polish step, not a prototype requirement.
- **Stay low-poly.** The game targets HTML5 export (WebGL). Keep tri counts conservative and batch materials where possible.
- **Separate meshes for moving parts.** Sails, windmill blades, and anything that may animate should be exported as distinct meshes, not baked into their parent.

---

## Poly Budget Guidelines

These are rough targets for the prototype, not hard limits. When in doubt, go lower — the fixed camera is very forgiving.

| Asset type | Target tri count | Notes |
|---|---|---|
| Sailboat hull | 200–500 | Symmetry-modeled. Exaggerate proportions for readability. |
| Individual sail | 50–150 | Quads only (may receive vertex animation later). |
| Landmark building | 200–600 | One per settlement. Most detailed structure. |
| Kit building | 100–400 | Reusable across settlements with color/scale variation. |
| Tree | 50–200 | Billboard or low-poly. 2–3 variants. |
| Rock | 30–100 | Deformed primitives. 2–3 variants. |
| Dock / pier section | 50–200 | Modular pieces that tile or extend. |
| Water surface | Driven by shapefile | Keep simple — shader does the visual work. |
| Terrain / shoreline | As low as possible | Only visible shoreline matters. Decimate aggressively. |

---

## P0 — Playable

The minimum set of assets to test the core loop: sail, trade, time, rank. Everything here can be a gray-box primitive.

| # | Asset | Count | Notes |
|---|---|---|---|
| 1 | Water surface mesh | 1 | Derived from shapefile. Flat or near-flat mesh; visual quality comes from shader. |
| 2 | Boat hull | 1 | Simple low-poly hull. Symmetry-modeled in Blender. Chunky, readable shape. |
| 3 | Jib sail | 1 | Small triangular headsail. Separate mesh, mountable on hull. |
| 4 | Genoa sail | 1 | Larger headsail. Same mount point as jib, toggled by game logic. |
| 5 | Spinnaker sail | 1 | Large, bulging downwind sail. Visually distinct silhouette from jib/genoa. |
| 6 | Mainsail | 1 | Always visible when any sail is flying. Mounted on the mast. |
| 7 | Mast and boom | 1 | Cylinder + horizontal spar. Rigging lines are not needed at this stage. |
| 8 | Dock zone marker | 5 | One per settlement. A simple platform or float the player sails into. Serves as the arrival trigger. |
| 9 | Settlement placeholder | 5 | One per settlement. A colored box or flag on shore to mark each port's location. |

**Milestone:** Player can sail between colored boxes, trigger docking, and test the full game loop.

---

## P1 — Recognizable

Settlements become visually distinct. Each one gets its landmark and basic terrain context.

| # | Asset | Count | Notes |
|---|---|---|---|
| 10 | Rockaway — long timber dock | 1 | Extended pier with simple market tents (colored shapes) at the end. |
| 11 | Gopher's Bay — chart house | 1 | Tall, distinctive building on the waterfront. Bright colors. |
| 12 | Woollie — windmill | 1–2 | Blades as a separate mesh for future rotation. Placed on island high ground. |
| 13 | Lincolnston — foundry HQ | 1 | Wide, dark-roofed building with chimney stack. Smoke is a future particle effect (P3). |
| 14 | Greenway — giant tree | 1–2 | Oversized old-growth tree(s). Tallest natural feature on the map. |
| 15 | Terrain / shoreline mesh | 1 | Covers visible land around all settlements. Derived from shapefile or hand-modeled. Vertex-colored (grass, dirt, sand). |
| 16 | Basic ground plane | 1 | Seabed or sub-water plane if needed for depth illusion beneath the water shader. |

**Milestone:** Each settlement is identifiable by its landmark from mid-water. The world has shape.

---

## P2 — Populated

The world feels inhabited. Reusable kit pieces fill out settlements and the environment.

| # | Asset | Count | Notes |
|---|---|---|---|
| 17 | Kit house — small | 1 | Box with pitched roof. Recolor per settlement. |
| 18 | Kit house — tall | 1 | Narrow and vertical. Recolor per settlement. |
| 19 | Kit house — wide | 1 | Low and broad, awning or porch. Recolor per settlement. |
| 20 | Kit shop / market stall | 1 | Open-front structure or tent. Used near docks. |
| 21 | Dock / pier section | 1–2 | Modular. Tiled to build docks of varying length per settlement. |
| 22 | Tree — standard | 1 | Sphere-on-cylinder deciduous. General use. |
| 23 | Tree — conifer | 1 | Cone-on-cylinder pine. Greenway and general use. |
| 24 | Tree — tall variant | 1 | For Greenway's old-growth forest canopy filler. |
| 25 | Rock — small | 1 | Deformed sphere. Scatter with MultiMeshInstance3D. |
| 26 | Rock — medium | 1 | Deformed cube. Shoreline and shallow-water placement. |
| 27 | Rock — large | 1 | Landmark-scale. Placed individually. |

**Milestone:** Settlements feel like places, not markers. Shorelines have detail. The world is worth looking at.

---

## P3 — Polished

Charm and personality. None of this is required for the prototype to ship, but it makes the world feel alive.

| # | Asset | Count | Notes |
|---|---|---|---|
| 28 | Windmill blade animation | — | Woollie's windmill blades rotate. Driven by AnimationPlayer or script. |
| 29 | Foundry smoke | — | Particle effect on Lincolnston chimney. Not a mesh — Godot GPUParticles3D. |
| 30 | Flags / banners | 1–2 | Small cloth pieces on poles. Vertex-animated or static. Settlement color-coded. |
| 31 | Gopher's Bay colored facades | — | Painted building variants or vertex color variation on kit houses for the bay. |
| 32 | Boat wake | — | Particle trail or shader behind boat while moving. Not a mesh. |
| 33 | Sail billow animation | — | Vertex displacement on sail meshes based on wind. Shader or blend shapes. |
| 34 | Crates / barrels (cargo props) | 1–2 | Visible on deck when cargo hold is loaded. Simple shapes. |

**Milestone:** The world has motion and personality. Settlements feel distinct beyond just landmarks.

---

## Blender Workflow Notes

- **Use symmetry (Mirror modifier)** for the boat hull. Apply before export.
- **Keep sails as quads.** They may receive vertex animation or blend shapes — tris complicate that.
- **Name meshes clearly** in Blender. Godot inherits node names from the `.glb` — `hull`, `sail_jib`, `sail_genoa`, `sail_spinnaker`, `sail_main`, `mast`.
- **Set origins deliberately.** Sail origins should be at their attachment point (mast head, tack) so rotation and toggling works naturally in Godot.
- **Merge by distance and delete loose geometry** before export.
- **No interior faces.** If a building has no interior, remove the floor polygon.
- **One material per color** is fine for prototype. A shared color palette material (vertex colors or a small texture atlas) is better for draw call batching if performance becomes an issue.

---

## Godot Integration Notes

- Import `.glb` files directly into the Godot project. Godot generates `.scn` or `.res` caches automatically.
- Use **`MultiMeshInstance3D`** for instanced scenery (trees, rocks) to minimize draw calls.
- Use **`MeshInstance3D` visibility** (`visible = true/false`) to toggle sail configurations rather than swapping scenes.
- Collision shapes for settlements and boundaries should be **simple convex shapes**, not the visual mesh.
- The water mesh does not need collision — the boat's movement is constrained by game logic and boundary zones.
