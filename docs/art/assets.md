# Game Assets — Checklist

This document lists all assets needed for the prototype, organized by priority tier. 3D models are built in Blender and exported as `.glb` (binary glTF) for Godot 4. 2D UI assets are created in Inkscape and exported as `.svg` or `.png`. Each tier builds on the last — the game is playable at P0, recognizable at P1, populated at P2, and polished at P3.

See [art/direction.md](direction.md) for the governing color palette, shape vocabulary, and visual tone that applies to all assets in this document.

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
| 8 | Harbour zone marker | 5 | One per settlement. Inner arrival zone. A simple floating platform or painted buoy ring. Triggers the arrival overlay and merchant access. Hue varies per settlement — see direction.md. |
| 9 | Start line marker | 5 | One per settlement. Outer departure boundary. Two posts or buoys framing the line the player crosses to start a timer leg. Visually lighter than the harbour marker — a suggestion, not a barrier. |
| 10 | Settlement placeholder | 5 | One per settlement. A colored box or flag on shore to mark each port's location. |

**Milestone:** Player can sail between colored boxes, trigger docking, and test the full game loop.

---

## P1 — Recognizable

Settlements become visually distinct. Each one gets its landmark and basic terrain context.

| # | Asset | Count | Notes |
|---|---|---|---|
| 11 | Rockaway — long timber dock | 1 | Extended pier with simple market tents (colored shapes) at the end. |
| 12 | Gopher's Bay — chart house | 1 | Tall, distinctive building on the waterfront. Bright colors. |
| 13 | Woollie — windmill | 1–2 | Blades as a separate mesh for future rotation. Placed on island high ground. |
| 14 | Lincolnston — foundry HQ | 1 | Wide, dark-roofed building with chimney stack. Smoke is a future particle effect (P3). |
| 15 | Greenway — giant tree | 1–2 | Oversized old-growth tree(s). Tallest natural feature on the map. |
| 16 | Terrain / shoreline mesh | 1 | Covers visible land around all settlements. Derived from shapefile or hand-modeled. Vertex-colored (grass, dirt, sand). |
| 17 | Basic ground plane | 1 | Seabed or sub-water plane if needed for depth illusion beneath the water shader. |

**Milestone:** Each settlement is identifiable by its landmark from mid-water. The world has shape.

---

## P2 — Populated

The world feels inhabited. Reusable kit pieces fill out settlements and the environment.

| # | Asset | Count | Notes |
|---|---|---|---|
| 18 | Kit house — small | 1 | Box with pitched roof. Recolor per settlement. |
| 19 | Kit house — tall | 1 | Narrow and vertical. Recolor per settlement. |
| 20 | Kit house — wide | 1 | Low and broad, awning or porch. Recolor per settlement. |
| 21 | Kit shop / market stall | 1 | Open-front structure or tent. Used near docks. |
| 22 | Dock / pier section | 1–2 | Modular. Tiled to build docks of varying length per settlement. |
| 23 | Tree — standard | 1 | Sphere-on-cylinder deciduous. General use. |
| 24 | Tree — conifer | 1 | Cone-on-cylinder pine. Greenway and general use. |
| 25 | Tree — tall variant | 1 | For Greenway's old-growth forest canopy filler. |
| 26 | Rock — small | 1 | Deformed sphere. Scatter with MultiMeshInstance3D. |
| 27 | Rock — medium | 1 | Deformed cube. Shoreline and shallow-water placement. |
| 28 | Rock — large | 1 | Landmark-scale. Placed individually. |
| 29 | Woollie sheep | 2–4 | Low-poly grazing sheep for Woollie's shoreline. Distinct silhouette. Static for prototype; may animate later. |

**Milestone:** Settlements feel like places, not markers. Shorelines have detail. The world is worth looking at.

---

## P3 — Polished

Charm and personality. None of this is required for the prototype to ship, but it makes the world feel alive.

| # | Asset | Count | Notes |
|---|---|---|---|
| 30 | Windmill blade animation | — | Woollie's windmill blades rotate. Driven by AnimationPlayer or script. |
| 31 | Foundry smoke | — | Particle effect on Lincolnston chimney. Not a mesh — Godot GPUParticles3D. |
| 32 | Flags / banners | 1–2 | Small cloth pieces on poles. Vertex-animated or static. Settlement color-coded. |
| 33 | Gopher's Bay colored facades | — | Painted building variants or vertex color variation on kit houses for the bay. |
| 34 | Boat wake | — | Particle trail or shader behind boat while moving. Not a mesh. |
| 35 | Sail billow animation | — | Vertex displacement on sail meshes based on wind. Shader or blend shapes. |
| 36 | Crates / barrels (cargo props) | 1–2 | Visible on deck when cargo hold is loaded. Simple shapes. |
| 37 | Campfire / lantern props | 2–4 | Settlement atmosphere props. **Must include a separate emissive mesh** for each glowing element — required for the future day/night lighting pass. Plan this at modeling time. See direction.md. |

