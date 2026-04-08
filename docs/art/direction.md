# Art Direction

This document describes the visual language of Sailio: its tone, palette, shape vocabulary, lighting, and per-settlement identities. It is the reference for all art production decisions. When a visual choice is unclear, the answer should come from this document.

See [assets.md](assets.md) for the complete asset checklist. See [ux/ui-flows.md](../ux/ui-flows.md) for UI layout.

---

## Visual Tone

Sailio should look like a **chill day on the water**: thoughtful details where they matter, and a laid-back approach to the rest. The world is bright, legible, and playful. A player who has no experience sailing should have plenty to enjoy; an experienced sailor may appreciate some of the attention to detail ignore details that are intentionally glossed over. The weather is pleasant and the aesthetic sits somewhere between a children's illustrated map and a clean mobile game - simplified geometry with bold shapes, clear silhouettes, and a palette that feels optimistic without being garish.

The environment should feel thoughtful, warm, and alive. The goal is to have it populated with details that are charming when noticed, making you want to keep looking and exploring. While the depth of Breath of the Wild is not intended, the immersive nature of that world is inspiration here.

Vibrant colors are used throughout the world in thoughtful and sometimes unrealistic ways. For example, some kinds of grass may be bright magenta against the otherwise green and brown hues of a hillside dotted with trees.

The one-sentence test: **does it look like somewhere you'd want to spend an afternoon?** If yes, it is on-tone.

### What It Is Not
- Not realistic. No photorealistic water, no PBR materials, no complex lighting setups.
- Not dark or gritty. No desaturated palettes, no moody weather.
- Not busy. Ornamentation should be restrained. Each element earns its presence.
- Not flat. The 3D geometry and lighting should give the world depth, even with a simplified style.

---

## Color Philosophy

### World Palette
The world palette is warm and saturated. Water is the dominant surface and should feel alive — a deep, clear blue-green that catches light rather than sitting flat. Long-term vision is the sand-to-blue hues visible in New Zealand near the coastlines, and a deep bluewater blue in between.

The hex values below are **starting suggestions** — grounded in the NZ coastline reference and the warm/saturated tone intent. Adjust freely in Blender and Inkscape and update the table to reflect production decisions. These are the canonical values the Inkscape palette file and assets.md references point to.

| Element | Tone | Starting hex | Notes |
|---|---|---|---|
| Water (shallow / coastal) | Blue-green, bright | `#47C6C6` | Sandy teal. NZ coast feel near shorelines. |
| Water (mid-lake / deep) | Deep clear blue | `#1A7FA8` | Dominant open-water color. |
| Grass / warm green | Warm green to golden | `#7DC95E` | General terrain. Woollie's bright hillsides. |
| Grass / forest green | Deep green | `#3D7A45` | Greenway and shadowed slopes. |
| Sand / dirt | Warm tan to ochre | `#D4A95A` | Shoreline transitions, bare ground near settlements. |
| Rock | Cool grey | `#8A9BA8` | Shoreline rocks and island outcrops. Contrasts water. |
| Sky (if rendered) | Pale warm blue | `#C8DFF0` | Horizon haze acceptable. |

### Vibrant Color Accents
The world uses **intentional, unrealistic color accents** as a design tool — not as decoration, but as a considered choice that makes the world feel alive and slightly magical. A hillside might have a patch of bright magenta grass among otherwise natural greens. A shoreline might have vivid teal moss on otherwise grey rocks.

The governing principle is **purposeful contrast**: accents work because they are surrounded by naturalistic tones. A world that is entirely saturated becomes noise; a world that is mostly grounded and occasionally surprising is memorable. 

- Accents should feel like they belong to the world's internal logic, not like an error or oversight
- No single area should have more than one or two accent colors competing
- Accents are most effective at mid-ground — visible enough to reward looking, not so prominent they distract from gameplay
- Settlement color identities (see below) take precedence over accents near ports — the accent language is primarily for open terrain and waterways

