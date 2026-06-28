# Synathra.Sport Technical Method Note v0.1

## Hybrid event-chain scoring, chain activation, and robustness labels

**Version:** Technical Method Note v0.1
**Related release:** Synathra.Sport v0.5 validation release
**Related product artifact:** Synathra.Sport Media & Academy Report v0.1
**Status:** Public technical/product note
**Purpose:** Explain the scoring components and interpretation rules behind the public Synathra.Sport v0.5 report.

---

## 1. Purpose of this note

Synathra.Sport v0.5 is a public validation release for an explainable football event-chain risk framework. The first product-oriented artifact built on top of it is the Synathra.Sport Media & Academy Report v0.1.

The report is designed to translate event-level football data into readable explanations for media analysts, academy coaches, independent analysts, data journalists, and early small-club pilot discussions.

This technical note explains the key terms used in the report:

* `hybrid_score_v0_1`
* `event_risk_calibrated`
* `inside_hardened_chain`
* `chain_delta_context`
* `subtype_reliability`
* `future_goal_h10`
* chain activation
* `risk_peak`
* `risk_delta`
* `robust` / `promising` chain labels

The note is intentionally compact. It is not a full academic paper, not a complete model specification, and not an ablation study. Its role is to make the public report more interpretable and technically honest.

---

## 2. Versioning

Synathra.Sport currently uses several version labels. They refer to different layers.

**Synathra.Sport v0.5** refers to the validation engine and public validation release.

**Media & Academy Report v0.1** refers to the first product-oriented report built from the v0.5 validation release.

**Technical Method Note v0.1** refers to this explanatory note. It defines the main score components and interpretation rules used in the public report.

**Planned v0.2 work** should include an ablation comparison, additional club-domain and women's-football demonstration cards, and a more detailed methodological appendix.

---

## 3. Validation target: `future_goal_h10`

The validation target used in the public snapshot is called:

`future_goal_h10`

In this release, a positive case means:

> a goal occurs within the next 10 event steps after the evaluated event.

The "h10" suffix means "horizon of 10 events." It does not mean 10 seconds, 10 minutes, or the full possession. It is an event-count horizon.

This target is used to test whether the score can rank events so that the highest-ranked cases are richer in near-horizon goal outcomes than the base rate.

The validation figure reports:

* mean top-10 lift: 4.79x
* minimum group lift: 4.68x
* ROC-AUC range: 0.729-0.756
* base rate: approximately 0.6-0.8%

The baseline line in the validation chart is the base-rate reference, normalized to 1.0. A mean top-10 lift of 4.79x means that the top-ranked event windows are about 4.79 times richer in `future_goal_h10` outcomes than the base-rate reference.

This should not be interpreted as a goal probability. It is a ranking-lift measure.

---

## 4. Hybrid score

The public v0.5 score is:

```text
hybrid_score_v0_1 =
0.50 В· event_risk_calibrated
+ 0.05 В· inside_hardened_chain
+ 0.10 В· chain_delta_context
+ 0.35 В· subtype_reliability
```

The score is normalized to the [0, 1] range in the public report.

The formula is deliberately hybrid. It does not claim that the whole ranking signal comes from chain dynamics. Half of the weight comes from calibrated event-level risk, a substantial part comes from subtype reliability, and a smaller but important part comes from chain-context components.

This is the correct reading:

> Synathra.Sport combines calibrated event risk, chain context, and subtype reliability. The chain layer is not the entire score; it is the explanatory context that helps translate a ranked event-risk signal into readable football sequences.

---

## 5. Component definitions

### 5.1. `event_risk_calibrated`

`event_risk_calibrated` is a normalized continuous event-level risk estimate on a 0-1 scale.

It represents how risk-relevant a single event appears before chain-context terms are added.

Interpretation:

* low values indicate weak local risk signal;
* high values indicate stronger local event-level risk signal;
* the value is not a direct probability of a goal.

Role in the formula:

```text
weight = 0.50
```

This is the largest component of the public hybrid score.

---

### 5.2. `inside_hardened_chain`

`inside_hardened_chain` is a binary indicator.

It takes the value:

```text
1 = the event belongs to a filtered and stabilized chain pattern
0 = the event does not belong to such a pattern
```

The word Hardened means that the chain pattern has passed the current rule/filter layer. It should not be read as a subjective tactical judgement.

A hardened chain is a candidate event sequence that has survived basic filtering conditions such as:

* the sequence is locally coherent;
* the events occur within the selected event window;
* the chain is not a loose verbal label only;
* the sequence can be mapped to the public chain grammar used in the report.

Role in the formula:

```text
weight = 0.05
```

This component is intentionally small. It does not dominate the score, but it marks whether the event belongs to an interpretable chain pattern.

---

### 5.3. `chain_delta_context`

`chain_delta_context` is a normalized continuous measure of local risk change inside the selected event window.

It captures whether the local sequence shows meaningful risk acceleration or risk accumulation.

