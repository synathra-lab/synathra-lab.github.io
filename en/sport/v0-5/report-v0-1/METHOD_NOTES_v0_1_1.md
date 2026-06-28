# Synathra.Sport report site patch v0.1.1 - method notes

This documentation patch clarifies the public Media & Academy Report v0.1 page. It does not change Synathra.Sport v0.5 validation metrics.

## Target

`future_goal_h10` is positive when a goal occurs within the next 10 event steps after the evaluated event.

## Formula

```text
hybrid_score_v0_1_raw =
  0.50 * event_risk_calibrated
+ 0.05 * inside_hardened_chain
+ 0.10 * chain_delta_context
+ 0.35 * subtype_reliability
```

## Term definitions

- `event_risk_calibrated`: normalized continuous event-level risk estimate.
- `inside_hardened_chain`: binary indicator that the event belongs to a filtered/stabilized chain pattern.
- `chain_delta_context`: normalized local risk increase inside the selected event window.
- `subtype_reliability`: normalized reliability weight for the event subtype.

## Chain-card metrics

- `risk_peak`: maximum normalized hybrid score inside the selected window.
- `risk_delta`: local increase from lower-risk part of the window to the peak.

## Robustness labels

- `robust`: stronger public demonstration subtype after current filter/rule layer.
- `promising`: useful demonstration subtype that needs more cross-domain support.

## Versioning

- Synathra.Sport v0.5 = validation engine / public release.
- Media & Academy Report v0.1 = first product report.
- Site patch v0.1.1 = documentation clarification patch.