**Milestone:** The world has motion and personality. Settlements feel distinct beyond just landmarks.

---

## 2D / UI Assets

These assets are created in Inkscape and exported as `.svg` or `.png`. See the Inkscape Workflow Notes section below for sizing, format, and export guidance.

### P0 — HUD Essentials

| # | Asset | Format | Notes |
|---|---|---|---|
| U1 | Wind direction indicator | SVG | Arrow or needle showing current wind angle relative to the boat. Displayed in the HUD. |
| U2 | Sail config buttons (×3) | SVG | One icon each for Jib, Genoa, and Spinnaker. Tappable. Active and inactive states required. |
| U3 | Cargo slot grid | SVG | Grid of cargo hold slots with filled and empty states. |
| U4 | Timer display panel | SVG | Background panel for the active leg timer. Includes a best-time comparison indicator. |
| U5 | Minimap settlement dots | SVG | Per-settlement icons: filled inner circle (harbour zone) + thin outer ring (start line). One color variant per settlement. |

### P1 — Navigation and Arrival

| # | Asset | Format | Notes |
|---|---|---|---|
| U6 | Compass rose | SVG | Decorative compass for the HUD corner. Fixed orientation with N/S/E/W markers. |
| U7 | Arrival overlay panel | PNG | Panel shown on docking: port name, elapsed time, leaderboard position. |
| U8 | Settlement screen tabs (×3) | SVG | Merchant / Boat Yard / Leaderboard tab buttons with active and inactive states. |

### P2 — Map and Tutorial

| # | Asset | Format | Notes |
|---|---|---|---|
| U9 | Map screen overlay | PNG | Full map view with settlement labels. Settlement dots reuse the minimap icons. |
| U10 | Tutorial card frame | PNG | Background card shape for the 7-step tutorial overlay sequence. |
| U11 | Main menu buttons | SVG | New Game, Continue, Settings buttons with active and hover states. |

---

## Inkscape Workflow Notes

- **Work in pixels.** Set document units to `px` (File > Document Properties). This keeps artboard sizes 1:1 with Godot's 2D coordinate space.

**Recommended artboard sizes by asset type:**

| Asset type | Artboard size | Export format | Notes |
|---|---|---|---|
| HUD icon / button | 128 × 128 px | SVG or PNG | For PNG, export at 2× (256 × 256) for high-DPI sharpness. |
| Minimap settlement dot | 32 × 32 px | SVG | Scales cleanly as a live vector in Godot. |
| Settlement icon (map screen) | 64 × 64 px | SVG | |
| Full-screen overlay / panel | 1920 × 1080 px | PNG | Bake to PNG if the panel contains drop shadows, blurs, or complex fills. |
| Tutorial card | 900 × 600 px | PNG | |

- **SVG vs PNG.** Use SVG for icons and flat-fill shapes that need to scale cleanly. Use PNG for panels, overlays, or anything with drop shadows, blurs, or raster effects that do not render predictably as a live SVG in Godot.
- **Godot SVG import.** When importing `.svg` files, set the **Scale** parameter in Godot's Import panel to `2.0` for crisp rendering. The default `1.0` renders SVGs at native pixel size, which often appears soft at display scale.
- **Stroke widths.** Never use the hairline stroke style. Set explicit pixel values:
  - Icons at 128 px and smaller: **2–3 px**
  - Panel borders and UI frames: **3–4 px**
  - Any stroke below 1.5 px will become invisible or aliased at normal display scale.
- **Color consistency.** Match hex values exactly from the palette in `direction.md`. Use Object > Fill and Stroke to paste hex values directly. For sustained work, create a custom Inkscape `.gpl` palette file with the game's named colors so you are not copy-pasting hex values every session.
- **Text as paths.** Convert all text to paths (Path > Object to Path) before exporting any SVG used in Godot. This prevents font-not-found rendering failures on other machines or in the Godot export pipeline.
- **No embedded raster images.** SVG files imported into Godot should contain only vector paths. Embedded bitmaps will not render correctly on import.
- **Object naming.** Godot imports an SVG as a single flat texture — internal layer and object names do not carry over. Still name objects clearly for your own benefit when returning to a file later.
- **PNG export.** Use File > Export > Export PNG Image. Match the artboard at 1× unless producing a 2× version for high-DPI. Do not use Export Selection unless you intend to crop to just the selected object.

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
