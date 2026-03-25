# Economy

This document defines how trade works in Sailio: goods, pricing, cargo, the starting economy, and the design principles that shape the trading loop.

---

## Design Principles

- **Gold is the single currency.** Upgrades, cargo, and all transactions use gold. Goods exist only to be traded, not crafted or consumed.
- **Producer sells cheap.** Each settlement produces one primary good and sells it at a discount. Settlements that don't produce a good sell it at a premium. Players exploit the spread.
- **The goods are thematic.** All five goods are related to maritime trade and boatbuilding. Players should feel like they are supplying a working coastal economy, not playing an abstract commodity game.
- **Fixed prices in the prototype.** Prices are static per settlement. No dynamic supply/demand, no market events. This keeps the prototype legible and the data model simple.

---

## Goods

Five tradeable goods exist in the prototype. Each has a **baseline price** representing the average across the world. Settlement prices are offsets from this baseline.

| Good | Baseline Price | Flavour | Primary producer type |
|---|---|---|---|
| **Timber** | 40 gold | Cut lumber for hulls and spars | Forest / inland settlement |
| **Rope** | 55 gold | Hemp lines and rigging | Coastal / fishing settlement |
| **Sailcloth** | 75 gold | Woven canvas for sails | Textile / weaving settlement |
| **Ironwork** | 100 gold | Nails, fittings, cleats, chain | Industrial / foundry settlement |
| **Charts** | 130 gold | Navigation maps and sailing guides | Navigator / scholarly settlement |

Goods are ordered roughly by value. A full hold of Charts is worth significantly more than a full hold of Timber — but Charts may be produced by fewer settlements, making the trade route less convenient.

---

## Price Model

Each settlement has a **price modifier** for each good. Modifiers are expressed as a percentage of the baseline:

| Settlement relationship to good | Price modifier range |
|---|---|
| **Primary producer** (makes this good) | 40–50% of baseline (sells cheap) |
| **Neutral** (neither produces nor needs) | 90–110% of baseline (near baseline) |
| **High demand** (needs this good heavily) | 140–160% of baseline (buys expensive) |

The spread between a producer price and a high-demand price is approximately **±60% of baseline**, as intended. This means:

- A unit of Timber bought at a forest settlement (~20 gold) and sold to a settlement that needs it heavily (~64 gold) yields ~44 gold profit per unit
- A unit of Charts bought at a navigator settlement (~65 gold) and sold to a high-demand settlement (~208 gold) yields ~143 gold profit per unit

Higher-value goods are more profitable absolutely, but their producer settlements may be less convenient to route through. This creates genuine cargo selection decisions.

### Price Table (Prototype)

Concrete prices require the five settlements to be defined first. Once [prototype/settlements.md](../prototype/settlements.md) is written, a full price matrix will be added here:

| Good | Settlement A | Settlement B | Settlement C | Settlement D | Settlement E |
|---|---|---|---|---|---|
| Timber | — | — | — | — | — |
| Rope | — | — | — | — | — |
| Sailcloth | — | — | — | — | — |
| Ironwork | — | — | — | — | — |
| Charts | — | — | — | — | — |

*To be filled once settlements are named and assigned.*

---

## Cargo

The player's boat has a **cargo hold** with a fixed number of **cargo slots**. Each unit of a good occupies one slot. Multiple units of the same good stack into the same slot up to a per-slot limit.

| Hold tier | Total slots | Units per slot | Notes |
|---|---|---|---|
| **Base hold** | 4 slots | 3 units each | Max 12 units total |
| **Upgraded hold** (Cargo Capacity upgrade) | 6 slots | 3 units each | Max 18 units total |

> These numbers are starting targets. Slot count and stack size should be tuned so that a full upgraded hold of Charts is lucrative but slow to fill, while Timber is easy to bulk-load.

Cargo has no effect on boat speed (see [sailing-mechanics.md](sailing-mechanics.md)).

---

## Starting Economy

New players begin with:

- **150 gold** (starting purse)
- **2 units of Rope** and **1 unit of Timber** already in the cargo hold

The tutorial directs the player to a **nearby settlement** where **one** of the two starting goods (either Rope or Timber, but not both) is in high demand. This is intentional — it teaches the player that different settlements buy different goods at different prices, without giving them a perfect destination that obviates future exploration. This first run:
- Teaches the player to sail to a destination
- Teaches the player how to sell at a merchant
- Gives the player enough gold to buy goods for a second independent run
- Starts the timer and records their first route time (without the player necessarily realising they are being timed until they arrive)

The starting goods are chosen to be easy to sell at the tutorial destination — not the most profitable possible trade, but a clear and satisfying first transaction.

> Starting values (purse amount, goods) should be tuned during development so the player can comfortably afford one boat upgrade after 2–3 trade runs.

---

## Settlement Economy Design

Each settlement is assigned one **primary good** it produces. It may also have a secondary good it buys at a strong premium (its "want"), creating a natural trade triangle with nearby settlements.

| Settlement role | Produces (sells cheap) | Wants (buys expensive) |
|---|---|---|
| Forest town | Timber | Ironwork |
| Fishing village | Rope | Charts |
| Weaving port | Sailcloth | Timber |
| Foundry harbour | Ironwork | Sailcloth |
| Navigator's outpost | Charts | Rope |

This arrangement ensures that every good has exactly one cheap source and at least one expensive buyer, and that no single two-port trade route dominates the entire economy. Players who chain three ports together will outperform those running a single back-and-forth — without that being a requirement.

*Specific settlement identities are defined in [prototype/settlements.md](../prototype/settlements.md). The roles above map to those settlements once they are named.*

---

## Merchant UI

Each settlement's merchant offers:
- A list of goods **available to buy** (the settlement's stock), with price per unit
- A list of goods **the player is carrying**, with the settlement's sell price per unit
- Current gold balance
- Buy/sell support **partial quantities** — the player selects how many units to transact using a `+` / `−` quantity selector. "Sell all" is a convenience option but not the only option. A player carrying 3 Timber can choose to sell 1, 2, or all 3.

Stock is **unlimited** in the prototype — settlements never run out of goods to sell, and always buy as many units as the player offers.

> **Future:** Stock limits may eventually apply only to goods a settlement does not produce — a forest town has endless Timber, but its Ironwork supply is finite and replenishes slowly. This creates scarcity and makes other traders' activity meaningful.

---

## Future: Dynamic Economy

The following are deferred from the prototype but should not be architecturally prevented:

| Feature | Notes |
|---|---|
| Dynamic pricing / supply and demand | Prices shift based on recent trade volume at each settlement |
| Price event system | Occasional "high demand" or "surplus" flags that temporarily shift prices |
| Multiplayer economy effects | Other players trading affects prices across the shared world — a natural extension of the async multiplayer model |
| Goods as upgrade materials | Future pass may require specific goods at a boatyard, not just gold |
| Cosmetic purchases | Gold may eventually be spent on visual customisation |
| Perishable goods | Time-sensitive cargo that degrades if the voyage takes too long |