A simplified interpretation is:

> how much the local risk context rises inside the chain window.

Role in the formula:

```text
weight = 0.10
```

Together, `inside_hardened_chain` and `chain_delta_context` represent the explicitly chain-related part of the current public score.

Their combined weight is:

```text
0.05 + 0.10 = 0.15
```

This is why the v0.5 score should not be described as a pure chain-only model. It is a hybrid event-risk model with an explanatory chain layer.

---

### 5.4. `subtype_reliability`

`subtype_reliability` is a normalized reliability weight attached to the event subtype.

It reflects how stable or informative the event subtype is in the validation setup.

Interpretation:

* higher values indicate event subtypes that are more reliable for ranking risk-relevant cases;
* lower values indicate event subtypes with weaker or less stable signal;
* the value is normalized and should not be read as an independent causal effect.

Role in the formula:

```text
weight = 0.35
```

This component gives the model stability by accounting for event-subtype reliability.

---

## 6. Chain activation

A chain activation occurs when a local event sequence is selected as a risk-relevant sequence under the current scoring and filtering logic.

In the public v0.5 report, a chain activation is not merely a tactical phrase such as "transition" or "set-piece pressure." It is a selected event window where several conditions come together:

1. The event window contains a coherent sequence of football events.
2. The sequence includes events that can be mapped to a public chain type.
3. The events show a sufficiently strong hybrid score signal.
4. The local window shows a meaningful risk change or risk peak.
5. The event subtypes have sufficient reliability under the current validation setup.
6. The sequence can be translated into a readable media or academy interpretation.

The current report uses event windows rather than full-match tactical phases. A selected chain card therefore describes a local episode, not the entire tactical identity of a team.

A simplified chain-activation logic can be written as:

```text
event sequence
-> local scoring window
-> hybrid score calculation
-> chain-context check
-> risk peak / risk delta extraction
-> chain label assignment
-> media and academy interpretation
```

This makes the chain card a reporting artifact rather than a black-box prediction.

---

## 7. Chain types in the public report

The public Media & Academy Report v0.1 uses demonstration chain types such as:

* Transition Pressure Chain
* Corner Pressure Chain
* Free-Kick Second-Phase Chain
* Duel-to-Shot Chain
* Open-Play Goal Chain

These names are not meant to be final universal taxonomy. They are public-facing labels that translate selected event sequences into readable football language.

The technical role of the chain type is to summarize the dominant pattern in the selected event window.

For example:

**Transition Pressure Chain** means that the selected event window is interpretable as danger forming through a contested or unstable transition phase.

**Corner Pressure Chain** means that the selected event window is interpretable as danger forming through a set-piece or second-ball pressure pattern.

**Open-Play Goal Chain** means that the selected event window is interpretable as danger forming through open-play progression before the visible endpoint.

---

## 8. `risk_peak`

`risk_peak` is the maximum normalized `hybrid_score_v0_1` observed inside the selected event window.

It is reported on a 0-1 scale.

A value such as:

```text
risk_peak = 0.893
```

should be read as:

> the selected chain window contains a very high normalized hybrid risk point.

It should not be read as:

```text
89.3% probability of a goal
```

The public report uses `risk_peak` as a ranking and interpretation signal, not as a direct probability statement.

---

## 9. `risk_delta`

`risk_delta` is the local increase in normalized risk inside the selected event window.

It describes how much the risk score rises from a lower-risk part of the window toward the peak-risk point.

A value such as:

```text
risk_delta = 0.5825
```

should be read as:

> the selected chain window contains a large local rise in normalized risk.

It does not necessarily mean that one specific event caused the goal. Football sequences are multi-factor and context-dependent.

The purpose of `risk_delta` is to help distinguish relatively flat high-risk windows from windows where risk visibly accelerates inside the sequence.

---

## 10. Robustness labels

The public demonstration cards may use two provisional labels:

* `robust`
* `promising`

These are communication labels for the public report. They should not be treated as final statistical classes.

### 10.1. `robust`

A chain example can be labelled `robust` when it satisfies the current report-level conditions:

1. The chain window has a high normalized risk peak.
2. The local risk delta is meaningful.
3. The sequence is coherent and football-readable.
4. The sequence can be mapped to a hardened chain type.
5. The event subtypes have sufficient reliability.
6. The episode supports both media interpretation and academy interpretation.
7. The selected outcome or near-horizon target is consistent with the reported chain logic.

A `robust` label means:

> this is a strong public demonstration example under the current v0.5 report logic.

It does not mean:

> this chain type is universally validated in all football contexts.

### 10.2. `promising`

A chain example can be labelled `promising` when it is interpretable and high-signal, but one or more report-level conditions are weaker.

Typical reasons:

* the risk peak is high but not as strong as in robust examples;
* the local risk delta is meaningful but less stable;
* the sequence is football-readable but requires more examples;
* the chain type is useful as a demonstration but needs further validation across domains;
* the example is suitable for media or academy interpretation but should not yet be treated as a stable benchmark chain.

