# Sailio

> **Work in progress.** This project is in early design and prototyping.

Sailio is a sailing and exploration game built in [Godot 4](https://godotengine.org/) using GDScript. Players sail a small boat across a hand-crafted world, visiting settlements to trade goods and upgrade their vessel. Every voyage between settlements is timed and ranked — the core concept being that any two sailors on the same route are always, implicitly, racing.

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
| [docs/design/world-activities.md](docs/design/world-activities.md) | Fishing, instruments, floating cargo, and other non-sailing world activities |
| [docs/ux/ui-flows.md](docs/ux/ui-flows.md) | Screens, HUD, and menus |
| [docs/art/direction.md](docs/art/direction.md) | Visual style, palette, tone |
| [docs/prototype/scope.md](docs/prototype/scope.md) | What is in v0.1 and what is explicitly deferred |
| [docs/prototype/settlements.md](docs/prototype/settlements.md) | The 4–5 prototype settlements defined |

## Technology

- **Engine:** Godot 4
- **Language:** GDScript
- **Rendering:** 3D with a fixed camera angle
- **Target platform:** [itch.io](https://itch.io)

## A Note on AI Assistance

This project is developed with transparency about the role of AI tools.

**Documentation and code logic** are developed with the assistance of AI (specifically, GitHub Copilot). This includes design specification documents, implementation planning, and — to varying degrees — code written during development. In all cases, the author reviews and takes responsibility for the output.

**Game assets and artwork** — including 3D models and visual design — are made by the author without AI generation tools, using Blender and Inkscape.

The author's intent is to understand how this game works at every level. Working in Godot directly makes this unavoidable: every scene, node, and script is touched and reasoned through by hand. AI assistance is a productivity tool, not a replacement for that understanding.

This project is sometimes described as *vibe-coded* — a shorthand for AI-assisted development. That label is used here honestly and without pretense.
