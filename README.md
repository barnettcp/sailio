# Sailio

> **Work in progress.** This project is in early design and prototyping.

Sailio is a sailing and exploration game built in [Godot 4](https://godotengine.org/) using GDScript. Players sail a small boat across a hand-crafted world, visiting settlements to trade goods and upgrade their vessel. Every voyage between settlements is timed and ranked — the core conceit being that any two sailors on the same route are always, implicitly, racing.

## Status

The project is currently in the design phase. Specification documents are being written in [`docs/`](docs/) before implementation begins.

## Design Docs

| Document | Description |
|---|---|
| [docs/overview.md](docs/overview.md) | Vision, pillars, and what the game is |
| [docs/design/sailing-mechanics.md](docs/design/sailing-mechanics.md) | Wind, movement, sail configurations, speed model |
| [docs/design/world.md](docs/design/world.md) | Map philosophy, geography, settlement placement |
| [docs/design/economy.md](docs/design/economy.md) | Goods, pricing, trade routes |
| [docs/design/progression.md](docs/design/progression.md) | Boat upgrades, tiers, tradeoffs |
| [docs/design/settlements.md](docs/design/settlements.md) | Port services, merchant design |
| [docs/design/racing-and-timers.md](docs/design/racing-and-timers.md) | Timer logic, leaderboards, route ranking |
| [docs/design/idle-activities.md](docs/design/idle-activities.md) | Fishing, instruments, and other modular idle systems |
| [docs/ux/ui-flows.md](docs/ux/ui-flows.md) | Screens, HUD, and menus |
| [docs/art/direction.md](docs/art/direction.md) | Visual style, palette, tone |
| [docs/prototype/scope.md](docs/prototype/scope.md) | What is in v0.1 and what is explicitly deferred |
| [docs/prototype/settlements.md](docs/prototype/settlements.md) | The 4–5 prototype settlements defined |

## Technology

- **Engine:** Godot 4
- **Language:** GDScript
- **Rendering:** 3D with a fixed camera angle
- **Target platform:** [itch.io](https://itch.io)
