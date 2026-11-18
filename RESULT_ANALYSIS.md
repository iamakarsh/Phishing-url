# Result Analysis Plan

**Project title:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability  
**Status:** **Pending experiments.** No dataset has been downloaded/audited for this capstone, no models have been trained, and no empirical results are available. All result cells below are intentionally blank or marked pending.

## 1. Purpose and analysis discipline

This document defines how results will be recorded and interpreted after the mentor approves the methodology and the experiments are run. It is not a report of findings. Do not replace “Pending experiments” with estimates, published-paper numbers, or assumed outcomes. Results from prior literature belong only in the literature review and must be labeled as literature-reported.

The primary decision analysis is the comparison of **B — calibrated-confidence-only** with **C — calibrated confidence plus explanation stability** at matched coverage on the same examples. **A — conventional full-coverage classification** is a reference, not a selective policy.

### Research question (retained verbatim)

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

## 2. Dataset composition and feature audit

### Planned table

| Dataset/cohort | Source/version/date | Initial rows | Retained rows | Phishing | Legitimate | Missing/malformed | Exact duplicates | Domain groups | URL-only fields retained | Status |
|---|---|---:|---:|---:|---:|---:|---:|---:|---|---|
| Primary in-domain: PhiUSIIL (candidate) | Pending manual inspection | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Raw URL + project-recomputed features + label only (proposed) | Not downloaded/audited |
| Shift cohort: PhishTank + benign controls (proposed) | Snapshot date and list/source identifiers pending | Pending collection | Pending collection | Pending collection | Pending collection | Pending collection | Pending collection | Pending collection | Raw URL + project-recomputed features + label | Not collected; protocol needs approval |
| Fallback cross-dataset: LegitPhish v2 (conditional) | Mendeley v2; actual file/hash pending | Pending inspection | Pending inspection | Pending inspection | Pending inspection | Pending inspection | Pending inspection | Pending inspection | Recompute from raw URL if approved | Not downloaded/audited |

### Planned analyses

- Verify actual file columns, target coding, license/terms, class balance and source metadata against repository records.
- Report URL, registrable-domain and (if available) campaign overlap across partitions. Flag conflicting labels and provide disposition counts.
- Map every feature to its origin: raw URL parse, external lookup, webpage/HTML, source identifier, domain identity, reputation/reference statistics or derived value. Exclude non-URL inputs from the primary URL-only condition.
- Report whether source identity or label can be inferred from a field or collection artifact. Do not treat Tranco membership alone as a verified benign label.
- Treat any group/time availability as pending until checked in the actual source files. A domain-group split is not a temporal split.

## 3. Full-coverage classification results (A)

### Planned result table

| Evaluation condition | Model | N | Accuracy | Phishing precision | Phishing recall | Benign specificity | FPR | FNR | F1 | PR-AUC | ROC-AUC | Status/notes |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| In-domain locked test | XGBoost (primary) | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | No model trained |
| Shifted cohort | XGBoost (primary) | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Cohort not collected |
| In-domain locked test | Logistic Regression (optional) | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Optional model not approved |
| Shifted cohort | Logistic Regression (optional) | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Optional model not approved |

Report A as a full-coverage reference. Accuracy is secondary; discuss phishing false negatives and benign false positives separately, along with class prevalence and thresholds.

**Planned graph:** confusion matrices for each evaluation condition, with a consistent label order and counts. Optional ROC and precision-recall curves may supplement the table but do not substitute for selective-risk analysis.

## 4. Calibration analysis

### Planned table

| Condition | Model | Calibration status | Brier score before | Brier score after | Log loss before | Log loss after | ECE protocol/value | Calibration sample N | Notes |
|---|---|---|---:|---:|---:|---:|---|---:|---|
| In-domain | XGBoost | Pending experiments | Pending | Pending | Pending | Pending | Pending | Pending | Calibrator not fitted |
| Shifted | XGBoost | Pending experiments | Pending | Pending | Pending | Pending | Pending | Pending | Transfer under shift untested |

