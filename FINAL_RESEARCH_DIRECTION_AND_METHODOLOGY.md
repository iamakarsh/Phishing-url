# Final Research Direction and Proposed Methodology

**Project:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability  
**Document purpose:** Consolidated research plan for discussion with the university mentor  
**Status:** Proposed study design. No capstone model training, implementation, or experiments are recorded as complete. Research choices marked open below require mentor/research-lead approval.

## 1. Research direction at a glance

The project will investigate whether the stability of a model’s local explanation across carefully validated URL variants can help decide which phishing/benign predictions are safe to issue automatically. The comparator is a selective policy based on calibrated confidence alone. The key comparison is between those policies at the same coverage, evaluated in-domain and under a clearly defined distribution shift. A conventional all-predictions classifier is retained as the full-coverage reference.

The study does **not** claim that phishing URL classification, SHAP, calibration, uncertainty, selective prediction, or explanation stability is new by itself. The literature already covers each of these and includes studies combining several of them. The proposed contribution is a narrowly specified empirical decision test of the incremental value of local explanation stability over calibrated confidence. Whether that exact gap survives a complete search and full-text screening remains provisional.

## 2. Research question (unchanged)

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

This wording is retained exactly from the approved project materials. This document does not revise it.

## 3. Literature review synthesis

### 3.1 What the literature already establishes

Phishing URL detection has been studied with lexical and structural feature models, tree ensembles, neural models, and raw character/token representations. SHAP and LIME have been applied to phishing URL or website classifiers for feature explanation and selection. Their use alone is not a novelty claim.

