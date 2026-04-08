# Multiplayer — Design Intent

This document covers Sailio's multiplayer vision and the architectural decisions that flow from it. **None of this is in scope for the prototype.** The prototype is single-player with local data only. This document exists to ensure future development decisions are made against a clear intent, and to prevent the async leaderboard system from being confused with the live-world vision.

---

## Two Distinct Systems

Sailio has two separate multiplayer-adjacent features that must not be conflated:

| System | What it is | When |
|---|---|---|
| **Async online leaderboard** | Shared score submission and ranking across all players. Times, routes, and personal bests stored server-side. | Phase 1 — first post-prototype multiplayer step. |
| **Live world (real-time)** | All players share a single persistent world. Boats are visible to each other in real time. | Phase 2 — a major architectural addition. |

These are built separately and in order. Phase 1 is a prerequisite for Phase 2 because it establishes player identity and server infrastructure.

---

## Phase 1 — Async Online Leaderboard

The async leaderboard is the first foray into multiplayer. It may ship as a **standalone web page hosted separately** from the game itself — a leaderboard site the player can visit outside of play. The game (Godot HTML5) posts times to a small backend on each leg completion.

### What this requires

- **A backend endpoint** that accepts score submissions (player UUID, route, time, tier snapshot, timestamp) and stores them.
- **A leaderboard query endpoint** the game and/or the leaderboard site reads from.
- **Reset logic** that archives and clears monthly (see racing-and-timers.md).

### Recommended stack at prototype scale

A straightforward option is **Supabase** (free tier): a hosted Postgres database with a built-in REST API and row-level security. The Godot client uses `HTTPRequest` to POST scores and GET leaderboard reads. No custom server code needed at this stage. Alternatives include a Cloudflare Worker + KV store (serverless, very low cost, no SQL) or a small Node.js/Express API on a cheap VPS.

The specific technology is not locked here — what matters is that the data model is defined (see racing-and-timers.md) and the write/read contract is stable before the client is built to it.

---

## Player Identity

Sailio avoids requiring players to create an account. The standard approach for casual browser games is:

### Anonymous UUID model

1. On first launch, the client generates a random UUID and stores it in browser `localStorage`.
2. All server-side data (scores, upgrades, player name, personal bests) is keyed to that UUID.
3. The player never sees the UUID.

**Tradeoff:** clearing browser data or switching browsers loses the player's identity.

### Export code (identity recovery)

To mitigate the above, an **export code** is offered — a short alphanumeric string the player can copy from the settings screen. It encodes their UUID and a server-side HMAC signature so it cannot be forged. Pasting the code on another device or browser restores their identity and data.

This is the entire account system. No email, no password, no OAuth. The player gets persistence without friction, and the developer avoids account infrastructure.

### What persists server-side (per UUID)

| Data | Notes |
|---|---|
| Player name | Display name for leaderboards. Subject to content moderation. |
| All leg times | Every completed run, per route and direction. |
| Personal bests | Derived from the run history; also cached for fast HUD display. |
| Upgrade tier | Current upgrade state per category. Resets only on explicit New Game. |
| Cargo state | Goods currently held. Persists across sessions. |

Upgrades and cargo persisting server-side is a firm requirement: the sailing world is large enough that progress lost to a browser refresh would be unacceptable.

---

## Content Moderation

The player name field is public-facing on leaderboards. Without accounts, moderation options are:

- **Client-side profanity filter on submission** — catches obvious cases, easily bypassed. Necessary but not sufficient.
- **Server-side flagging queue** — submissions that match a blocklist are held for review or auto-rejected. Simple to implement.
- **Report mechanism** — a "flag this name" option on the leaderboard. Requires a review queue for the developer.
- **Lightweight external API** — Google's Perspective API (free tier) can score toxicity on submission. Adds a network dependency but reduces manual review burden.

Recommended approach: client-side filter on submission + server-side blocklist + a simple flag queue visible to the developer. The Perspective API is a nice-to-have for scale.

---

## Phase 2 — Live World (Real-Time Multiplayer)

The long-term vision is a **single shared persistent world** where all connected players are visible to each other as sailing boats in real time — similar to the slither.io model. Players race the same routes, see each other crossing start lines, and encounter familiar vessels by sail configuration.

### Core model

- All clients connect to a shared WebSocket server.
- Each client broadcasts its position, heading, active sail config, and speed every ~50ms.
- The server relays all player states to all other clients in the same world instance.
- **No collision physics.** Boats pass through each other. There is no collision to reconcile between clients. This simplifies the architecture significantly.

### Recommended framework

**Colyseus** — open source, runs on Node.js, has an official Godot client library, and is designed for exactly this use case (small-to-medium real-time browser games with shared world state). A self-hosted WebSocket server in plain Node.js is a lean alternative with less structure but more control.

Managed platforms (Photon, PlayFab, etc.) exist and have free tiers, but add cost at scale and create lock-in. Given the preference for self-hosted infrastructure, Colyseus on a VPS is the better fit.

### Boat customization

The live world makes boat customization meaningful — a player can recognize a known sailor by their sail configuration and colors. This is a deferred feature within Phase 2, not a prerequisite for it:

- **Phase 2 minimum:** all player boats use the default hull and sail appearance. Identity comes from the player name label above the boat.
- **Phase 2 extended:** hull color, sail color, or a simple insignia selectable from a limited palette. Keeps the visual language consistent while allowing recognition.

Spotting a known vessel by sail configuration alone is a real sailing experience worth preserving. The system should support it when customization is built.

### Player name display

In the live world, a name label floats above each player's boat. Labels should:
- Fade or hide at distance to reduce clutter in busy ports.
- Always show the name label on the local player's own boat.
- Optionally abbreviate (first 6 characters) when many boats are nearby.

---

## Scale

A single server instance handles the initial live world. Vertical scaling (larger machine) is the first tool. Horizontal sharding — multiple world instances each serving a region or shard of the player base — is the correct next step if the game grows to a size where one instance is insufficient.

Scale is not likely to be a relevant concern early. It is worth acknowledging so the architecture does not make sharding impossible to add later (e.g., do not store session state in a way that cannot be moved between instances).

---

## Cross-References

- [design/racing-and-timers.md](racing-and-timers.md) — leaderboard data model, monthly reset, tier snapshot at start line, online leaderboard architecture notes.
- [ux/ui-flows.md](../ux/ui-flows.md) — leaderboard display on arrival screen and map screen.
- [prototype/scope.md](../prototype/scope.md) — multiplayer is explicitly out of scope for the prototype.