**Planned graphs:** reliability diagrams before and after sigmoid/Platt calibration on in-domain test data, plus shifted data if class counts support meaningful estimates. Include a histogram of predicted confidence. State binning, sample size and uncertainty. If isotonic is examined, label it a development-selected sensitivity comparison and disclose the calibration data used.

The proposed primary calibrator is sigmoid/Platt scaling fit on a separate calibration partition. It has not been fitted. Isotonic calibration is an optional development-only sensitivity check, not a second primary result.

Do not claim a model is calibrated solely because it outputs probabilities or has a particular Brier score. Do not use final test labels to select a calibrator.

## 5. Perturbation validity and explanation stability

### Planned validity/stability table

| Perturbation family | Generated pairs | Rejected as non-equivalent | Manual review agreement | Feature vector changed | Prediction changed | Median top-5 Jaccard | IQR/range | Status |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| Scheme/host case normalization (candidate) | Pending experiments | Pending experiments | Pending review | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Not generated |
| Unreserved-character percent-encoding form (candidate) | Pending experiments | Pending experiments | Pending review | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Not generated |
| Unambiguous default-port form (candidate) | Pending experiments | Pending experiments | Pending review | Pending experiments | Pending experiments | Pending experiments | Pending experiments | Not generated |

These are candidates only. The final set needs mentor approval and standards/parser-based validation. If the transformation leaves the URL-only feature vector unchanged, report that separately; a resulting stability score of 1 may be a consequence of invariant preprocessing, not evidence of meaningful explanation reliability.

### Planned stability analysis

For each original URL, compute the mean Jaccard overlap between the top five features by absolute SHAP value for the original and each accepted variant. Report the distribution by class, prediction correctness, confidence range, perturbation family and evaluation condition where sample size permits. Report cosine similarity or signed-rank agreement only as secondary sensitivity analyses if approved.

**Planned graphs:**

1. Histograms/violin plots of per-instance stability, split by correct versus incorrect predictions and by in-domain versus shifted data.
2. Scatter/hexbin plot of calibrated confidence versus stability, colored by prediction correctness; this shows whether stability may contain information not captured by confidence.
3. Prediction-change rate by perturbation family, separate from attribution-stability scores.
4. Optional plot of stability score against error rate across pre-defined bins, with confidence intervals. Bins must be selected without exploiting final test outcomes.

No stability score or perturbation result is currently available.

## 6. Selective prediction results: A/B/C

### Planned risk-coverage table

Use the same locked test cases and frozen model for B and C. At the approved common coverage grid, accept the same number/proportion under each ranking and compute risk among accepted cases. Test-set score ranking is an evaluation of ranking quality at matched coverage, not a fixed-threshold deployment simulation. Separately apply thresholds frozen on policy-validation data and report the coverage achieved after transfer.

| Condition | Coverage | A full-coverage error | B error among accepted | C error among accepted | C − B risk difference | 95% paired cluster-bootstrap CI | B accepted N | C accepted N | Status |
|---|---:|---:|---:|---:|---:|---|---:|---:|---|
| In-domain | Mentor-approved point(s) pending | Pending | Pending | Pending | Pending | Pending | Pending | Pending | No experiments |
| Shifted | Same target point(s), if estimable | Pending | Pending | Pending | Pending | Pending | Pending | Pending | Shift cohort not collected |

### Planned class-specific risk table

| Condition | Policy | Coverage | Accepted-set phishing FNR (FN / accepted actual phishing) | Accepted-set benign FPR (FP / accepted actual benign) | Class-specific coverage | Deferred rate |
|---|---|---:|---:|---:|---|---:|
| In-domain | B | Pending | Pending | Pending | Pending | Pending |
| In-domain | C | Pending | Pending | Pending | Pending | Pending |
| Shifted | B | Pending | Pending | Pending | Pending | Pending |
| Shifted | C | Pending | Pending | Pending | Pending | Pending |

