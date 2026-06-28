# Synathra.Sport Ablation Note v0.1 — computation draft

## Purpose

This draft compares an event-only baseline against the frozen full hybrid score for the public v0.5 validation target.

- **Event-only baseline:** `event_risk_calibrated only`.
- **Operational column used:** `synathra_calibrated_score`, because the v0.5 event-level tables do not contain a literal `event_risk_calibrated` column and the public `compute_hybrid_score_v0_1.py` script falls back to `synathra_calibrated_score` for `event_risk_calibrated`.
- **Full score:** `hybrid_score_v0_1`.
- **Target:** `future_goal_h10`, i.e. goal within the next 10 event steps after the evaluated event.
- **Top-10 lift:** top decile by score versus base rate.

## Summary

- Validation groups: **5**.
- Total event rows: **822,283**.
- Total positive `future_goal_h10` rows: **6,012**.
- Mean event-only top-10 lift: **2.57x**.
- Mean full hybrid top-10 lift: **4.79x**.
- Mean absolute lift delta: **2.22x**.
- Mean relative lift delta: **87.4%**.
- Mean event-only ROC-AUC: **0.637**.
- Mean full hybrid ROC-AUC: **0.742**.
- Mean ROC-AUC delta: **0.105**.

## Group-by-group results

| validation_group       | domain_type                          |   observations |   positives |   base_rate |   event_only_top10_lift |   full_top10_lift |   lift_delta_abs |   lift_delta_pct |   event_only_roc_auc |   full_roc_auc |   auc_delta_abs |
|:-----------------------|:-------------------------------------|---------------:|------------:|------------:|------------------------:|------------------:|-----------------:|-----------------:|---------------------:|---------------:|----------------:|
| FIFA World Cup 2018    | same_family_base                     |          71838 |         590 |    0.008213 |                  2.5084 |            4.6609 |           2.1525 |         0.858108 |             0.63692  |       0.741665 |        0.104745 |
| FIFA World Cup 2022    | same_family_cross_season             |         234637 |        1808 |    0.007706 |                  2.594  |            5.0055 |           2.4115 |         0.929638 |             0.632045 |       0.748289 |        0.116244 |
| UEFA Euro 2024         | cross_competition_mens_international |         187924 |        1140 |    0.006066 |                  2.842  |            4.7718 |           1.9298 |         0.679012 |             0.67015  |       0.756462 |        0.086311 |
| Ligue 1 2021/2022      | club_transfer                        |         101766 |         810 |    0.007959 |                  2.5801 |            4.6912 |           2.111  |         0.818182 |             0.625902 |       0.73405  |        0.108148 |
| Women's World Cup 2023 | gender_transfer                      |         226118 |        1664 |    0.007359 |                  2.3257 |            4.8437 |           2.518  |         1.08269  |             0.617615 |       0.729025 |        0.11141  |

## Interpretation

The full `hybrid_score_v0_1` improves the event-only baseline on **top-10 lift in all five validation groups**. The mean lift increases from **2.57x** to **4.79x**. This supports the product claim that the frozen hybrid score is not just an event-risk ranking; the additional chain-context and subtype-reliability terms add measurable ranking value for the `future_goal_h10` target.

The ROC-AUC improvement is smaller but consistently positive in this computation. This is expected because ROC-AUC measures global ranking quality, while the product use case is concentrated in the top-ranked event windows used for report generation.

## Caveat

This is a computation draft for the ablation note. It compares the available event-risk proxy column used by the public scoring script (`synathra_calibrated_score`) against the full frozen score (`hybrid_score_v0_1`). If a future pipeline exports a literal `event_risk_calibrated` column, the ablation should be re-run with that exact column name for strict nomenclature alignment.
