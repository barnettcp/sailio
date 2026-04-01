# Prototype Settlements

This document defines the five settlements of the prototype world. Each entry covers map position, economic role, landmark, and personality. Together they form a complete, self-contained trading world.

The map is derived from Lake Washington rotated 180°, so the southern end of the real lake becomes the northern end of the game map. Juanita Bay sits at the south, Mercer Island is toward the north-centre, and Seward Park is at the far north. This rotation makes the geography less immediately recognisable while keeping the shape's natural character intact.

For the full price matrix and economy model, see [design/economy.md](../design/economy.md).
For map placement context, see [design/world.md](../design/world.md).

---

## Settlement Overview

| Name | Map Position | Type | Produces | Wants | Special Role |
|---|---|---|---|---|---|
| Rockaway | SW shore, south end of map | Fishing Village | Rope | Charts | START |
| Gopher's Bay | E-centre (Union Bay rotated) | Navigator's Outpost | Charts | Rope | TUTORIAL destination |
| Woollie | Centre-north island (Mercer Island rotated) | Weaving Port | Sailcloth | Timber | — |
| Lincolnston | W-centre shore (Bellevue rotated) | Foundry Harbour | Ironwork | Sailcloth | — |
| Greenway | NE shore, north end of map (Seward Park rotated) | Forest Town | Timber | Ironwork | — |

---

## Rockaway
**Type:** Fishing Village | **Produces:** Rope | **Wants:** Charts
**Map position:** SW shore, south end of the map
**Landmark:** A long timber dock extending into the bay, visible from far out on the water
**Personality:** Smaller local town — unhurried, familiar, the kind of place where everyone knows the tides

This is where the player begins. Rockaway is a modest fishing harbour tucked into a sheltered southwest bay at the southern end of the map. It's the most sheltered and approachable port — a natural starting point. The player's first sail out of Rockaway opens into the main body of water with room to find their bearings.

Rockaway produces Rope in quantity (fishing lines, rigging, mooring lines — the village runs on it). The villagers here love angling and relaxing. Experts of the local waterways, they love to laze around and study local nautical charts. This puts charts of all kind in high demand. 

The land surrounding Rockaway are rolling hills with some pretty trees. Houses dot the landscape. The long dock allows for ships to tie off as well as contains a market made up of a small row of colorful tents.

**Tutorial role:** The player starts here with a small purse and a hold containing 2 units of Rope and 1 unit of Timber. A hint directs them toward Gopher's Bay, where Rope sells at a premium. After their first successful trade, Rockaway becomes simply another port of call.

---

## Gopher's Bay
**Type:** Navigator's Outpost | **Produces:** Charts | **Wants:** Rope
**Map position:** E-centre of the map (Union Bay inlet, rotated)
**Landmark:** A large library and chart house on the waterfront — tall, distinctive, visible from mid-channel
**Personality:** Upbeat and colourful — a busy, intellectual harbour town with flags, signage, and the smell of ink and salt

Gopher's Bay is the tutorial destination and sits in the eastern-centre of the map — a natural diagonal crossing northeast from Rockaway across open water. The chart house dominates the waterfront and is recognisable from a distance — players will learn to navigate to it by sight.

The settlement produces Charts (printed, copied, and bound here from the accumulated knowledge of generations of navigators). Rope is the one thing chart-makers, bookbinders, and scribes all need and can never make themselves so it commands a premium here.

Gopher's bay is bright and cheerful, with houses painted in many bright colors. Some groups of colorful bulidings are only visible as you enter the bay, with residents choosing to paint their outward facing walls a more bland color. The land around this bay slopes gently inward, creating a protected and peaceful pace.

**Trade note:** Charts bought here cheaply are best sold at Rockaway (which wants them) or carried onward to any port where navigation knowledge is scarce.

**Tutorial role:** On first arrival, the player is walked through the timer system (their crossing time from Rockaway is displayed and recorded), the merchant screen, and optionally the boat yard. A hint suggests Woollie on Mercer Island needs Timber — planting the next destination without forcing it.

---

## Woollie
**Type:** Weaving Port | **Produces:** Sailcloth | **Wants:** Timber
**Map position:** Centre-north island (Mercer Island, rotated to north-centre of map)
**Landmark:** Large windmills on the high ground of the island, turning steadily — visible from both channels on either side
**Personality:** Grassy and slow — an island community that keeps its own pace, proud of its craft

Woollie occupies the central island in the northern half of the map. The island divides the upper channel into two passages: the western passage is wider and more direct toward Lincolnston; the eastern passage is narrower and angles toward Greenway. Players will develop a preference based on wind conditions.

The windmills are the island's defining image. They power the looms and card the wool that becomes the finest sailcloth on the water. Woollie needs Timber constantly: for loom frames, mill repairs, dock maintenance, and buildings. With only a few patches of trees, the island grows very little that is taller than scrub.