### Planned graphs and statistical analysis

- **Risk-coverage curves:** selective 0–1 error against coverage for B and C, plotted separately for each evaluation condition; show uncertainty bands if the cluster bootstrap supports them.
- **Paired risk-difference plot:** `R_C - R_B` and 95% paired domain/campaign-cluster bootstrap intervals at the primary coverage and approved grid. Negative values favor C. Keep both policies on identical test examples and resample the same domain/campaign groups for each.
- **Shift-transfer plot/table:** validation-selected fixed thresholds applied unchanged to shifted data; show achieved coverage, risk, phishing FNR, benign FPR and deferral rate. Do not retune using shifted test labels.
- **AURC:** secondary summary if the integration convention is specified in advance.

Use effect sizes and intervals as the main inferential evidence. A p-value, if added, is secondary. URLs and perturbation variants within a domain/campaign are not independent units. Predeclare the primary coverage point and any multiple-comparison approach.

## 7. Error analysis and failure cases

For accepted and deferred examples, categorize false negatives and false positives using a codebook established before final analysis. Candidate categories include URL parser/normalization issue, domain-repetition/source cue, unusual subdomain, encoded or obfuscated string, high-confidence error, low-stability but correct prediction, stable but incorrect explanation signal, perturbation invalidity, and label ambiguity. These categories are proposed; each case must be grounded in actual evidence and not inferred solely from SHAP.

Review whether C defers useful correct cases, whether it accepts high-confidence errors, whether class-specific coverage changes, and whether the shifted cohort’s source composition affects outcomes. Do not treat a stable attribution as proof the classifier relied on a valid security cue.

## 8. Interpretation rules for possible outcomes

- **C has lower risk than B at matched coverage in-domain and shifted, with intervals supporting a practically meaningful difference:** evidence would support incremental value for this model, perturbation protocol, dataset and shift cohort. It would not prove general improvement for all phishing classifiers or deployment contexts.
- **C improves in-domain but not shifted:** possible dependence on the development distribution; the proposed claim must be limited and shift robustness cannot be claimed.
- **C lowers overall error but raises phishing FNR, or vice versa:** report the trade-off; do not summarize as an unqualified improvement. Mentor-approved cost priorities would be needed for operational interpretation.
- **C and B are similar or confidence intervals include no meaningful difference:** the study does not support added value from the stability signal under the tested protocol. A null result is informative and does not justify claiming risk reduction.
- **C performs worse:** report that the extra signal may add noise, distort ranking, or be unstable under the tested perturbations. Do not hide the result by selecting a favorable coverage point after analysis.
- **No sufficiently valid perturbations or insufficient feature variation:** the explanation-stability construct was not adequately measured; do not claim the research question was answered.
- **Poor calibration or calibration drift:** discuss the baseline limitation separately. A/B/C comparisons cannot be interpreted as a clean confidence-only versus stability test if B’s confidence policy is not properly calibrated under the evaluated condition.
- **Shift cohort is source-confounded or benign labels are weak:** qualify or withhold the external-shift conclusion; report class-conditional results and data limitations.

## 9. Results pending

All empirical results are **Pending experiments**, including:

- actual dataset composition, URL-only feature audit, duplicate/domain counts and final split sizes;
- XGBoost/optional Logistic Regression model fitting and full-coverage metrics;
- fitted calibration method and Brier/log-loss/ECE/reliability results;
- perturbation counts, validity review, feature-vector changes and prediction-change rates;
- SHAP values and per-instance stability score distributions;
- B/C coverage, selective risk, class-specific error rates and risk-coverage curves;
- in-domain versus shifted comparisons;
- paired bootstrap confidence intervals, effect sizes and any statistical tests;
- error categories and failure-case interpretation.

No values may be filled until generated by the approved experiment and traceable to the saved data, code and analysis outputs.
