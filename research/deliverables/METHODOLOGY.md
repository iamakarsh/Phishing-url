# Proposed Methodology

**Project title:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability  
**Document status:** Proposed methodology for mentor review; no capstone data analysis, model training, or experiments have been completed.

## 1. Research design

This capstone is a controlled empirical study of selective prediction for phishing URL classification. It will test whether the stability of local feature-attribution explanations under a narrow set of meaning-preserving URL representation changes provides useful information for deciding which predictions to issue automatically. The principal comparison is between a calibrated-confidence-only policy and a policy that uses calibrated confidence plus local explanation stability. A conventional full-coverage classifier will provide a reference condition.

The design is comparative and non-interventional with respect to real users: it evaluates predictions on held-out data and does not claim a live deployment, user study, or causal effect. The model, calibration method, explanation settings, feature extractor, perturbation rules, policy-development process, and analysis plan will be frozen before the locked test is evaluated.

### Research question (retained verbatim)

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

### A/B/C experimental framework

- **A — Conventional full-coverage classification:** the classifier outputs phishing or legitimate for every eligible URL. No case is deferred.
- **B — Calibrated-confidence-only selective prediction:** the classifier accepts predictions based on calibrated confidence in its predicted class and defers the remainder.
- **C — Calibrated confidence plus local explanation stability:** a selection score uses the same calibrated confidence as B together with a local explanation-stability score; lower estimated reliability can lead to deferral even when confidence alone would accept the case.

The primary test is **B versus C at matched coverage on the same held-out examples**, reported separately for the in-domain and shifted evaluation cohorts. A is a full-coverage reference and is not a matched-coverage selective policy.

## 2. Dataset selection and justification

### 2.1 Proposed primary dataset: PhiUSIIL, conditional on audit

The proposed primary in-domain dataset is **PhiUSIIL Phishing URL (Website), UCI ID 967**. The UCI record lists 235,795 instances, a raw `URL` field, a binary label, 54 features, no missing values in its tabular summary, and a CC BY 4.0 license. It reports 100,945 phishing and 134,850 legitimate URLs. The UCI record also states that instances represent URLs and their corresponding webpages and that features were derived from URL and webpage source code. These are repository-reported facts, not values verified by a capstone file audit.

PhiUSIIL is selected provisionally because the raw URL field allows a single project-owned URL feature extractor to be applied consistently to training, calibration, test data and generated variants. The dataset is not URL-only in its original feature representation. Therefore, the primary experiment will use only the raw URL and label from the source and recompute documented URL-string features. It will exclude HTML/page features, host-reputation and security lookup fields, unrestricted domain identity, `FILENAME`, and derived/reference statistics that are not reproducible from the URL alone.

**Selection remains conditional.** Before training, inspect the actual source file, schema, label coding, missing/malformed URLs, exact duplicates, conflicting labels, registrable-domain overlap, source identifiers and feature provenance. UCI’s description that many URLs were “latest” at construction does not establish per-row timestamps. Unless timestamps are found and validated in source material, PhiUSIIL will not be described as temporally splittable.

### 2.2 Shifted evaluation cohort

The proposed shifted evaluation is a **prospective, dated PhishTank verified-online phishing cohort**, captured after a declared development-data cutoff and archived with retrieval time and source metadata. PhishTank’s official developer record documents URL, submission time, verification status/time and online status. The feed supplies phishing candidates, not a balanced labeled phishing/benign test set.

Pair these phishing URLs with a contemporaneous, independently curated benign control panel. Tranco may provide a reproducible sampling frame and archived list identifier; ranking or inclusion in Tranco alone is not proof that a URL is benign. A separate verification protocol and evidence log are required. Use multiple benign sources if feasible, remove overlap with development data, and inspect whether source identity is predictive. Do not visit potentially malicious URLs during collection or verification.

If the control-panel protocol is infeasible, use **LegitPhish v2** only as a fallback cross-dataset cohort after URL/domain overlap and provenance checks. Its Mendeley record reports raw URLs and labels, but its source overlap is unestablished, its listed class counts sum to one fewer than its reported total, and its source description includes URLHaus. URLhaus’s official scope is malware distribution and says phishing is out of scope. These issues require review before using LegitPhish; it must not be called an independent or temporal evaluation set without evidence.