A `promising` label means:

> this chain is a useful candidate for further testing, expansion, and video-review validation.

It does not mean the example is invalid. It means that the evidence level is weaker than for `robust` demonstration cases.

---

## 11. Baseline and ablation status

The public v0.5 validation snapshot reports the performance of the frozen `hybrid_score_v0_1`.

It does not yet report a full ablation comparison between:

```text
event_risk_calibrated only
```

and:

```text
full hybrid_score_v0_1
```

This comparison is important because it will show how much additional ranking value comes from chain context and subtype reliability beyond event-level risk alone.

A future technical note should report at least:

* event-only top-10 lift;
* full hybrid-score top-10 lift;
* difference in lift;
* event-only ROC-AUC;
* full hybrid-score ROC-AUC;
* group-by-group comparison across the five validation groups.

Until that ablation is published, the correct claim is:

> The frozen hybrid score produces a stable ranking signal across five football validation groups.

The stronger claim:

> The chain-context components independently improve the event-only baseline by X%

should be reserved for the ablation release.

---

## 12. Domain-transfer interpretation

Synathra.Sport v0.5 tests the same frozen scoring logic across five groups:

* FIFA World Cup 2018
* FIFA World Cup 2022
* UEFA Euro 2024
* Ligue 1 2021/2022
* Women's World Cup 2023

The domain-transfer logic is:

```text
base contour
-> frozen score
-> club transfer
-> gender transfer
```

The important methodological point is that the transfer groups are not locally reweighted in the public validation snapshot.

This supports the public claim that the same score can be tested across multiple football contexts.

However, the current v0.1 demonstration cards are still concentrated in men's international football. A later report should add public demonstration cards from:

* Ligue 1 2021/2022;
* Women's World Cup 2023.

This will make the domain-transfer claim visible not only in the validation table, but also in concrete chain examples.

---

## 13. Relationship to xT, VAEP, and xGChain

Synathra.Sport should not be presented as a replacement for existing football analytics methods.

The distinction is functional:

**xT** values how actions move the ball into more threatening zones.

**VAEP-style models** estimate how actions change the probability of scoring or conceding.

**xGChain and xGBuildup** attribute involvement in attacking sequences to players.

**Synathra.Sport** ranks risk-relevant event windows and translates selected sequences into readable chain explanations.

The most natural output is not a player leaderboard. It is a report artifact:

* risk-chain of the match;
* turning-point chain;
* transition-pressure chain;
* set-piece second-phase chain;
* team risk-chain profile;
* coach education note.

A future technical comparison should apply xT, VAEP, xGChain, and Synathra.Sport to the same selected episodes. That comparison is not included in v0.1.

---

## 14. Current limitations

The v0.5 / v0.1 public release has several important limitations.

First, the public score is hybrid and should not be described as a pure sequence model.

Second, the current public note does not include full ablation results.

Third, the demonstration chain cards are not yet balanced across all validation domains.

Fourth, the robustness labels are provisional report-level labels rather than final statistical classes.

Fifth, the score is a ranking and explanation signal, not a direct probability of scoring.

Sixth, the current report does not replace video analysis, tracking data, club-specific tactical models, or licensed data-provider workflows.

These limitations are not defects of the public release. They define the next development steps.

---

## 15. Recommended next technical steps

The next technical release should include:

1. Ablation study:

   * `event_risk_calibrated` only;
   * full `hybrid_score_v0_1`;
   * group-by-group lift and ROC-AUC comparison.

2. Transfer-domain chain cards:

   * at least one Ligue 1 2021/2022 example;
   * at least one Women's World Cup 2023 example.

3. Formal chain-window rules:

   * event-window size;
   * start condition;
   * end condition;
   * interruption logic;
   * minimum score thresholds.

4. Robustness criteria refinement:

   * numeric thresholds for `risk_peak`;
   * numeric thresholds for `risk_delta`;
   * recurrence criteria by chain type;
   * domain-transfer presence.

5. Same-episode comparison:

   * Synathra.Sport vs xT;
   * Synathra.Sport vs VAEP;
   * Synathra.Sport vs xGChain / xGBuildup.

---

## 16. Summary

Synathra.Sport v0.5 uses a frozen hybrid score:

```text
hybrid_score_v0_1 =
0.50 В· event_risk_calibrated
+ 0.05 В· inside_hardened_chain
+ 0.10 В· chain_delta_context
+ 0.35 В· subtype_reliability
```

The current public interpretation is:

> calibrated event risk provides the main local signal; subtype reliability stabilizes the ranking; chain context turns the score into a readable sequence explanation.

The product value is not only the numeric score. It is the translation of selected event windows into media-readable and coach-readable chain cards.

That is the core role of Synathra.Sport at this stage:

> event-chain risk intelligence for explaining how football danger forms before it becomes visible.


