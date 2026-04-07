# Ambient World Activities

This document defines the ambient and idle activity systems in Sailio — things the player can do beyond sailing and trading. These activities add texture to the world, reward curiosity, and give players something to engage with when they are drifting, waiting for wind, or simply exploring.

> **Prototype status:** All activities in this document are deferred. None are present in the prototype. See [prototype/scope.md](../prototype/scope.md).

---

## Design Intent

Ambient activities should feel like they belong to the world — not like minigame menus bolted onto a sailing simulator. The best ones emerge naturally from what players are already doing: trailing a line while sailing, pulling out an instrument at anchor, reaching for a bottle drifting past.

Activities fall into two broad categories:

**Idle activities** — things done while stationary or drifting. They require no active sailing input and can be interrupted at any time by the player choosing to sail again.

**Active world encounters** — things that require deliberate boat maneuvering to engage with. They are not minigames in the traditional sense; they use the core sailing controls in a focused context.

Both types are documented here. The distinction matters for implementation: idle activities can be gated behind a "drop anchor" or "heave to" state, while active encounters happen in open water and use the boat's existing physics.

---

## Activity: Fishing

**Category:** Idle

### Description
The player streams a fishing line off the stern of the boat. While the line is out, fish can be caught passively as the boat drifts or idles. The player is not required to actively manage the line — fishing is background activity, not a rhythm game or reaction challenge.

### Acquisition
Fishing is available from the start or purchasable early at any settlement. It requires no significant barrier — it should feel like a default thing you can always be doing.

### Mechanics
- The player deploys the fishing line from a UI action (e.g., a button in the HUD or via an inventory item)
- While deployed, the line passively accumulates catch opportunities over time
- Fish are caught based on location (different fish in different parts of the map — open water vs. sheltered bays vs. near island shores) and a probability roll at intervals
- The line is automatically retrieved when the player accelerates past a threshold speed — you cannot trawl at full sail
- Catch is added to a separate fish inventory (distinct from the cargo hold — fish do not occupy cargo slots)

### Collection
Fish species form a **collectible species log**. Each species has a name, a brief description, and an illustration. The log tracks how many of each species the player has caught across all sessions. Discovering a new species for the first time should feel like a small celebration.

Species variety should reflect the world's geography: common species everywhere, rare species in specific regions, and possibly a handful of legendary species with very low catch probability.

### Economy Integration
Fish can be sold at settlements for gold. Not all settlements buy fish — a forest town inland has less use for fresh catch than a fishing village. Which settlements buy which fish is defined in the map-specific settlement document.

See also: Floating Cargo Recovery below, which can yield items that complement or interact with the fishing system.

### Trophy Fish [FULL — post-prototype]

Beyond the standard species log, a small number of **trophy fish** exist as persistent world entities. These are named individual fish — specific creatures that live in the world and move within a defined territory. Only one player can catch each trophy fish per reset period. When caught, the fish disappears from the world until the next reset.

#### Design Intent
Standard fishing rewards consistent, patient players with collection progress and gold. Trophy fishing rewards *knowledge of the world and timing*. A player who understands where a trophy fish ranges and fishes that area consistently has a genuine edge over one who fishes randomly. This is skill expression through world knowledge — consistent with the game's broader philosophy.

The exclusivity of the catch (one per reset period, world-wide) turns each trophy fish into a shared hunt. Knowing that nobody else caught "Carl the Great" this month is a distinct kind of satisfaction from a personal best time.

#### Mechanics
- Trophy fish are individually named entities with a defined **territory** — a region of the map they move within over time. Territories vary in size; some fish roam widely, others are more localised.
- A trophy fish is caught through the same fishing mechanic as ordinary fish — no special input required. The player simply needs to be fishing within the fish's territory when a catch opportunity rolls.
- When caught, the trophy fish **despawns** for all players until the next reset. It cannot be caught again that period.
- Trophy fish are included in the species log and marked distinctly (e.g., a named entry with the catch date).
- The reward for catching a trophy fish should feel meaningfully special: a significant gold bounty, a unique item, a permanent log entry, or a named acknowledgement visible to other players (e.g., a settlement notice board).

#### Reset Cadence
Trophy fish reset on the same cadence as leaderboard records — monthly by default. On reset, all trophy fish reappear in their territories.

#### Naming
Trophy fish are individually named — not just "Large Cod" but a specific name like *Carl the Great*. Names should have personality and fit the maritime world. The name persists across resets (Carl is always Carl); only the catch status resets.