The older **UCI Phishing Websites** dataset is not selected as primary because its public record lists 30 engineered integer features but no raw URL field. **URLhaus** is not a phishing-positive dataset and will not be used to label phishing cases.

### 2.3 Dataset facts versus pending inspection

The figures and metadata above are reported by UCI/Mendeley/provider records. They are not verified counts in a downloaded capstone dataset. No candidate data were downloaded or inspected for this methodology document. Final dataset selection, licenses/terms, label quality, feature mapping, row counts, overlap and class distributions remain pending manual verification and mentor approval.

## 3. URL-only feature extraction

Build one deterministic parser/extractor that accepts a raw URL and returns a documented vector of lexical and structural features available from the string itself. Candidate examples include total URL length, host/path/query lengths, dot/hyphen/digit/special-character counts, subdomain count, query-parameter count, IP-literal indicator, scheme/HTTPS indicator and selected token/encoding properties. The final feature list is not yet approved and must be frozen before model evaluation.

The feature extractor must not fetch the target page, perform DNS/WHOIS/reputation lookups, use HTML or page content, include source/filename/domain identity as a predictive feature, or use statistics estimated using held-out data. Normalize and parse consistently. Preserve the original URL in the data manifest for audit while applying a pinned, documented feature transformation. Handle malformed URLs through an explicit reject/exclusion rule rather than silently converting them to normal records.

The same code and settings must be used for original URLs, perturbation variants, all partitions and the shifted cohort. This avoids using features during training that cannot be computed at the intended URL-only decision point.

## 4. Data cleaning and label checks

Before splitting:

1. Store source/version, retrieval date, file hash, license and label-source metadata.
2. Confirm label coding from the actual downloaded file; map to a single internal convention and record the mapping.
3. Identify missing, malformed and unsupported URL strings. Report each exclusion and its class/source distribution.
4. Normalize URLs for duplicate detection without erasing meaningful path/query distinctions. Identify exact duplicate strings, normalized equivalents and conflicting labels. Do not silently choose a label for conflicts; quarantine and review them under a documented rule.
5. Parse registrable domains using a pinned Public Suffix List version. Inspect domain repetition and cross-label cases.
6. Record class balance, source distribution, date coverage (if available), URL feature availability, and duplicate/domain counts before and after exclusions.
7. For PhishTank positives, retain provider IDs and verification/collection timestamps. For benign controls, keep the evidence and verification date for each label. Tranco is a sampling list, not a benign label source.

No row counts, duplicate rates, final class balance or label quality results are available yet; they must be calculated from the selected source files.

## 5. Leakage prevention and partitions

Use a grouped, locked evaluation design. At minimum, all records sharing a registrable domain and all generated variants of an original URL must remain in the same partition. If reliable campaign or source-group identifiers exist, assess whether they require grouping as well. Exact URL deduplication alone does not prevent domain or campaign leakage.

Create separate development subsets for:

1. **Training/model development:** fit candidate classifier parameters and select a constrained XGBoost configuration through grouped cross-validation or development-only validation.
2. **Calibration:** fit the post-hoc probability calibration mapping using predictions from a model that did not train on these calibration examples, with group separation maintained.
3. **Policy development:** choose the B and C selection rules/parameters using held-out or cross-fitted development predictions; do not use locked-test labels.
4. **Locked in-domain test:** assess once after the pipeline, thresholds, coverage points, metric definitions and perturbation protocol are frozen.
5. **Shifted test:** evaluate on the dated PhishTank/control cohort, with exact URL/domain overlap removed and the shifted collection process fully documented.

Partition proportions will be chosen only after manual inspection of the number of distinct groups and class counts. No fixed split percentages are claimed. Group-disjoint development partitions and an external/prospective test are distinct from a temporal split. Domain grouping alone does not establish temporal shift.

## 6. Models and configuration

### 6.1 Primary model: XGBoost

Use XGBoost as the primary classifier on the project-owned URL-only feature vector. It can represent nonlinear feature relationships and interactions and supports TreeSHAP explanations. Constrain tree depth, number of boosting rounds and other complexity-related settings; choose them only with development data. Record software versions, parameters, class weighting if used, random seeds, hardware, training time and preprocessing.

The exact model configuration and whether the available computer can train it in acceptable time are unknown. Profile computational feasibility only after mentor approval and after the dataset audit; no training is part of this document.

