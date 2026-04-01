# Racing & Timer System

This document defines how voyage timing, route recording, and leaderboards work in Sailio. The timer system is the game's most distinctive mechanical feature — it makes every departure a race start, whether the player intends it or not.

---

## Design Intent

Every voyage between settlements is implicitly a race. The timer requires no opt-in. Players who are trading, exploring, or just sailing are simultaneously setting times. The leaderboard surfaces those times and ranks them — creating a competitive layer that overlays all other play.

The system should feel **lightweight and ambient** during play (you are barely aware of the timer) and feel **satisfying and meaningful** at arrival (your time is revealed, ranked, and kept).

---

## Timer Behaviour

### Start
The timer for a route begins the moment the player's boat **exits a settlement's dock zone**. The departure settlement is recorded as the route origin.

- The timer starts automatically — no player action required
- The HUD displays the running elapsed time from the moment of departure
- If the player re-enters the departure settlement's dock zone before reaching another settlement, the timer resets (the run is abandoned)

### Stop
The timer stops the moment the player's boat **enters any other settlement's dock zone**. That settlement is recorded as the route destination.

- The elapsed time is immediately displayed to the player
- The time is recorded against the route `origin → destination`
- The run is directional: Rockaway → Gopher's Bay and Gopher's Bay → Rockaway are separate routes with separate leaderboards

### Abandoned Runs
If the player closes the game mid-voyage, the run is discarded. No partial times are recorded. On next session load, the timer is not running — the player must depart again to start a new run.

---

## Route Definition

A route is defined as an ordered pair: **(origin, destination)**. Routes are directional — the same two settlements produce two distinct routes depending on which way the player is sailing.

In the prototype with 5 settlements, the total number of tracked routes is:

$$5 \times 4 = 20 \text{ directional routes}$$

Each route maintains its own independent leaderboard and time history.

### Multi-Stop Routes (deferred)
Future versions will track combined times for sequences of up to 3 legs (e.g., Rockaway → Woollie → Greenway as a single timed passage). The data model should not prevent this: storing individual leg times with session identifiers allows multi-stop totals to be calculated without re-recording data.

---

## Time Storage

### What is stored per run
Every completed run stores:
- Origin settlement
- Destination settlement
- Elapsed time (to the nearest tenth of a second)
- Timestamp (real-world date/time of the run)
- Player identifier (local in prototype; account-linked in future)
- **Boat upgrade tier** (snapshot at the moment of departure — see Tier-Class Leaderboards below)

### Storage requirement: all times, not just top N
Both raw rankings and percentile rankings require access to the full distribution of times — not just the top 3. **All completed run times must be stored**, not only leaderboard entries. In the prototype this is a local save file; in the future it is a server-side time-series store.

This is an architectural constraint. Do not implement a system that discards times outside the top N at write time.

---

## Leaderboards

### Display
Leaderboard data is surfaced in two places:
- **At each settlement:** on arrival, the player's new time is shown alongside the top times for that arrival route
- **Map screen:** a full route leaderboard is accessible from the map, showing all 20 directional routes and their top times

### Ranking Types

Two ranking types are displayed for each route:

**Raw ranking:** The player's position by time among all recorded runs. E.g., "#4 fastest of all time on this route."

**Percentile ranking:** The player's position expressed as a percentage of all runners. E.g., "Top 8% on this route." Calculated as:

$$\text{percentile} = \left(1 - \frac{\text{rank} - 1}{N}\right) \times 100$$

Where $N$ is the total number of recorded runs on that route.

> **Why both?** Raw rank becomes unachievable as a game grows. A player who is genuinely fast may be rank #847 simply because thousands of others have played longer. Percentile rank is scale-invariant — being in the top 5% means the same thing whether there are 100 players or 100,000. Both numbers are meaningful to different players.

### Prototype Leaderboard
In the prototype, all records are **local only**. The leaderboard shows times from the current save file. There is no online comparison. The full ranking system (raw + percentile) is still implemented locally — it just operates on the local dataset.

