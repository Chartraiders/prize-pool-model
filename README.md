# Prize Pool Distribution Model

**Chart Raiders · Rade Engine Module · Dynamic Payout Scaling Engine**

The Chart Raiders Prize Pool Distribution Model calculates and allocates competitive match payouts across any number of players, any entry tier, and any match size — from 2 players to 10,000+. It guarantees full distribution, fair payouts, and mathematical correctness at every scale.

---

## Platform Classification — Read First

Before the mechanics: understanding what these prize pools are is important.

**Chart Raiders is a peer-to-peer skill-based contest platform.** Players compete against each other for rank. The prize pool is formed from ticket sales and distributed back to players by rank. Chart Raiders charges a platform fee (1–10%) and does not participate as a player or act as a counterparty.

**Players never buy, sell, or hold any securities.** Prize pools are not tied to the direction or value of any underlying asset. Payouts are determined entirely by where players finish relative to each other — player skill and trading decisions are the intervening variable between market data and contest outcome.

**Chart Raiders is a Game of Skill.** The prize distribution model exists to reward the players who performed best — defined by their trading decisions across the duration of the match, not by a single market outcome.

All prize amounts, distribution formulas, and the ticker being analyzed are disclosed to all players **before the match begins**.

---

## The Five Guarantees

These hold in every match, no exceptions:

1. **The pool always equals what players paid in.** Every dollar of ticket sales goes into the prize pool. Nothing is created from thin air.
2. **Higher rank always means a bigger prize.** No lower-ranked player ever earns more than a higher-ranked one. Mathematically guaranteed.
3. **The top 60% of players receive a payout.** In standard matches (50+ players), the top three-fifths of finishers earn something.
4. **Prizes scale cleanly.** Doubling entrants doubles every prize. Moving from $10 to $100 tickets multiplies every prize by exactly 10.
5. **One model covers all three ticket tiers.** The percentage structure is identical at $10, $100, and $1,000 — only the dollar amounts differ.

---

## The Formula

```
Prize Pool = Number of Entrants × Ticket Price

Payout Pool = Prize Pool

Each prize = Payout Pool × Prize Percentage
```

Prizes are defined as **percentages of the pool** — not fixed dollar amounts. This is the most important design decision: a percentage-based model works for any player count automatically, without needing separate tables for every scenario.

---

## Prize Structure — Two Layers

### Layer 1: Individual Top-9 Prizes (68.5% of pool)

Each of the top 9 finishers receives their own fixed slice of the payout pool:

| Rank | % of Pool | Prize (100 × $10) | Prize (100 × $100) | Prize (100 × $1,000) |
|---|---|---|---|---|
| **1st** | **30.21%** | $302.10 | $3,021.00 | $30,210.00 |
| 2nd | 13.25% | $132.50 | $1,325.00 | $13,250.00 |
| 3rd | 6.04% | $60.40 | $604.00 | $6,040.00 |
| 4th | 4.38% | $43.80 | $438.00 | $4,380.00 |
| 5th | 4.19% | $41.90 | $419.00 | $4,190.00 |
| 6th | 3.41% | $34.10 | $341.00 | $3,410.00 |
| 7th | 2.83% | $28.30 | $283.00 | $2,830.00 |
| 8th | 2.34% | $23.40 | $234.00 | $2,340.00 |
| 9th | 1.85% | $18.50 | $185.00 | $1,850.00 |
| **Total** | **68.50%** | **$685.00** | **$6,290.00** | **$62,500.00** |

### Layer 2: Flat Bracket Tiers (31.5% of pool)

Ranks 10 through the 60th percentile are grouped into five bracket tiers. Boundaries are defined as percentages of total entrants — they adjust automatically as match size changes.

| Tier | Percentile Range | Pool Share | Per Person (100 × $10) |
|---|---|---|---|
| A | Top 10–13% | 5.70% | $14.25 |
| B | Top 14–19% | 6.85% | $11.42 |
| C | Top 20–29% | 6.64% | $6.64 |
| D | Top 30–43% | 6.64% | $4.74 |
| E | Top 44–60% | 5.67% | $3.34 |
| **Total** | | **31.50%** | |

**Combined: 68.50% + 31.50% = 100% of payout pool.**

---

## Scaling Across Match Sizes

| Entrants | Ticket | Prize Pool | 1st Place | 2nd Place | 3rd Place |
|---|---|---|---|---|---|
| 50 | $10 | $500 | $151.05 | $66.25 | $30.20 |
| 100 | $10 | $1,000 | $302.10 | $132.50 | $60.40 |
| 200 | $10 | $2,000 | $604.20 | $265.00 | $120.80 |
| 500 | $10 | $5,000 | $1,510.50 | $662.50 | $302.00 |
| 100 | $100 | $10,000 | $3,021.00 | $1,325.00 | $604.00 |
| 100 | $1,000 | $100,000 | $30,210.00 | $13,250.00 | $6,040.00 |

---

## The Anti-Inversion Guarantee

A **prize inversion** — where a lower-ranked player earns more than a higher-ranked one — must never happen. For large matches (50+ players), the tier structure prevents it naturally. For small matches, the model uses **adaptive tier merging**: adjacent tiers automatically merge when an inversion would occur, repeating until no inversions remain.

Result: zero inversions for any player count from 2 to 10,000+.

---

## Small Tournament Handling

| Players | Players Paid | % of Pool Paid | Notes |
|---|---|---|---|
| 5–9 | All | 100% | Individual prizes only |
| 10–14 | All | ~70–78% | Tiers merge to prevent inversions |
| 20 | 14 of 20 | ~78% | Bottom 6 receive nothing |
| 50 | 30 of 50 | 100% | Full tier structure activates |
| 100+ | Top 60% | 100% | Full structure, scales linearly |

---

## Platform Fee

A platform fee of 1–10% is deducted during checkout before prize calculation. All prizes are calculated against the post-fee payout pool.

**Example: 100 players, $10 ticket**
- Gross pool: $1,000
- Fee: $0
- Payout pool: $1000
- 1st place: 30.21% × $1000 = **$302.10**

**Example: 100 players, $100 ticket**
- Gross pool: $10,000
- Fee: $0
- Payout pool: $10000
- 1st place: 30.21% × $10000 = **$3021.00**

**Example: 100 players, $1000 ticket**
- Gross pool: $100,000
- Fee: $0
- Payout pool: $100000
- 1st place: 30.21% × $100000 = **$30210.00**

---

## Validation

The model was validated against 10 test cases across all major scenarios. Zero inversions across all cases. Full distribution confirmed for all N ≥ 50.

---

## Integration

This prize engine runs inside **Rade Engine**. It connects to REMM (which determines match dynamics and match length) and REMMR (which processes post-match rating updates). All prize calculations happen server-side — the client is a display layer.

→ **See public matches:** https://chartraiders.com/matches  
→ **Prize FAQ:** https://chartraiders.com/faq  
→ **Creator:** c0tt0nc4ndyta — Founder of Chart Raiders, Inventor of DayTradeSport

---

*Chart Raiders is the world's first DayTradeSport platform. Prize pools are peer-to-peer — distributed by rank among competitors. Players never hold securities. Game of Skill.*