The hills are a bright green grass, dotted with small herds of sheep among the bright white windmills. In some places the island's hills are rocky and steep, but mostly green. Very few buildings are outside of the town of Woollie, but those that are well-kept.

**Trade note:** Sailcloth bought here cheaply is most wanted at Lincolnston (Foundry Harbour), which uses it for bellows, covers, and industrial cloth.

---

## Lincolnston
**Type:** Foundry Harbour | **Produces:** Ironwork | **Wants:** Sailcloth
**Map position:** W-centre shore (Bellevue shore, rotated to west side)
**Landmark:** A prominent mining and foundry headquarters building — wide, dark-roofed, with a chimney stack that lets off occasional smoke
**Personality:** Work hard, play hard — a loud, purposeful town that smells of coal and hot metal, but has the best tavern on the water

Lincolnston is the industrial heart of the world, sitting on the western shore in the centre of the map. It produces Ironwork — nails, cleats, fittings, chain, anchors — everything that holds a boat together and keeps a harbour functioning. The foundry runs on coal and bellows, and the bellows wear out. Sailcloth is always in demand.

The smoke stack is visible from a fair distance and serves as a navigational marker on the western shore. Lincolnston is a natural mid-point on routes between the island and the southern shore.

The buildings here have a more modern feel, but still the atmosphere is friendly. Monochromatic tones are more common, but artfully done.

**Trade note:** Ironwork bought here cheaply is most wanted at Greenway (Forest Town), which needs it for logging tools and mill machinery.

---

## Greenway
**Type:** Forest Town | **Produces:** Timber | **Wants:** Ironwork
**Map position:** NE shore, north end of map (Seward Park, rotated to far north)
**Landmark:** Giant trees rising above the settlement, visible from well out on the water — the tallest natural feature on the map
**Personality:** Friendly and industrious — a working forest community, cheerful and self-sufficient, with sawdust on everything

Greenway occupies a forested headland at the northern end of the map. The giant trees are its signature — old-growth that towers above the settlement and can be seen from mid-water. The town mills timber constantly: rough-cut boards, spars, masts, and planks all leave Greenway by boat.

The loggers and millworkers need good iron tools — axes, saw blades, adzes — and the local forge can't keep up with demand. Ironwork from Lincolnston is always welcome here.

Greenway is a natural endpoint for south-to-north routes and sits at the far end of the map from Rockaway, making a Rockaway–Greenway crossing one of the longer routes and a strong record-setting target.

Despite Greenway's industry of logging, trees are revered here and they are proud to be home to the many tall trees. Some nearby hillsides contain plots of land at various stages of growth for continuous and sustainable harvest of trees.

**Trade note:** Timber bought here cheaply is most wanted at Woollie (Weaving Port), which needs it for loom and mill maintenance.

---

## Price Matrix

Prices in gold per unit. Calculated from baseline prices using producer (40–50%), neutral (90–110%), and high-demand (140–160%) modifiers.

| Good | Rockaway | Gopher's Bay | Woollie | Lincolnston | Greenway |
|---|---|---|---|---|---|
| **Timber** *(baseline 40)* | 40 | 40 | 60 | 40 | 18 |
| **Rope** *(baseline 55)* | 25 | 83 | 55 | 55 | 55 |
| **Sailcloth** *(baseline 75)* | 75 | 75 | 34 | 113 | 75 |
| **Ironwork** *(baseline 100)* | 100 | 100 | 100 | 45 | 150 |
| **Charts** *(baseline 130)* | 195 | 59 | 130 | 130 | 130 |

**Key profitable routes (prototype):**
| Route | Cargo | Buy | Sell | Profit per unit |
|---|---|---|---|---|
| Rockaway → Gopher's Bay | Rope | 25 | 83 | 58 |
| Gopher's Bay → Rockaway | Charts | 59 | 195 | 136 |
| Woollie → Greenway | Sailcloth? | — | — | — |
| Lincolnston → Greenway | Ironwork? | — | — | — |
| Greenway → Woollie | Timber | 18 | 60 | 42 |
| Woollie → Lincolnston | Sailcloth | 34 | 113 | 79 |
| Lincolnston → Greenway | Ironwork | 45 | 150 | 105 |

> All prices are starting targets for tuning. The goal is that a fully upgraded boat running optimal routes earns enough in 10–15 minutes to afford the next upgrade tier.

---

## Tutorial Sequence

1. **Rockaway (START):** Player spawns with 150 gold, 2× Rope, 1× Timber. Tutorial introduces sailing controls.
2. **Sail to Gopher's Bay:** Player is prompted toward Gopher's Bay (Rope in demand). First timed crossing. On arrival, timer result is displayed and explained.
3. **At Gopher's Bay:** Player sells Rope (83g each, strong profit). Walks through merchant screen. Optionally buys Charts (59g) to carry onward. Boat yard is available for a first upgrade. A hint suggests Woollie on the island to the north needs Timber.
4. **Free play:** Tutorial ends. Player sails onward under their own direction.