This means the percentile system will have very few data points in the prototype (the player's own times). That is acceptable — the infrastructure should be built to work correctly at scale from the start, even if the prototype data set is tiny.

---

## Tier-Class Leaderboards

In addition to the open leaderboard (all boats, all tiers), times are also ranked within each upgrade tier. The boat's tier is **snapshotted at departure** — not read at arrival. This prevents a player from upgrading mid-voyage and claiming a lower tier's record.

> **Snapshot definition:** Tier is the highest tier that has been fully purchased across all upgrade categories at the moment the dock zone is exited. A boat that has any Tier 2 upgrades installed is classified as Tier 2 for that run, regardless of which categories were upgraded.

This creates parallel leaderboards per route — one open and one per tier:

| Leaderboard class | Who competes |
|---|---|
| Open | All runs, all tiers |
| Tier 0 (stock) | No upgrades purchased |
| Tier 1 | At least one Tier 1 upgrade, no higher |
| Tier 2 | At least one Tier 2 upgrade, no higher |
| Tier 3 | At least one Tier 3 upgrade, no higher |
| Tier 4 (max) | Any Tier 4 upgrade purchased |

### Why tier-class records matter
A fully maxed boat will always outrun a stock boat on the open leaderboard. Tier-class records make the competition fair within each upgrade stage — a new player can compete for the fastest Tier 0 time on a route, which is a genuinely achievable goal. A mid-game player can chase Tier 2 records without being buried by Tier 4 veterans.

This also opens the door to **monthly rewards per tier class** — e.g., the fastest Tier 3 time on each route over the month. Such rewards incentivise players to keep racing even when they haven't maxed upgrades.

### Prototype scope for tier-class
In the prototype, tier class is stored per run but the leaderboard displays open times only. Tier-filtered views are **not required** — in the prototype or in the full game — but the data should be captured so the option remains open. The data model must support filtering from the start, even if no UI exposes it.

---

## Record Reset Cadence

Leaderboards reset on a regular interval to keep records fresh and attainable for new and returning players.

- **Default cadence:** monthly (every 30 days from the last reset)
- The cadence is **configurable** — stored as a setting in a data file, not hardcoded, so it can be adjusted without a code change
- On reset: all stored run times for all routes are cleared. Upgrade state and gold are **not** reset — only race records.
- Players are notified of an upcoming reset (e.g., "Records reset in 3 days") so they can make a last attempt

> **Prototype:** No automatic reset runs in the prototype. Records persist until manually cleared or a new save is started. The reset system should be architecturally present but not scheduled.

### Cadence Considerations

| Cadence | Pros | Cons |
|---|---|---|
| Weekly | Records feel very fresh | Records disappear before they feel meaningful; punishing for casual players |
| Monthly | Enough time to feel proud of a record | Optimal for most player bases |
| Seasonal (quarterly) | Records feel prestigious | May feel stale for active players |

Monthly is the recommended default. Consider allowing the player to see when the next reset is at all times.

---

## Personal Bests

In addition to global leaderboard times, the game tracks each player's **personal best** per route separately. Personal bests are **never reset** by the cadence wipe — they are permanent records of the player's own performance.

The player can always see:
- Their personal best for any route
- How their personal best ranks against the current leaderboard (which may reset around it)

This means a player whose personal best was set two seasons ago can still see it, even if it no longer appears on the active leaderboard.

---

## Future: Global Online Leaderboard

Post-prototype, times will be submitted to a shared global leaderboard visible to all players. Design and architectural intent:

- **All times are stored server-side.** Every completed run from every player is recorded. This is the dataset that drives both raw rankings and percentile calculations.
- **Queries must be performant at scale.** Leaderboard reads should use indexed, pre-aggregated data — not full table scans. The top-N display and percentile calculation should be pre-computed or cached, not calculated live per request.
- **Route leaderboards are independent datasets.** Each of the 20 directional routes is its own indexed leaderboard. There is no global "all routes" ranking in the prototype, though this could be a future feature (e.g., a combined score across all routes).
- **Write path:** a time is written to the server on dock zone arrival. The write can be asynchronous — a slight delay in leaderboard update is acceptable. Failure to write (offline, server error) should degrade gracefully: the time is stored locally and synced on next session.
- **Ghost boats** (replays of recorded voyages alongside the player) will draw from the same time-series data. Each stored run is a candidate ghost. Architecture should store enough positional data per run to replay it — or defer ghost storage to when that feature is scoped.

---

## HUD & UI Summary

| Element | Location | Notes |
|---|---|---|
| Running timer | Top-left HUD | Displays elapsed time since last departure; always visible |
| Arrival time reveal | Centre screen (brief overlay) | Shown on dock zone entry; dismissed automatically |
| Personal best indicator | Alongside arrival overlay | "New personal best!" if applicable |
| Route leaderboard | Settlement arrival screen | Top times for the just-completed route |
| Full leaderboard | Map screen | All routes; filterable by route |
| Reset countdown | Map screen / settings | "Next reset in X days" |

See [ux/ui-flows.md](../ux/ui-flows.md) for detailed screen layouts.