### 6.2 Optional reference model: Logistic Regression

If scope permits, fit Logistic Regression on the same feature set as a simpler, low-compute reference. Its coefficients provide a transparent benchmark, and a compatible linear SHAP method can be used for a secondary explanation analysis. The primary A/B/C test remains on XGBoost unless the mentor approves running both models through the full policy comparison.

### 6.3 Random Forest

Random Forest is excluded from the minimum design. It is a valid nonlinear baseline and supports tree explanations, but adds another tree model, tuning/calibration work and result tables without being necessary to test the policy signal. Reintroduce it only if the mentor requests all Phase 1 candidates or the dataset audit gives a specific justification.

## 7. Probability calibration

Use sigmoid/Platt scaling as the proposed primary post-hoc calibration method. Fit the mapping using a group-disjoint calibration partition, separate from classifier training and final test. Keep the classifier frozen while fitting the calibrator. Evaluate calibration on held-out in-domain and shifted data; in-domain calibration is not assumed to transfer under shift.

If enough independent calibration groups remain, isotonic calibration may be a development-only sensitivity comparison. Choose the primary calibrator with a predeclared validation procedure, not locked-test results. Document the fit partition, method, label coding, predicted score, reliability-bin procedure and software version. A probability output or Brier score alone does not establish that calibration was performed.

For a binary classifier, define B’s confidence as the calibrated probability assigned to the classifier’s predicted class. Calibration metrics will include Brier score, log loss, reliability diagrams and one pre-specified ECE procedure. Report overall and by evaluation condition where feasible.

## 8. Local SHAP explanations and stability

For the primary XGBoost model, use SHAP `TreeExplainer`. Freeze the explainer version, output being explained, class orientation, feature dependence setting and background convention before test use. Proposed primary setting: explain the same base XGBoost model output for original/variant pairs using `feature_perturbation="tree_path_dependent"`; report whether raw margin/log-odds or probability space is explained. The post-hoc calibration mapping is a separate component and is not automatically explained by TreeSHAP. Do not claim that SHAP is causal or inherently faithful.

### Primary stability metric

For each original URL `x_i`, produce `m_i` accepted semantically validated variants. For the specified model output, let `T_5(x_i)` be the five features with the largest absolute SHAP values. Let `T_5(x'_ij)` be the corresponding set for variant `j`. The primary per-instance stability score is the mean top-5 Jaccard similarity:

\[
S_i = \frac{1}{m_i}\sum_{j=1}^{m_i}
\frac{|T_5(x_i)\cap T_5(x'_{ij})|}{|T_5(x_i)\cup T_5(x'_{ij})|}.
\]

The score ranges from 0 to 1; larger values indicate greater overlap in the top five attributed features. This score ignores attribution magnitudes and sign changes. Cosine similarity or signed-rank agreement may be reported as secondary sensitivity analyses only. If the final feature set is too small for top five to be informative, the value of `k=5` must be reconsidered before analysis and approved.

## 9. URL perturbations and semantic validation

The primary protocol will use a narrow set of representation-preserving rewrites, not arbitrary adversarial edits. Candidate operations include:

- case changes in scheme/host, which are case-insensitive in URI syntax;
- percent-encoding or decoding of unreserved characters only, where normalized resource identifiers remain equivalent;
- explicit default-port normalization only where the scheme/port equivalence is unambiguous.

Do not treat subdomain insertion, homoglyph replacement, TLD swaps, query-value changes or arbitrary path edits as meaning-preserving; they can change destination or content. Such changes would be separate adversarial robustness tests and are not part of this primary method.

For every generated variant, apply formal parser/canonicalization checks for the declared equivalence rule, log the original and transformed component differences, reject ambiguous or non-equivalent cases, and manually review a sample of pairs (preferably independently by two reviewers). Do not fetch or open live phishing URLs to validate content. A syntactically valid URL is not necessarily semantically equivalent. If allowed transformations do not change the extracted feature vector, record this limitation and consult the mentor rather than widening the perturbation set without approval.

## 10. A/B/C policies and matched-coverage analysis

### A — Full coverage

Apply the frozen classifier to every eligible test URL. Record phishing/legitimate predictions and probabilities. This is the conventional reference; there is no abstention.

### B — Confidence-only selective policy