### Boat Palette
The player's boat should be the most distinctive object on the water. Sails are the primary visual — they should be bold and readable at minimap scale. A white or cream hull with colourful sails is the default intention; specific colors to be decided at time of production.

### Settlement Color Identities
Each settlement has a color personality that runs through its buildings, details, and terrain. These are not strict rules — they are tendencies that make each port feel distinct when the player arrives. The settlement's harbor zone and start line markers should not have the same color, but should vary slightly in hue to be distinct.

| Settlement | Color personality | Notes |
|---|---|---|
| **Rockaway** | Warm wood tones, rope-tan, faded blues | A working fishing village. Weathered but cozy. Tents at the market end of the dock in muted reds and ochres. |
| **Gopher's Bay** | Bright and varied — many distinct colors per building | Cheerful and intellectual. Buildings are individually colored; outer-facing walls are deliberately more muted (this is a designed quirk, not an oversight). |
| **Woollie** | Bright grass green, white, soft wool tones | Clean and pastoral. White windmill sails and whitewashed buildings against vivid green hillsides. Sheep-grey accents. |
| **Lincolnston** | Dark slate, charcoal, iron grey — but tasteful | Monochromatic by design, not by neglect. The artfulness matters — think dark tones used as a considered palette, not just "industrial grime." |
| **Greenway** | Deep forest greens, amber wood tones, sawdust beige | Raw material neatly stored: Lumber stacks and log piles are part of the scenery. |

---

## Shape Vocabulary

### General Principles
- **Exaggerate proportions** for readability at camera distance. Buildings should be slightly taller and chunkier than realistic scale; the boat should read clearly from above.
- **Favour silhouette**. Every important asset should be identifiable by outline alone. The chart house, the windmill blades, the chimney, the giant tree — all should be unmistakable from mid-water.
- **Round where soft, angular where rigid**. Terrain, sails, and vegetation lean toward curves; docks, buildings, and machinery lean toward hard edges.

### The Boat
The boat is the player's avatar. It should be the most carefully designed asset in the game. Key shape requirements:
- **Chunky and readable from above.** The hull should have enough width to be clearly visible on the minimap. Avoid a thin racing-yacht profile.
- **Sails must be visually distinct from each other.** Jib (small triangle), Genoa (slightly larger triangle), Spinnaker (wide, billowing, visually unmistakable) — even a player who doesn't know sailing terminology should intuitively sense that the Spinnaker is a different beast.
- **The mainsail is always present** when any configuration is active. It should be clearly the largest fore-aft sail.
- **Exaggerated mast height** is acceptable and encouraged. A tall mast reads well from above.

### Landmarks
Each settlement's landmark is its primary navigational marker — players will navigate by them before they read map labels.

| Settlement | Landmark | Shape notes |
|---|---|---|
| Rockaway | Long timber dock with market tents | Horizontal emphasis, extending into the water. Tents are colourful peaks against the dock flat. |
| Gopher's Bay | Tall chart house / library | Vertical emphasis. Tallest building in the settlement. Distinctive roofline or tower. |
| Woollie | Windmills | Round blades rotating against the sky. High ground placement means they read against the horizon. 2–3 mills in close proximity. |
| Lincolnston | Foundry HQ with chimney | Wide, low mass with a vertical chimney breaking the roofline. Chimney smoke is a P3 particle effect. |
| Greenway | Giant old-growth trees | The tallest natural features on the map. Tree canopy should visibly dominate the skyline above the settlement. |

---

## Lighting

The world is lit as a **bright mid-morning to early afternoon**. This is a permanent time of day in the prototype — no day/night cycle.

- **Primary light:** Directional sun, warm white, coming from a consistent angle (suggest: high and slightly south). Angle should be chosen to cast readable shadows that add depth without obscuring top-down readability.
- **Ambient light:** Warm, not dark. Shadow areas should still be clearly readable, not muddy.
- **Water reflections:** Subtle. A light sheen on the water surface that shifts as the boat moves is desirable; complex reflections are not required.
- **No dramatic lighting effects** in the prototype. No god rays, bloom, or atmospheric fog. These are deferred to visual polish.

