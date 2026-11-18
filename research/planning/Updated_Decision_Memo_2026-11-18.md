# Updated Decision Memo: Novelty and A/B/C Defensibility

**Date:** 18 November 2026  
**Decision status:** Evidence-informed recommendation; no research decision has been changed.  
**Companion document:** [Literature_Update_Oct07-Nov18_2026.md](Literature_Update_Oct07-Nov18_2026.md)

## Recommendation

**Retain the current research question and A/B/C comparison provisionally.** They remain defensible as a narrow incremental-value study, not as a claim that explanation stability, phishing URL abstention, or calibration-plus-explanation evaluation is new in general. Before locking the proposal, inspect the IJAICET full text and conduct a logged final search; then present the novelty boundary to the mentor/research lead.

## Exact research question (unchanged)

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

No wording change is proposed in this memo.

## Why the boundary has narrowed

- Ahamed et al. (Frontiers in Computer Science, published 31 Aug 2026) already evaluate SHAP top-K stability under structured URL perturbations in phishing URL detection. They study stability as an evaluation axis, not as a selective decision signal.
- Patel et al. (IJAICET, page date 1 Nov 2026) report calibration, TreeSHAP behavior and constrained perturbation results. The abstract does not establish abstention or a matched-coverage confidence-versus-stability policy; the full paper remains to inspect.
- Khan’s OACSP-Forensics preprint, Ghalechyan et al.’s interval-based “unknown” approach, and PhishLens cover related uncertainty, selective prediction or routing. They do not establish the exact proposed comparison in the accessible evidence.
- IJREK (28 Oct 2026) proposes feature-selection stability plus uncertainty-based abstention, but does not report experiments and uses global feature stability rather than per-instance explanation stability.

These sources make absolute “first” or “never studied” language indefensible. The plausible contribution is the *empirical decision test* of whether local explanation stability adds selective-risk value beyond calibrated confidence at the same coverage, across in-domain and shifted data.

## A/B/C comparison remains appropriate, with operational definitions

| Arm | Policy | Purpose |
|---|---|---|
| **A — Full-coverage classifier** | The classifier issues a phishing/benign label for every test URL. | Conventional operating reference; report overall and class-sensitive error metrics. It is not a selective policy. |
| **B — Confidence-only** | Use calibrated confidence/uncertainty to accept or defer predictions. The calibration and selection policy are fitted using development data only. | Primary baseline selective policy. Report its risk-coverage curve and risk at stated coverage. |
| **C — Confidence + explanation stability** | Combine the same calibrated confidence signal with a pre-specified local explanation-stability score to select/defer. | Tests whether stability adds information beyond B. Compare B and C at matched coverage on each test condition. |

An explanation-stability-only arm may be retained as a secondary ablation if feasible and approved. It should not replace B as the main comparator. A and B/C answer different operating questions; do not imply A has matched coverage.

## Methodological conditions for a defensible comparison

1. **Define perturbations before evaluating.** Specify which URL transformations preserve the intended URL semantics and the phishing/benign label, and how that is checked. Syntactic validity alone is not proof of semantic validity.
2. **Define explanation stability before testing.** Fix explainer, feature set, perturbation sampling, aggregation, distance/similarity metric, and whether prediction changes are excluded or counted. Stability is not faithfulness or correctness.
3. **Prevent leakage.** Keep training, calibration, policy/threshold selection, and final evaluation data separate. Group by registrable domain/campaign and use an explicit time or external-shift protocol where feasible.
4. **Compare at matched coverage.** Match B and C on the same held-out samples within each domain/shift condition. Report the risk-coverage curves, fixed-coverage risk differences and uncertainty intervals; do not tune on final test labels.
5. **Keep feature conditions aligned.** If the study is URL-only, exclude webpage/HTML-derived features. If multiple feature regimes are used, calibrate and report each separately.
6. **Make cost assumptions explicit.** The primary selective risk should define how false negatives and false positives are weighted. Report sensitivity analyses if the costs are uncertain.

## Recommended primary evaluation and statistics

Retain the project’s previously proposed metrics, with the primary decision endpoint made explicit:

- Predictive performance for A: confusion matrix, phishing precision/recall, specificity/FPR, F1, PR-AUC and ROC-AUC, plus accuracy as secondary context.
- Calibration for B/C: Brier score, log loss, reliability diagram and a clearly defined ECE (including binning protocol); calibration must be evaluated on held-out data.
- Selective performance: coverage, selective risk at pre-registered coverage points, risk-coverage curve and AURC (or a clearly defined normalized variant). State the primary coverage/risk endpoint in advance.
- Stability: pre-specified local attribution similarity (e.g., top-k overlap or rank/attribution similarity), with rationale and sensitivity checks; distinguish score reliability from prediction robustness.
- Uncertainty: bootstrap confidence intervals with resampling at the domain/campaign unit rather than individual URLs where grouping applies. For B versus C, use paired resampling on the same test URLs and report risk difference at matched coverage. If multiple seeds or shift sets are used, report between-split variation.
- Statistical tests should be paired and secondary to effect sizes/intervals. Avoid treating URL-level observations as independent when hosts/campaigns cluster. Freeze the analysis plan before final test use.

These are recommendations to validate with the mentor, not approved final parameter choices. The precise primary metric, target coverage points, cost matrix, bootstrap unit and calibration method remain undecided in existing project records.

## Decision and approvals still needed

- Do not claim “first” or “no prior work.” Use “we test whether … adds value” and identify Ahamed et al. and Patel et al. as close stability work, and OACSP/Ghalechyan/PhishLens as close confidence/selective work.
- Preserve the exact research question unless the research lead approves a revision after reviewing the full IJAICET paper and final search.
- Confirm with mentor: feasibility and scope, URL-only definition, dataset choice and shifted split, perturbation validity protocol, primary stability/risk measures, cost assumptions, deliverables and deadlines.
- Bibliographic fixes are established: Ghalechyan DOI is `10.1038/s41598-024-74725-6`; Buu & Cho’s venue is *Fuzzy Sets and Systems* 517, 109474 (2025), DOI `10.1016/j.fss.2025.109474`. Existing source documents were not edited; corrections are recorded in the separate addendum.

## Immediate next task

**Retrieve and inspect the full IJAICET paper “Robust Explainable and Calibrated Machine Learning for Phishing URL Detection under Constrained Feature Perturbations,” then update the novelty matrix in a separate version/addendum with its exact explanation-stability definition, perturbation protocol, calibration method and whether any selective/abstention policy is actually evaluated.** After that, run a logged final search for exact combinations of “local explanation stability,” “selective prediction/deferral,” “matched coverage,” “phishing URL,” and distribution shift; then take the unchanged research question and A/B/C table to the mentor/research lead for approval. Do not train or implement until the study design is approved.