Rank cases by calibrated confidence in the predicted class. The highest-confidence cases are accepted automatically; lower-confidence cases are deferred. Select a threshold or coverage rule on policy-development data and freeze it before final evaluation.

### C — Confidence plus explanation stability

Use calibrated confidence and the per-instance stability score. Proposed operational form: a low-capacity logistic correctness selector trained on cross-fitted development predictions, with calibrated confidence and stability as inputs and base-model correctness as the target. Generate the selector’s training scores out-of-fold so it does not learn from in-sample predictions. Freeze it before final testing. This form is a proposal and requires mentor approval; if scope is too high, choose and predeclare a simpler fixed combination rather than tuning on the test set.

### Matched-coverage comparison

On the same test cases, at a common pre-specified coverage grid, rank B and C scores and accept the same number of cases for each policy. Calculate risk among accepted cases. This evaluates ranking quality at matched coverage; using test score ranks for a common coverage is not a fixed-threshold deployment simulation and must be described as such.

In a separate operational-transfer analysis, select thresholds on policy-validation data, freeze them, and apply them unchanged to the shifted test. Report achieved coverage, risk and deferral rate even if coverage differs between domains. Never choose thresholds or model parameters from test labels.

## 11. Metrics and statistical analysis

### Predictive and calibration outcomes

- A: confusion matrix, phishing precision/recall, benign specificity, false-positive rate, false-negative rate, F1, PR-AUC, ROC-AUC and accuracy as secondary context.
- Calibration: Brier score, log loss, reliability diagram and a specified ECE/binning protocol. Report before/after calibration and by in-domain/shift condition where sample sizes permit.

### Selective outcomes

- **Primary risk:** 0–1 error rate among accepted cases at the mentor-approved primary coverage, with risk-coverage curves.
- **Primary policy effect:** paired (R_C-R_B) risk difference at matched coverage, separately in-domain and shifted; negative values favor C.
- Secondary: phishing false-negative rate and benign false-positive rate among accepted cases, class-specific coverage, deferral rate, and AURC using a stated convention.
- Do not invent a cost ratio. If the mentor approves asymmetric costs, state the cost matrix and report sensitivity to it alongside 0–1 risk.

### Stability, validity and uncertainty

- Report the distribution of primary stability scores, number of accepted variants per URL, rejected variant rates, perturbation type and prediction-change rate.
- Prediction changes, calibration error and explanation stability are separate measurements; do not conflate them.
- Use paired domain/campaign-cluster bootstrap confidence intervals for B–C risk differences. Resample groups, retain all records/variants in a sampled group, and evaluate both policies on the same resampled cases.
- Report effect sizes and intervals; tests/p-values are secondary. If multiple coverage points or models are examined, predeclare a primary point and handling of multiplicity.
- Do not treat URLs or their perturbation variants as independent when they share a domain/campaign.

## 12. Reproducibility and research integrity

Archive source versions/hashes, data retrieval times, labels, feature lineage, URL normalization and perturbation rules, group/split manifests, model and calibrator settings, SHAP configuration, policy rules, random seeds, package versions, hardware notes and analysis scripts. Maintain a distinction between repository-reported dataset metadata, published-paper findings and capstone measurements. Mark unknowns rather than filling gaps. No capstone result may be reported before it has been computed from the approved protocol. Preserve all source documents; changes to research decisions require mentor/research-lead approval.

## 13. Pending verification and approvals

**Manual checks before implementation:** download and inspect PhiUSIIL; verify raw URL/label fields and counts; audit class coding, missing/malformed records, duplicate URLs, conflicting labels, registrable-domain overlap, timestamps, source cues and field provenance; verify LegitPhish v2 count discrepancy/provenance if used; design and document the prospective PhishTank benign-control labels; determine whether approved perturbations alter model inputs.

**Mentor approval still needed:** strict URL-only scope; primary and shifted data sources; benign-control verification; model set and whether A/B/C runs on Logistic Regression; final XGBoost configuration/tuning budget; calibration method; SHAP output/background settings; (k) and the stability score; permitted URL equivalence operations/reviewer protocol; C selector form; coverage point(s); primary selective-risk definition; bootstrap/multiplicity plan; deadlines, compute limits and institutional data/ethics requirements.

This document specifies a proposed procedure only. No models were trained and no experiments were conducted.