### Future: Day/Night Lighting Vision
If a day/night cycle is added, the nighttime aesthetic should be **light, cheerful, and glowing** — not dark or foreboding. The world at night should feel like a festival rather than a threat.

Glowing elements are the primary light source in this vision:
- **Bioluminescent water** — shorelines and wakes that glow softly in blues and greens
- **Campfires** at settlements — warm amber point lights, visible from the water
- **Settlement windows and lanterns** — warm yellow glow from buildings
- **Decorative lights** — strings of colored lights at docks, glowing buoys, lanterns on boats

Any object that is intended to glow at night should be **modelled with a separate emissive mesh or material flag** from the start — do not assume this can be added to an existing model without rework. When placing world objects, consider whether they will contribute to the nighttime scene.

The overall palette at night should remain colourful — use the vibrant accent logic from daytime but shifted toward luminous blues, greens, and warm ambers rather than relying on reflected sunlight.

---

## Camera

The camera is fixed — the player does not control it. It sits above the world at a consistent angle.

- **Angle:** Fixed isometric-style. Slightly tilted from true overhead (approximately 45–60° from horizontal) so that buildings have visible front faces rather than just rooftops. Exact angle should be tested in-engine for readability before being locked. The angle should be implemented as a parameter, not hardcoded — this keeps a player-adjustable option open in future without a code change.
- **Height:** Fixed — no zoom. The full playable area should be navigable without the player needing to reframe the world.
- **Orientation:** North = up on screen. The compass rose in the HUD reinforces this.
- **Field of view:** The visible area should comfortably encompass a settlement and its immediate surroundings, with enough open water ahead to read the wind and plan a heading.

> The camera angle is the single most important art production constraint. **All assets should be modelled and textured for this specific viewpoint.** Detail on the underside of objects, or on faces that point away from the camera, is wasted production time.

---

## UI & HUD Visual Style

The HUD is drawn in **Inkscape** and imported as 2D overlay assets. It should share the world's warmth and clarity without competing with the sailing view.

- **Clean and minimal.** Each HUD element should occupy the minimum screen area needed to be readable.
- **Rounded edges and warm tones.** UI panels should feel like the game world, not like a technical interface.
- **High contrast.** All text and symbols must be readable against both bright water and dark shoreline. Use dropshadows or panel backgrounds rather than relying on colour contrast alone.
- **Icons over labels where possible.** The sail configuration selector, wind direction arrow, and cargo grid should all be icon-first.

---

## Settlement Terrain Rules

Each settlement's immediate terrain should visually reflect its economy role. This makes the world feel self-consistent rather than arbitrarily decorated.

| Settlement | Terrain character |
|---|---|
| Rockaway | Rolling hills, scattered trees, shoreline pebbles. Open enough to feel accessible. |
| Gopher's Bay | Gently sloping land creating a sheltered bay. Grassy, open, a few ornamental trees near the chart house. |
| Woollie | Bright green hillsides, low scrub, very few tall trees. Sheep visible as small props. Rocky in places. |
| Lincolnston | Flatter, harder ground near the water. Less vegetation. Some evidence of excavation or industrial staging area. |
| Greenway | Dense tree cover starting immediately inland. Visible lumber staging areas (stacked logs, cleared patches at various regrowth stages). |

---

## Character Theme

An animal-based character theme (crew figures, NPCs) is under consideration for a future pass and is **not implemented in the prototype**. All systems and models should be character-agnostic — no humanoid proportions, faces, or character-specific details required at any stage until a character direction is explicitly chosen.

---

## Tools & Workflow

- **3D assets:** Blender → exported as `.glb` (binary glTF) → imported to Godot 4
- **2D / UI assets:** Inkscape → exported as `.svg` or `.png` → imported to Godot 4
- **AI-generated art:** Not used. All assets are hand-authored.

See [assets.md](assets.md) for per-asset production notes and the Blender workflow checklist.