#### Discovery: Open Design Question
Whether players receive any hint that a trophy fish is nearby is an open design choice with two valid directions:

- **Blind discovery:** No hint. Players discover trophy fish purely through consistent fishing in territories they've learned over time. More magical when it happens; rewards deep world familiarity.
- **Hinted discovery:** A subtle signal when a trophy fish is catchable nearby — a minimap indicator, a rumour from a settlement NPC, or a disturbance in the water. Rewards engagement and gives players a reason to act on information. Easier to design rewards around.

Both are valid. This should be decided before implementation. Author is leaning towards a hinted discovery, with general geographic hints coming from NPCs within fishing villages.

#### Future: Migration Lore
Over time, the aggregate locations of all trophy fish catches can be plotted on a world map — a living record of where each fish has been caught across all players and all months. With enough data, patterns emerge: migration routes, seasonal tendencies, territories that shift. This turns the catch history into discoverable lore, rewarding players who pay attention to where catches have been reported.

These maps perhaps may only be visible in fishing villages.

This feature requires storing catch location (position on the map) alongside each trophy fish catch record. The data model should support this from the time trophy fish are implemented, even if the lore map UI is added later.

---

## Activity: Instruments

**Category:** Idle

### Description
The player acquires a musical instrument and can choose to play it while at anchor or drifting. Playing produces musical output — initially approximate and off-key, improving over time as the player's skill with that instrument grows. At high skill, the player's instrument sounds like a genuine, pleasurable contribution to a musical arrangement.

### Acquisition
Instruments are acquired through play — purchased at settlements, found in the world, or received as gifts (see Messages in a Bottle). Each instrument is a distinct item. The player can own multiple instruments.

### Skill Progression
Each instrument has an independent skill track. Skill increases with time played — not with correct inputs. There is no rhythm game, no button timing, no failure state. Skill is simply accumulated play time, expressed as:

- **Low skill:** Notes are approximate, timing is loose, the melody is recognisable but rough
- **Mid skill:** The instrument sounds competent and pleasant
- **High skill:** The performance is confident, musical, and sits well alongside other instruments

This model respects that players who just want to hear music can get there, while players who engage with it more will hear more polish.

### The Ensemble Vision
The instrument system is designed with a long-term multiplayer goal: **each instrument plays one layer of a shared musical arrangement**. If 5–6 instruments are all being played simultaneously by nearby players, they combine into a full song — each player contributing one part.

- Each instrument corresponds to a track in a pre-composed arrangement (e.g., one plays melody, one plays bass, one plays rhythm)
- A solo player hears only their track; nearby players' tracks layer in as they join
- At high skill, the combined tracks produce a recognisable, cohesive piece of music
- At low skill, the combination is pleasant but rough — it rewards a group of dedicated players more than a group of beginners

This is an audio architecture concern: tracks must be designed to be layered, and player proximity must be detectable. This vision should be kept in mind when the instrument audio assets are created — they need to be stems of the same arrangement, not independent pieces.

> **Note on music deferral:** Music is listed as deferred in the prototype scope. When the music system is scoped, the instrument layer architecture should be designed at the same time, not retrofitted later.

### Instrument Set (Initial)
A starting set of 5–6 instruments is the target. Exact instruments are TBD and should be chosen to fit the maritime world's aesthetic. Candidates: concertina, tin whistle, fiddle, bodhrán, hurdy-gurdy, lute. Final selection should be made alongside the art direction and audio direction documents.

---

## Activity: Messages in a Bottle

**Category:** Idle / Async Social

### Description
Players can write a short text message, optionally attach a small gift (an item from their inventory), seal it in a bottle, and release it into the water. Other players discover drifting bottles in the world and can retrieve them to read the message and claim any gift inside.

### Mechanics
- Bottles drift slowly with the current — their position updates over time between sessions
- A player who encounters a bottle sees it floating in the water and must approach it (similar to Floating Cargo Recovery, but simpler — no speed constraint)
- On retrieval, the message is displayed and any gift is added to the player's inventory
- The original sender can optionally be notified that their bottle was found

### Message Constraints
- Messages are short — a character limit appropriate for a small label or note (e.g., 200 characters)
- Messages are subject to content moderation before becoming visible to other players
- Players can optionally sign their message or leave it anonymous

### Gifts
- Any non-essential item can be gifted: fish, small amounts of gold, instruments, cosmetic items (future)
- Cargo goods (Timber, Rope, etc.) cannot be gifted via bottle — the bottle is a social object, not a trade bypass