Probability quality, uncertainty and deferral also have prior work. Ghalechyan et al. (2024) study probabilistic neural-network weight uncertainty and an “unknown” outcome when an interval crosses a decision threshold. Their work is uncertainty-based deferral, but the available evidence does not establish a formally calibrated selective policy or explanation-stability signal. The corrected publisher DOI is **10.1038/s41598-024-74725-6**; the Phase 2 document’s alternate DOI is erroneous. [Publisher full text](https://www.nature.com/articles/s41598-024-74725-6)

Khan’s **OACSP-Forensics** (SSRN preprint, posted 1 August 2026) is a close selective-prediction study. Its abstract describes an ordinal-aware class-conditional selective head, post-hoc temperature scaling, coverage-risk analysis, PhiUSIIL, and external URLhaus phishing with Tranco benign controls. The abstract also reports a PhiUSIIL feature audit. The full paper could not be inspected in the available review; treat it as a preprint and its detailed claims as abstract-level. It covers confidence-based selective prediction and shift, but the accessible abstract does not describe local explanation stability. [SSRN record](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7140758)

Distribution shift and split design are established concerns. Rashid et al. (2024) evaluate cross-dataset generalization and unsupervised domain adaptation for phishing URL detection. Ghalechyan et al. also report limitations in source tracking and label quality in mixed-source settings. These motivate domain/time-aware evaluation but do not answer whether local explanation stability improves selective decisions. [Rashid et al.](https://doi.org/10.1016/j.comnet.2024.110398)

### 3.2 Closest explanation-stability and perturbation studies

**Ahamed et al. (2026)** provide the clearest direct precedent for URL-level explanation stability. Their *Frontiers in Computer Science* study evaluates LR, XGBoost, CharCNN and BERT using processed PHIUSIIL and PhishTank data, with structured perturbations, external evaluation and SHAP top-K stability. The paper reports SHAP top-K Jaccard similarity declining to approximately 0.526–0.535 under subdomain perturbation. It studies stability as a robustness/evaluation outcome; the reviewed paper does not test a calibrated confidence-only selective policy against a confidence-plus-stability policy at matched coverage. Its external construction and which analyses cover which model families require care. [Publisher full text](https://www.frontiersin.org/journals/computer-science/articles/10.3389/fcomp.2026.1834407/full)

**Patel et al. (2026)**, “Robust Explainable and Calibrated Machine Learning for Phishing URL Detection under Constrained Feature Perturbations,” is a close adjacent study. Its publisher abstract reports PhiUSIIL, a normalized domain-grouped development protocol and one frozen test assessment; five model families; conservative and full-sensitivity feature conditions; calibration analysis; TreeSHAP; and greedy feature-space evasion. The abstract reports strong clean-test metrics for conservative uncalibrated HistGradientBoosting but also high attack success at three/five feature changes and mean SHAP cosine similarity of −0.350. The full paper and author manuscript could not be accessed despite documented attempts. Exact fields, split protocol, calibration procedure, perturbation semantics, SHAP computation and any selective policy are therefore **unknown**. The audit does not infer that selective prediction is absent merely because the abstract omits it. [Publisher abstract/record](https://ijaicet.com/index.php/ijaicet/article/view/41) · [Full-paper access audit](PATEL_IJAICET_FULL_PAPER_AUDIT.md)

**Panthi et al. (2026)** compare SHAP and LIME for boosting models in phishing URL detection. The reviewed record reports explanation analyses and global consistency but does not establish local explanation-stability-based deferral or the target matched-coverage comparison. [Publisher article](https://www.nature.com/articles/s41598-026-66800-x)

### 3.3 Other important adjacent work

- **PhishLens (October 2026)** uses an uncertainty band to route ambiguous phishing website cases to a local LLM for verified evidence selection. The classifier label and probability remain frozen; this routes the explanation/evidence pathway rather than clearly withholding the classifier’s phishing/benign decision. It does not report the capstone’s local attribution-stability signal or matched-coverage B/C comparison. [Publisher article](https://ijemr.vandanapublications.com/index.php/j/article/view/1951) · [Full paper](https://ijemr.vandanapublications.com/index.php/j/article/download/1951/2070/4111)
- **Ghalechyan et al. (2024)** show that uncertainty intervals and an unknown outcome have precedent for URL classification; their uncertainty is not the same as explanation stability.
- **The IJREK article (28 October 2026)** proposes feature-selection/ranking stability across resamples or time together with uncertainty-based abstention. The article describes benefits as requiring experimental demonstration; no measured benefit was established in the accessible record. Its stability is global feature-selection consistency, not per-instance local explanation stability. Its peer-review status was not independently established in the project’s review. [Article record](https://www.ijrek.org/article/IJREK-2026-00157)
- **MAG-Phish X (November 2026)** is relevant to calibration and feature availability. Its reported threshold mismatch across complete-evidence and URL-only feature conditions illustrates why calibration and policy selection must match the evaluated input condition. It is not a local explanation-stability selective-policy comparison. [Publisher article](https://www.comdem.org/index.php/comdem/article/view/19008)
- **Khan et al. (October 2026)** combine SHAP-guided feature learning, Brier/reliability analysis and threshold/risk discussion for phishing URL detection. A Brier score or reliability plot alone does not prove that a post-hoc calibration method was fitted; the reviewed record does not establish matched-coverage selective evaluation or explanation-stability-driven decisions. [Publisher article](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2026.1854934/full)
- **Rashid et al. (2024)** and later temporal/cross-dataset studies support explicit shift evaluation. A domain-grouped split by itself is not a distribution-shift test; temporal or external shift must be separately defined.
- **Buu & Cho (2025)** is “A Transformer network calibrated with fuzzy logic for phishing URL detection,” *Fuzzy Sets and Systems* 517, article 109474, DOI 10.1016/j.fss.2025.109474. The DOI-linked venue resolves the Phase 2 venue conflict. Exact calibration interpretation and detailed results need full-text verification; fuzzy rules should not automatically be treated as statistical probability calibration. [Publisher record](https://www.sciencedirect.com/science/article/pii/S0165011425002131)
- **HIEF (September 2026)** concerns phishing email rather than URLs. The project’s prior review notes an AURC comparison favoring calibrated probability over evidential uncertainty; those specific values were not reverified against the accessible publisher abstract in the refresh. Treat them as unverified until the article body/supplement is checked. It is adjacent caution, not URL-specific evidence. [Publisher article](https://www.mdpi.com/1999-4893/19/9/741)

### 3.4 Evidence status and review limitations

The source base includes full publisher text for some studies, publisher abstracts/records for others, and repository/preprint abstracts. These statuses are identified in the existing [literature update](Literature_Update_Oct07-Nov18_2026.md) and [Patel audit](PATEL_IJAICET_FULL_PAPER_AUDIT.md). The review is a targeted, concept-led scoping review, not a reproducible systematic review: the prior project has not logged database search strings, screening decisions, deduplication, or full-text eligibility for all records. Search access to subscription databases and all supplements was not confirmed. Accordingly, “no direct match found” means none was identified in the inspected material, not that no such study exists.

## 4. Precise research gap and its limits

### Proposed gap

The reviewed evidence includes (a) explanation stability under phishing URL perturbations, (b) calibrated confidence/uncertainty and selective prediction or deferral for phishing-related tasks, and (c) work that combines calibration, explanation analysis and perturbation robustness. What remains **insufficiently established in the accessible evidence** is whether a per-instance local explanation-stability score, computed under semantically valid URL perturbations, contributes incremental selective-risk reduction beyond a properly calibrated confidence-only policy when the two policies accept the same proportion of cases, including under in-domain and shifted evaluation.

This is a testable empirical gap, not a claim that the component ideas are new. The gap may be narrowed or closed by the inaccessible Patel full text, unreviewed papers, a complete database search, or later publications. Ahamed et al. make clear that URL explanation stability itself is prior work. OACSP and Ghalechyan make clear that uncertainty and selective decisions are prior work. Patel et al. prevent a definitive distinction until its full text can be inspected.

### Novelty boundary

The project may state: **“We evaluate whether local explanation stability adds selective decision value beyond calibrated confidence at matched coverage under in-domain and shifted phishing URL evaluation.”** It should not state “first,” “novel use of SHAP,” “first explanation stability for phishing,” “first uncertainty-based phishing detection,” or “no previous study combines explainability and calibration.”

The proposed study is distinct only if it operationalizes explanation stability as a per-instance decision signal and directly compares policy B with policy C at matched coverage. It must also avoid leakage and establish perturbation validity. If those elements are not implemented as planned, the current gap would not be tested.

## 5. Research objectives

1. Establish a leakage-aware phishing/legitimate URL classification baseline using documented lexical/structural inputs.
2. Assess and, where suitable, calibrate predicted class probabilities using data separate from final testing.
3. Generate local explanations for individual model decisions and define a reproducible per-instance explanation-stability score under validated URL perturbations.
4. Compare conventional full-coverage classification, confidence-only selective prediction, and confidence-plus-stability selective prediction.
5. Test predictive, calibration and selective-risk behavior in-domain and under an explicitly defined temporal or external distribution shift.
6. Quantify uncertainty in B-versus-C risk differences at matched coverage and report limitations, failure cases and validity threats.
7. If required by the university capstone scope, use the evaluated method later in a demonstration prototype; prototype development is downstream of research-plan approval and experiments.

## 6. Research hypotheses

These hypotheses are carried forward from Phase 2 as proposed, testable statements; none has been tested by this capstone.

- **H1:** Lower local explanation stability is associated with higher prediction error after accounting for calibrated confidence, model, class and evaluation domain.
- **H2:** At matched coverage, a confidence-plus-stability policy has lower selective risk than a calibrated confidence-only policy.
- **H3:** The incremental value of explanation stability is larger under temporal/domain shift and perturbation families that cause attribution drift.
- **H4:** Invalid or meaning-changing URL edits can produce misleading stability signals; therefore perturbation validity must be controlled and reported.

H1 is associational; it does not by itself establish causation. H2 is the primary policy comparison. H3 is a conditional/interaction hypothesis and may require a larger sample and multiple shift sets. H4 motivates validation and sensitivity analysis rather than presuming all perturbations are invalid.

## 7. A/B/C experimental comparison

| Arm | Operational definition | Role and comparison |
|---|---|---|
| **A — Conventional full-coverage classifier** | The selected classifier issues phishing or legitimate for every eligible URL. No case is deferred. | Full-coverage reference for discrimination, class errors and calibration. A is not a selective policy and should not be described as matched in coverage to B/C. |
| **B — Calibrated-confidence-only selective policy** | The classifier’s probability is calibrated on development/calibration data. A pre-specified confidence/uncertainty score ranks cases; the policy accepts the highest-confidence cases and defers the remainder. | Primary baseline selective policy. The calibration and selection rule are frozen before final test evaluation. Report its risk at stated coverages and risk-coverage curve. |
| **C — Confidence plus local explanation stability** | Use the same underlying classifier and calibrated confidence signal as B, plus a pre-specified local explanation-stability score. A pre-specified combination/ranking rule accepts or defers cases. | Tests the incremental value of explanation stability. The primary contrast is C versus B on the same held-out examples at matched coverage, separately for in-domain and shifted conditions. |

For comparability, B and C must use the same model, feature condition, test cases, and target coverage. Policy rules and any combination weights must be chosen using development data only; final test labels cannot be used to tune them. The exact score combination is open for mentor approval. A stability-only arm may be a secondary ablation if feasible, but it does not replace A/B/C. Deferral means no automatic phishing/legitimate decision and referral to a stated review path; the human review itself is outside the main classifier evaluation unless separately approved.

## 8. Proposed methodology

This is a staged proposal, not an implementation specification approved for execution.

### Stage 1 — Confirm scope and audit candidate data

1. Confirm with the mentor whether the capstone must remain strictly URL-only. Phase 1 focuses on URL lexical/structural information; PhiUSIIL includes fields that may derive from webpage content, host/security metadata or other sources. Exclude non-URL fields from a URL-only arm or label feature regimes separately.
2. Evaluate candidate resources: **PhiUSIIL** (Phase 2 preferred starting point but requires field/leakage audit), **UCI Phishing Websites Dataset**, **LegitPhish**, and time-stamped snapshots from **PhishTank** or **URLhaus** with independent benign controls. No dataset is selected yet. Verify current access, license/terms, label provenance, collection times, field availability and intended-use constraints.
3. Record the exact source/version, hashes, acquisition date, label definition, class balance, exclusions, duplicate counts and feature computation. Do not rely on published sample counts as the project’s counts.
4. Audit exact-URL duplicates, normalized duplicates, registrable-domain overlap, campaign/group overlap and source-identifying fields. Define a group key before splitting.
5. Avoid constructing an external test set from phishing examples in one source and benign examples in a different source without addressing source shortcuts. Seek independent controls or use a clearly qualified external protocol and source-confounding analysis.

### Stage 2 — Freeze the evaluation design

1. Separate model fitting, model/hyperparameter selection, calibration fitting, policy selection and final evaluation. One practical design is training/development, calibration/policy-validation, and locked test partitions, with group/time constraints preserved across all partitions.
2. Create an in-domain held-out test using domain/campaign grouping to limit leakage. Define the shifted condition separately: preferably a later timestamped test period if reliable dates exist; otherwise an independently sourced, domain-disjoint dataset with its limitations clearly stated. If feasible, report both temporal and external shift; feasibility is unresolved.
3. Ensure the final test data are not used to select features, calibrators, thresholds, perturbation operators, stability metric, coverage target or C combination rule.
4. Establish a data-flow diagram and split manifest so all variants from one source URL remain in the same evaluation partition.

### Stage 3 — Select a feasible model and establish calibrated probabilities

1. Phase 1 names Logistic Regression, Random Forest and XGBoost as initial model candidates. These are not locked choices. Seek mentor approval for a feasible small model set and a primary model. A simpler model may be preferable if it supports interpretable feature contributions and capstone resource limits.
2. Train only after the data and design are approved. Use identical eligible input fields for A, B and C. Compare no more models than needed to answer the question.
3. Fit candidate calibration methods (for example, sigmoid/Platt, isotonic or temperature scaling where appropriate) using held-out calibration data or properly nested/cross-fitted development predictions. Choose method by a pre-specified validation protocol, not final test performance.
4. Report the calibration method, fitting data, binning/estimation choices, and calibration diagnostics. Do not call a model “calibrated” solely because it reports probabilities or has a low Brier score on a test set.

### Stage 4 — Define and validate perturbations and explanations

1. Develop a perturbation catalogue that aims to preserve URL syntax **and** the relevant URL intent/class. Candidate families suggested by prior work include controlled subdomain/path/character/padding variants, but no family is automatically semantically valid. Exclude transformations that change the destination or phishing intent unless they are analyzed separately as adversarial attacks.
2. Specify the validation procedure before scoring stability. Possible validation could combine deterministic equivalence rules with expert/manual review on a sample; the mentor must approve a feasible and defensible protocol. Report rejected/ambiguous variants. URL syntax validity alone does not prove semantic validity.
3. Compute local attribution explanations for each original and its accepted perturbations using a selected explainer compatible with the model (TreeSHAP for supported tree models is a candidate; an appropriate local method is needed for other models). Freeze the background/reference data and explainability settings using development data.
4. Define a per-instance stability score from similarity of attribution vectors for the original and its validated variants. Candidate metrics from project records include top-k Jaccard, rank correlation, cosine similarity, signed agreement or normalized attribution drift. Select one primary metric and direction before final testing; analyze alternatives as sensitivity checks.
5. Keep concepts separate: explanation stability measures attribution consistency; explanation faithfulness asks whether an explanation reflects model behavior; calibration measures probability reliability; prediction robustness measures label/prediction behavior; perturbation validity concerns whether meaning/intent is preserved. One is not a proxy for the others.

### Stage 5 — Fit policies and run A/B/C evaluation

1. Freeze model probabilities and stability scores on development/calibration sets. Define the confidence ranking for B and the confidence-plus-stability ranking/combination for C. Document how ties and class asymmetry are handled.
2. Select any policy parameters, combination rule and target coverage points using policy-validation data only. Consider matching C and B by a common coverage grid, with the primary coverage point or summary endpoint identified in advance.
3. Evaluate A at full coverage; evaluate B/C over the same locked in-domain and shifted test sets. For each condition compare B and C at matched coverage. Preserve results by class, source, and shift where sample size permits.
4. Include a stability-only ablation only if it is feasible and approved. It is secondary and is not the primary research question.
5. Record environment, random seeds, model/configuration, data manifest, policy rules, and analysis code so the work can be reproduced. This stage is future work and has not begun.

## 9. Evaluation metrics and statistical analysis

### Predictive performance (primarily A and full model behavior)

- Confusion matrix; phishing precision and recall; legitimate specificity; false-positive and false-negative rates; F1; PR-AUC; ROC-AUC; accuracy as a secondary summary.
- Report class prevalence and thresholds. Emphasize false negatives and false positives separately because their operational consequences differ.

### Calibration

- Brier score and log loss, reliability diagrams, and ECE or adaptive ECE with the binning protocol stated. Include calibration diagnostics by domain/shift and, where feasible, class or score range.
- Assess calibration on data not used to fit the calibrator. Evaluate whether calibration changes under shift.

### Selective prediction

- Coverage (accepted fraction) and defer rate; selective risk among accepted cases; risk-coverage curves; AURC using a stated convention; risk at pre-specified matched coverage points; false-negative rate among automatically accepted phishing predictions.
- Define the primary risk function before running the test. At minimum report error risk; if using asymmetric operational costs, publish the cost matrix and sensitivity analysis. Do not switch risk definitions after seeing results.

### Explanation stability and perturbation validity

- Primary local stability metric to be selected before final evaluation; likely attribution-vector similarity (such as top-k Jaccard or cosine) but no metric is approved yet.
- Report distributions of stability scores and how they associate with errors conditional on calibrated confidence. Report perturbation families, number of accepted variants per URL, validity review and failures.
- Report prediction-change rates separately from attribution change so stability is not confused with prediction invariance.

### Uncertainty and comparisons

- Report effect sizes and confidence intervals, not only p-values.
- Use paired comparisons for B versus C on the same URLs and at matched coverage. Bootstrap by registrable domain/campaign or other justified group unit to account for within-group dependence; do not assume each URL or generated variant is independent.
- Report paired bootstrap confidence intervals for B–C risk differences at the primary coverage and across a pre-specified coverage curve/grid. If multiple random seeds or split replications are feasible, report between-split variation.
- Any inferential tests, multiplicity treatment across several coverage points/models/shift sets, and minimum sample/precision targets require a pre-analysis plan. No significance result is currently available.

## 10. Unresolved decisions for mentor/research-lead approval

1. Is the project strictly URL-only, or may a separate URL+webpage/content condition be included?
2. Which primary dataset/version is feasible and acceptable, and what independent or temporal shifted evaluation is available?
3. What benign controls, label validation and time-stamped snapshots can be obtained without source confounding?
4. Which model(s) are feasible and which one is primary: Phase 1 candidates are Logistic Regression, Random Forest and XGBoost; no final set is approved.
5. What exactly qualifies as semantically valid perturbation, who validates it, and which perturbation families can be defended?
6. Which explainer and primary stability metric should be used? What sampling budget for variants is feasible?
7. Which calibration method and data partition are appropriate? How will policy selection be separated from calibration and final testing?
8. What score-combination rule defines C, and what target coverage point(s) are operationally meaningful?
9. What is the primary selective-risk definition and false-negative/false-positive cost assumption?
10. Which bootstrap/grouping unit, confidence level and multiplicity approach should be pre-specified?
11. What are mentor requirements, rubric, deadline, paper/prototype deliverables, and expected scope? No formal requirements or feedback were recovered in project records.
12. Is the proposed study feasible within capstone time and compute limits? If not, agree on a narrower design without changing the research question until explicitly approved.

## 11. Limitations and threats to validity

- **Search completeness:** Current review is scoping and not systematic; the exact novelty gap is search-dependent. The Patel full paper is inaccessible, and other full texts or supplements remain unverified.
- **Dataset bias and leakage:** Benchmarks may include duplicates, shared domains/campaigns, labels tied to source, or fields unavailable in a real URL-only setting. Random splits may overstate generalization.
- **Shift confounding:** External datasets can differ in time, source, prevalence and collection process at once. A source shift is not automatically a clean temporal or campaign shift.
- **Semantic validity:** Some URL edits alter destinations or intent. Incorrectly treating those as label-preserving can invalidate the explanation-stability score.
- **Explanation limitations:** Stable attributions may consistently emphasize a spurious feature; stability is not faithfulness, causality, or truth. Feature correlations and explainer choices affect attribution.
- **Calibration drift:** A calibrator fitted in-domain may be unreliable after shift. The study must report this, not assume calibration transfers.
- **Policy tuning bias:** Selecting thresholds, coverage, combinations, or metrics on the final test set would inflate apparent benefit.
- **Scope and resource limits:** Multiple datasets, models, shift protocols, explainer methods and perturbation families can exceed capstone resources. A small, pre-approved design is preferable to an underpowered broad one.
- **Operational generalization:** A benchmark selective-risk improvement would not alone demonstrate user benefit, deployment safety, or reduced real-world phishing harm. Human review outcomes and live operational deployment are outside the current approved scope.

## 12. Why this study is worth conducting

Phishing detectors are often evaluated by how well they classify a fixed test set, while a deployed system must decide when an automatic answer is reliable enough to act on. Confidence-only deferral is a meaningful baseline, but it may miss cases where the prediction score appears strong while the model’s local rationale changes sharply under a valid URL variation. A controlled matched-coverage test can show whether explanation stability adds practical triage information, whether any gain persists under distribution shift, and where the signal fails. Even a null result would be useful: it would discourage adding explanation complexity when calibrated confidence already selects cases as well or better.

The study is worth pursuing if the mentor agrees that data provenance, semantic perturbation validation, and a fair matched-coverage comparison are feasible. Its value depends on careful evaluation and a bounded claim, not on presuming that the combined method will improve performance.

## 13. Project status and next steps

**Completed:** Phase 1 and Phase 2 research documents exist; a prior literature review and matrix were inspected; the project’s research context and handover were prepared; an October–November 2026 literature addendum and decision memo were created; the Patel IJAICET access audit records abstract-level evidence and failed full-text access attempts. The corrected Ghalechyan DOI and Buu & Cho venue are documented in the addendum.

**Not completed:** No dataset has been selected or audited for this capstone; no model has been trained; no calibration, perturbation, explanation, A/B/C, or shifted-data experiment has been run; no prototype exists in the inspected project folder; no mentor rubric, deadline or approval of the remaining methods choices is recorded.

**Recommended immediate actions:**

1. Bring this plan and the exact research question to the university mentor; resolve the scope and decisions in Section 10.
2. Seek an accessible copy of the Patel full paper from the publisher or authors and update its audit only when methods/results text is available.
3. Complete a logged, reproducible literature search and screening before making a final novelty claim.
4. After mentor approval, audit candidate dataset fields/provenance and finalize the data split, shift definition, perturbations, calibration, stability metric, policy, risk function and statistical plan.
5. Only after those decisions are approved, implement and run the A/B/C experiments. Training and implementation are not authorized by this proposed document.

## Source documents consolidated

This plan consolidates `phase1.docx`, `Capstone_Phase_2_Literature_Review.docx`, `phishing_xai_literature_review.docx`, `phishing_xai_literature_matrix.xlsx`, `RESEARCH_CONTEXT.md`, `HANDOVER.md`, `Literature_Update_Oct07-Nov18_2026.md`, `Updated_Decision_Memo_2026-11-18.md`, and `PATEL_IJAICET_FULL_PAPER_AUDIT.md`. The source documents were not edited in preparing this new plan.
