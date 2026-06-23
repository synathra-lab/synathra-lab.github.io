# SYNATHRA.Sport v0.5 — Domain-Transfer Validated Football Event-Chain Risk Twin

**Release decision:** `PASS_V0_5_DOMAIN_TRANSFER`

## What it is

SYNATHRA.Sport is an event-chain risk and explanation twin for football event streams. It does not try to predict the final score of a match. Instead, it ranks events and short event chains by their proximity to future attacking danger: shots, high-xG shots, and goals within a short event horizon.

The central operational score in v0.5 is frozen `hybrid_score_v0_1`.

```text
hybrid_score_v0_1_raw =
  0.50 * event_risk_calibrated
+ 0.05 * inside_hardened_chain
+ 0.10 * chain_delta_context
+ 0.35 * subtype_reliability

hybrid_score_v0_1 = normalized deployment score in [0, 1]
```

## What v0.5 proves

v0.5 tests whether the frozen score transfers beyond the original men's international validation contour.

Validation groups:

1. FIFA World Cup 2018
2. FIFA World Cup 2022
3. UEFA Euro 2024
4. Ligue 1 2021/2022
5. Women's World Cup 2023

New transfer domains:

- club football: Ligue 1 2021/2022;
- women's international football: Women's World Cup 2023.

## Main result

For the primary label `future_goal_h10`, frozen `hybrid_score_v0_1` retained strong ranking power across all five groups.

```text
minimum top10_lift across all groups      = 4.6778
mean top10_lift across all groups         = 4.7882
minimum top10_lift across new domains     = 4.6788
minimum ROC-AUC across new domains        = 0.7290
```

## Correct public claim

Frozen `hybrid_score_v0_1` retains event-risk ranking power across men's international tournaments, a club competition, and a women's international tournament.

## What it is not

SYNATHRA.Sport v0.5 is not a bookmaker odds model, not a final-score predictor, and not a universal football model. It is a validated event-chain risk / explanation layer.

## Product value

- explains why certain events matter before a visible shot or goal;
- supports post-match analytical reports;
- can rank dangerous event-chain fragments;
- can support a future near-live event-risk monitor;
- creates a bridge from sports analytics to broader Synathra event-chain reasoning.