### Social Design Intent
This system is deliberately low-stakes and warm. It creates a layer of player presence in the world without requiring synchronous multiplayer. Finding a bottle with a kind note and a small gift is a moment of delight. The system should not become a trade exploit or a harassment vector — the constraints above protect against both.

---

## Activity: Floating Cargo Recovery

**Category:** Active World Encounter

### Description
Occasionally, the player spots a floating item in the water — a crate, a bundle, a barrel — drifting loose on the surface. To recover it, the player must manoeuvre their boat alongside the item and come to a near-stop next to it. The item is then automatically collected.

### Why This Is Not an Idle Activity
Floating cargo recovery requires deliberate boat handling. The player must:
1. Spot the item from a distance (visual detection)
2. Plot an approach that accounts for wind — you cannot easily park a sailboat; you need to arrive on a heading that allows controlled deceleration
3. Reduce speed alongside the item without overshooting

This is a compact test of the core sailing mechanic — specifically, the man-overboard skill of precise boat control at low speed. It is not a separate minigame; it uses the exact same physics as normal sailing.

### Rewards
Floating cargo yields a random item from a loot table:
- Small amounts of gold
- Units of trade goods (any of the five)
- Rare: an instrument
- Rare: a fish species the player hasn't caught (a bottled specimen, narratively)
- Rare: a message in a bottle (pre-populated with world-building flavour text, not player-written)

The loot table should be tuned so that recovery is always worth attempting but never so rewarding that it becomes the optimal gold-making strategy over trading.

### Spawn Behaviour
- Floating cargo appears at randomised positions on the water, biased toward open-water areas away from settlements
- Items despawn after a time limit if not collected — they eventually sink
- Spawn rate should be low enough that encounters feel like a pleasant surprise, not a farming mechanic

---

## Activity: Whittling

**Category:** Idle

### Description
The player carves small wooden items while at anchor or drifting. Whittling produces sellable trinkets — decorative objects with no functional gameplay effect, but with real trade value. It is the only activity that lets the player generate sellable goods from nothing, and is balanced accordingly.

### Acquisition
The player starts with or acquires a whittling knife early in the game (purchasable at any settlement, or given as a starter item). No additional materials are required — the boat always has scrap timber aboard.

### Mechanics
- The player activates whittling from the HUD while stationary or drifting
- Each whittling session produces one trinket after a fixed time (e.g., 2–3 minutes of idle time)
- Trinkets are stored in a small separate inventory, distinct from the cargo hold — they do not occupy cargo slots
- Only one trinket can be in progress at a time; the player cannot queue multiple
- Whittling is interrupted if the player begins sailing at speed (same threshold as fishing)

### Trinket Variety
A small set of trinket types can be carved, each with a name and flavour description. Examples: a small fish, a toy boat, a compass rose, a seabird. The available types expand as the player's whittling skill increases.

### Skill Progression
Whittling has a simple skill track based on total trinkets produced. Higher skill unlocks:
- Additional trinket types
- Slightly higher sell value per trinket
- Reduced carving time (marginally)

Skill should progress slowly enough that whittling remains a supplementary income source, not a dominant one.

### Economy Integration
Trinkets are sold at settlements for gold. All settlements buy trinkets — they are universally appealing small goods. Sell price is modest and fixed (not subject to the ±60% trade spread), keeping whittling income predictable but never dominant over trading.

> **Balance note:** Whittling produces gold from time alone, with no cargo investment. Its per-hour gold yield should be tuned to be meaningfully below the yield of an active trade run — it rewards patience during downtime, not as a replacement for sailing.

---

## Activity Economy Summary

| Activity | Reward type | Sells at settlement? | Notes |
|---|---|---|---|
| Fishing | Fish species (collection) + gold | Yes, at select settlements | Fish inventory separate from cargo hold |
| Instruments | Enjoyment, ensemble play | No — instruments are kept | Acquired, not produced |
| Messages in a Bottle | Social gifts, delight | No | Gift items enter normal inventory |
| Floating Cargo Recovery | Gold, goods, rare items | Items sell normally | Loot table tuned to not outpace trading |
| Whittling | Trinkets (sellable) + gold | Yes, at all settlements | Separate trinket inventory; yield below active trading |

---

## Future Activities

The five activities above represent the initial set but are not exhaustive. When adding future activities, they should adhere to the following guidelines:

- They emerge naturally from the sailing context — they feel like things sailors do, not things imported from another genre
- They have a distinct reward loop that does not make trading or racing obsolete
- They are completable in short sessions — a player can engage and disengage quickly
- They are individually optional — no activity should be required to progress

