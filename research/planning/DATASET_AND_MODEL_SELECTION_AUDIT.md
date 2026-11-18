# Dataset and Model Selection Audit

**Project:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability  
**Prepared:** 18 November 2026  
**Status:** Feasibility recommendations for mentor review. No dataset was downloaded or analyzed for this audit; no model was trained; no experiments were run.  
**Research question:** unchanged. This audit does not revise the question or A/B/C comparison.

## Executive recommendation

1. **Primary in-domain candidate: PhiUSIIL (UCI ID 967), conditionally.** It is the strongest of the named candidates for a URL-only study because its public record includes a raw `URL` field, binary labels, a substantial sample, and CC BY 4.0 terms. For the URL-only experiment, use only raw URL and label from the source file and compute the project’s own URL-string features. Do not use its precomputed page/content/security or dataset-derived fields in the URL-only condition. This recommendation is conditional on a manual file/label/domain-leakage audit.
2. **Shift evaluation: prospectively collect a dated PhishTank positive cohort, paired with a separately curated benign control panel.** PhishTank provides verified, online phishing URLs and submission/verification timestamps, but no benign class. A Tranco list can provide a reproducible sampling frame for candidate popular domains; it is not by itself ground truth that a URL is benign. Benign labels need a documented independent verification protocol. If that protocol is infeasible, use **LegitPhish v2** as a secondary cross-dataset check after deduplication and provenance screening, and describe it as cross-dataset rather than independent or temporal.
3. **URLhaus is not a phishing-positive dataset.** Its official policy says phishing and phishing kits are out of scope; URLhaus tracks URLs used for malware distribution. Do not use it as a phishing-label source or call its records phishing without a separate, defensible relabeling study.
4. **Core model set: XGBoost as the primary model and Logistic Regression as a simple reference.** Run the primary A/B/C comparison on XGBoost. Logistic Regression can be a compact secondary baseline; if mentor scope requires minimal work, retain only XGBoost. Do not add Random Forest unless the mentor requests a third model or a reason emerges from the data audit.
5. **Proposed primary procedures:** sigmoid/Platt probability calibration using a separate group-disjoint calibration partition; TreeSHAP for the primary XGBoost model with fixed settings and background/reference convention; top-5 absolute-SHAP feature-set Jaccard as the primary per-instance stability score; standards-based, representation-preserving URL rewrites with explicit validation; group-disjoint train/calibration/policy/test partitions; 0–1 selective error at matched coverage as the primary risk endpoint, with phishing false-negative rate and benign false-positive rate reported separately; paired domain/campaign-cluster bootstrap intervals for B–C differences.

Each item above is a **recommendation**, not a completed or approved experiment. Dataset, perturbation and evaluation choices remain subject to mentor approval.

## Evidence and confidence labels

- **Verified primary record:** publisher/repository documentation was accessible and supports the stated record-level fact. It does not mean the row-level file was downloaded or independently audited.
- **Verified paper/abstract claim:** a publication record or paper reports the claim. It is not a result from this capstone and may not have been independently reproduced.
- **Recommendation/inference:** reasoned from the verified facts and research question; needs approval and, where marked, empirical data inspection.
- **Unknown/not checked:** not stated in accessible metadata or not verified in the local project. Do not fill in by assumption.

The source documents already in the workspace were consulted, including Phase 1 and Phase 2, [RESEARCH_CONTEXT.md](RESEARCH_CONTEXT.md), [HANDOVER.md](HANDOVER.md), the literature update, decision memo and Patel access audit. Their earlier dataset descriptions were treated as candidate information, not as proof that data are suitable. The details below are checked against repository/provider records where available.

## 1. Dataset assessment

### Comparative summary

| Candidate | Verified availability and format | URL and label information | Provenance, time and class balance | Fit for the primary URL-only task | Main risks / required checks |
|---|---|---|---|---|---|
| **PhiUSIIL Phishing URL (Website), UCI ID 967** | Public UCI record; downloadable CSV; UCI lists CC BY 4.0. It reports 235,795 instances, 54 features, and no missing values in its tabular record. [UCI record](https://archive.ics.uci.edu/dataset/967/phiusil+phishing+url+dataset) | UCI variable list includes raw `URL`, `Domain`, many URL features, and the target. Label 0 = phishing; 1 = legitimate. UCI says the instances represent URLs and their corresponding webpages and says features were extracted from URL and webpage source code. It names derived variables such as `URLTitleMatchScore`, `URLCharProb`, `TLDLegitimateProb`, and `CharContinuationRate`. | UCI reports 100,945 phishing and 134,850 legitimate URLs (about 42.8%/57.2% respectively), and says most analyzed URLs were “latest” at construction. **No row-level submission/verification/collection timestamps are listed in the public variable table.** The precise row-level label acquisition and adjudication protocol has not been established from the UCI metadata inspected. | **Best primary candidate, conditional.** It provides raw URL strings from which a consistent lexical/structural representation can be recomputed, while the scale is ample for grouped partitions, calibration and a held-out test in principle. UCI data are directly available with clear reuse attribution terms. | Not URL-only out of the box. Exclude `Domain` as an unrestricted predictor, page/HTML-derived fields, and opaque/reference-derived variables from the URL-only arm; use only URL string and label as inputs and recompute a documented feature extractor. Audit duplicate URLs, conflicting labels, registrable domains, campaign proxies, and source cues. Confirm the downloaded file’s exact columns/labels/duplicates and the actual file license/citation. UCI’s page-level “latest” language does not create a temporal dataset. |
| **UCI Phishing Websites, ID 327** | Public UCI dataset; UCI lists 11,055 instances, 30 integer features, no missing values, and CC BY 4.0. Donated 25 March 2015. [UCI record](https://archive.ics.uci.edu/dataset/327/phishing+websites) | The record exposes engineered indicator features such as `url_length`, `having_ip_address`, `shortining_service`, `having_sub_domain`, `sslfinal_state` and favicon. It is a tabular feature set; **no raw URL field is listed**. | UCI says source material came mainly from PhishTank archive, MillerSmiles archive and Google search operators. The current record does not provide row-level capture timestamps or source-to-row mapping. The class counts are not included in the retrieved UCI page extract and are therefore not asserted here. | **Not suitable as primary.** It can serve as an old, feature-only historical benchmark, but cannot support re-parsing raw strings, controlled URL perturbations, or a local explanation-stability score derived from URL variants. | Old collection/donation record, no raw URLs, unclear row-level provenance/time. Its 30 website indicators may encode properties not available from a URL-only input. Do not describe it as a direct raw-URL dataset. |
| **LegitPhish v2, Mendeley Data DOI 10.17632/hx4m73v2sf.2** | Public Mendeley record, version 2 published 22 May 2025; it lists CC BY 4.0 and a “Download All” control. [Mendeley record](https://data.mendeley.com/datasets/hx4m73v2sf/2) | Record describes raw full URL, label, and 17 engineered URL features (18 fields including label). The listed features include URL length, IP address, dot count, HTTPS, entropy, token/subdomain/query counts, TLD length, and related indicators. Labels: 0 phishing, 1 legitimate. | The record states 63,678 phishing and 37,540 legitimate URLs, which sum to **101,218**, while the stated total is **101,219**. This one-record discrepancy needs resolution in the downloaded version. The dataset paper says phishing URLs came from URLHaus/PhishTank and other verified sources and legitimate URLs from Google Search API/Wikipedia/Stack Overflow; it describes manual verification. **No row-level timestamps are shown in the accessible record/schema.** [Data article](https://doi.org/10.1016/j.dib.2025.111972) · [PMC full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC12538017/) | **Useful secondary cross-dataset candidate; not preferred primary.** It has raw URLs and a more explicitly described URL-feature representation, and can offer a ready-made second dataset. | The same source families may appear in other phishing corpora, so independence is unproven. Benign examples are curated from prominent sites, which may create class/source shortcuts. The data article’s use of URLHaus for phishing needs particular scrutiny: official URLhaus policy says phishing sites/kits should not be submitted because URLhaus is for malware-distribution URLs. That does not prove every LegitPhish positive is mislabeled, but it makes the source/label mapping a required audit. Resolve count mismatch, duplicates, domain overlap with training and feature extraction recipe before describing it as an independent test. |
| **PhishTank current feed/snapshot** | Official developer documentation describes downloadable JSON/CSV/XML formats and an hourly-updated feed. The regular feed requires an application key for ongoing access; unauthenticated access is limited. [Developer documentation](https://phishtank.org/developer_info.php) · [Terms](https://phishtank.org/terms.php) | Feed has phishing URL and metadata including `phish_id`, `url`, `submission_time`, `verified`, `verification_time`, `online`, and optional `target`. The published “online-valid” feed contains records that are marked verified and online. These are positive phishing candidates, **not a balanced labeled classification dataset**. | Useful timestamps exist at feed-entry level, supporting a new prospective snapshot. The source is community-submitted/community-verified; its terms disclaim a warranty of data accuracy. Current positive count/class balance was not downloaded or measured. | **Recommended source for a prospective shifted positive cohort**, not a standalone test dataset. It is timestamped and can be collected after the model-development data cutoff, subject to access and terms. | Must preserve snapshot bytes, retrieval time, feed version, IDs, and exclusions. Prevent overlap with training by exact URL and registrable domain. Active-only status introduces survivorship/availability bias. Need a contemporaneous benign sample with a separate label protocol; source/popularity shortcuts are a major threat. Do not visit unsafe URLs during data collection. |
| **URLhaus feed/snapshot** | Official community API offers full/recent/active malware URL dumps; current download access requires an abuse.ch Auth-Key and fair-use terms. Dumps are generated frequently. [URLhaus API](https://urlhaus.abuse.ch/api/) · [About](https://urlhaus.abuse.ch/about/) | Official API describes URL, `dateadded`, status, threat and associated tags in exports. The threat is malware distribution. Its submission policy explicitly says phishing sites and phishing kits should not be submitted: “Phishing is not Malware.” | Provides useful timestamped malware-feed records, but **not phishing ground truth** under its stated scope. No current record count was downloaded. | **Not recommended as a phishing positive class or the primary shifted phishing evaluation.** It may be useful only for a separate malware-vs-phishing task or as a negative-control source after label definition/validation, neither of which is the current research question. | Do not adopt LegitPhish’s or any secondary source’s “URLHaus phishing” wording without row-level provenance. Feed is time-varying and authentication/access constraints apply. Malware URLs are not benign controls. |

### Primary in-domain dataset recommendation: PhiUSIIL, with a strict URL-only extraction

**Recommendation:** Begin with PhiUSIIL UCI ID 967 as the primary in-domain candidate, subject to a manual audit before it is locked. The reasons are:

- The primary UCI record provides a raw `URL` column and binary label, unlike the older UCI Phishing Websites table.
- Its reported sample size and both class counts are large enough in principle for domain-grouped training, a held-out calibration subset, a policy-development subset and locked evaluation. This is a feasibility inference, not a claim that a final usable sample has been confirmed.
- A single project-owned URL parser/feature extractor can be applied to train, calibration, test, perturbations and any future URL. This reduces the train/serving mismatch that comes from using non-reproducible precomputed fields.
- UCI reports CC BY 4.0. Proper attribution and preservation of the source citation are still required.

**Important qualification:** PhiUSIIL is explicitly described as URLs **and corresponding webpages**, with features drawn from URL and webpage source. It is not a clean URL-only feature matrix. A URL-only analysis must start from the raw URL field and label, then recompute only documented URL-string lexical/structural features. Do not include the precomputed `URLTitleMatchScore`, HTML/page indicators, host-reputation/security metadata, raw domain as an unrestricted categorical identifier, or reference-derived/dataset-wide statistics in that condition. Run a column/feature provenance audit before modeling.

**Manual checks required before selection:**

1. Download the exact UCI version, archive its hash and retrieval date, and verify target coding and source-file schema.
2. Inspect missing, malformed, duplicate and conflicting-label URLs; calculate exact URL and registrable-domain overlap without publishing unsafe destinations.
3. Map each candidate feature to a source: raw URL parse, DNS/host lookup, HTML/page scrape, reputation/reference data, filename/source ID, or derived/global statistic. Retain only permitted URL-string fields in the primary condition.
4. Confirm whether usable timestamps or collection periods are available in original files or supplementary material. UCI’s public variables page does not document row-level timestamps; until proven otherwise, treat PhiUSIIL as **not temporally splittable**.
5. Check likely source/label shortcuts and domain repetitions by label. A grouped split controls cross-partition domain leakage but does not correct source-label confounding or label errors.

If this audit reveals that raw URL/label fields are unavailable, provenance is inadequate, or labels are too inconsistent, revisit LegitPhish as a primary source only after its version, labeling and URLHaus issue are resolved. Do not silently switch datasets.

### Shifted evaluation recommendation

**Preferred plan for mentor approval:** define a prospective, dated evaluation cohort from a saved PhishTank verified-online snapshot collected after a pre-declared development-data cutoff, and pair it with a contemporaneous benign control panel drawn from one or more independently documented sources. Tranco is a reproducible ranking/sampling frame with permanent list IDs and archived daily lists; it is a popularity list, not a benign-label authority. [Tranco project](https://tranco-list.eu/) · [Methodology](https://tranco-list.eu/methodology)

The benign panel requires explicit ground-truth rules (for example, multiple trusted allowlist/ownership sources and a non-phishing verification procedure at a fixed date). A URL’s presence in Tranco, or absence from PhishTank, is **not** by itself proof of legitimacy. The study must report how candidate controls were validated, whether they were live at collection, and the limits of that label. If verification is infeasible, do not call such data “confirmed benign”; use a clearly labeled proxy-control condition or choose another approved source.

This is more realistic for time shift than using a static dataset with no timestamps, but it has risks: PhishTank only contributes positive cases, active-only filtering can bias toward still-live campaigns, and phishing-versus-popular-benign source differences can make source identity predictive. Mitigate by using more than one benign source if feasible, matching URL/domain characteristics where possible, auditing source cues, removing overlaps with training, and reporting class-conditional performance. Do not claim a clean independent-domain shift unless the collection protocol supports it.

**Secondary fallback:** use LegitPhish v2 as a cross-dataset test only after exact URL/domain overlap removal and feature recomputation from raw URLs. Because it shares claimed source feeds with other datasets and its benign data are selected from high-authority sources, it is not automatically independent. Report it as a cross-dataset cohort with documented limitations, not a pristine temporal test.

## 2. Model feasibility and recommendation

### Model comparison

| Model | Strengths for this project | Limitations and SHAP fit | Recommendation |
|---|---|---|---|
| **Logistic Regression** | Low computational overhead; simple, reproducible baseline for a compact lexical/structural feature table; coefficients and odds direction provide a transparent reference. scikit-learn supports dense and sparse inputs. Logistic Regression often has relatively good probability behavior because of its log-loss link, but calibration must still be measured. [scikit-learn model docs](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) · [calibration guide](https://scikit-learn.org/stable/modules/calibration.html) | May miss nonlinear feature interactions. SHAP is available through a linear explainer, but this is not TreeSHAP; if it is only a contextual baseline, it need not receive the full perturbation/deferral experiment. | **Keep as a lightweight comparator if resources permit.** It can show whether the selective-policy result is tied to the primary nonlinear tree model. It is not the primary model for the SHAP-stability experiment. |
| **Random Forest** | Nonlinear, handles interactions without feature scaling, familiar and feasible on tabular URL features. scikit-learn describes averaging trees as controlling overfit and supports parallel tree fitting. TreeExplainer supports most scikit-learn tree ensembles. [Random Forest docs](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) · [TreeExplainer docs](https://shap.readthedocs.io/en/latest/generated/shap.TreeExplainer.html) | Ensembles can be larger than a linear baseline; default trees can grow very large and need constraints. scikit-learn notes characteristic probability calibration behavior for forests; post-hoc calibration is still relevant. It overlaps substantially with XGBoost for this capstone’s purpose, adding another tuning/calibration/SHAP evaluation branch. | **Do not include in the minimum set.** Add only if the mentor requires the Phase 1 three-model comparison or if a documented reason favors bagging over boosting. |
| **XGBoost** | Strong fit for structured URL features and nonlinear interactions; regularization controls tree complexity. It has model-native/TreeSHAP-compatible explanations; XGBoost documentation describes exact TreeSHAP contributions on CPU and GPU in current releases. [XGBoost model tutorial](https://xgboost.readthedocs.io/en/stable/tutorials/model.html) · [prediction/SHAP docs](https://xgboost.readthedocs.io/en/stable/prediction.html) | More settings to tune than Logistic Regression; careless tuning can overfit or consume project time. Tree ensembles are not intrinsically transparent, so local SHAP is explanatory analysis rather than proof of causal reasoning. CPU time on the actual laptop is unknown and must be profiled after approval. | **Recommended primary model.** Constrain depth/rounds and tune only on grouped development data. It supports the proposed feature-attribution stability score directly through TreeExplainer. |

### Minimal proposed model set

- **Primary model:** one regularized, depth-constrained XGBoost classifier using the URL-only project feature extractor.
- **Optional secondary baseline:** Logistic Regression on the same inputs. Report its full-coverage results and, if mentor scope allows, repeat B/C for it using a compatible linear SHAP explainer as a secondary analysis.
- **Exclude Random Forest from the minimum plan.** It is a reasonable algorithm, but running it in addition to XGBoost adds a second tree ensemble without being necessary to answer the incremental-value question. The research question is about the policy signal, not the winner among classifiers.

For the clearest feasible test, A/B/C should all use the same frozen primary XGBoost model. If only one classifier is approved, state that the inference is about the selected XGBoost setting and do not generalize across all phishing classifiers. Do not add a model because published papers report high accuracy; no capstone benchmark result exists yet.

## 3. Recommended methodological choices

### Probability calibration

**Primary recommendation:** fit a sigmoid/Platt calibrator on a group-disjoint calibration subset using predictions from the frozen primary classifier. The calibration data must be separate from model fitting and final test data. scikit-learn documents sigmoid/Platt calibration as a parametric mapping and warns that calibration samples and classifier-training samples should be disjoint; it also describes isotonic calibration as more flexible but more prone to overfit on small calibration samples. [Calibration guide](https://scikit-learn.org/stable/modules/calibration.html) · [CalibratedClassifierCV documentation](https://scikit-learn.org/stable/modules/generated/sklearn.calibration.CalibratedClassifierCV.html)

**Reasoning:** sigmoid calibration is a modest-capacity, monotone method, a practical choice for a capstone and a clean baseline. It preserves score ordering (apart from ties/numerics), which helps separate calibration from the selective-ranking question. This is a methodological recommendation, not evidence that sigmoid will calibrate this dataset best.

**Sensitivity check:** if the calibration partition is sufficiently large after grouping, compare isotonic calibration using development data only. Select the primary method by a pre-declared validation criterion (for example log loss/Brier plus reliability diagnostics), never by locked test performance. If applying a calibrator to a frozen model via `FrozenEstimator`, keep its training/calibration data disjoint as the scikit-learn documentation requires. Reassess calibration on shifted data; in-domain calibration does not guarantee shifted calibration.

### Primary local explanation method

**Primary recommendation:** SHAP `TreeExplainer` for the XGBoost primary model. It is model-specific TreeSHAP and is designed for tree ensembles; the SHAP documentation describes fast/exact tree attribution methods and makes explicit that results depend on feature-dependence assumptions, background data and output space. [TreeExplainer documentation](https://shap.readthedocs.io/en/latest/generated/shap.TreeExplainer.html)

Before analysis, freeze and report: (a) XGBoost prediction output being explained (raw margin/log-odds or probability), (b) positive-class orientation, (c) background/reference convention, (d) `feature_perturbation` setting, and (e) SHAP version. Recommended simple consistent setting for the primary stability calculation: same frozen model and output space for original/variant pairs, `feature_perturbation="tree_path_dependent"`, using the same training-fitted trees and path counts. If the mentor prefers interventional TreeSHAP, use one fixed, documented training-derived background sample and never rebuild it for each test point. Do not describe TreeSHAP as causal explanation or proof of faithfulness.

The calibrated probability is used by policy B/C. SHAP should explain the underlying XGBoost prediction; it does not automatically explain the separate post-hoc calibration mapping. State this explicitly. If explaining calibrated probability is required, verify the explainer can represent that composite function before adopting it.

### Primary explanation-stability metric

**Primary recommendation:** per-URL mean top-5 Jaccard similarity of absolute SHAP attribution sets.

For each original URL (x_i), create (m_i) accepted, semantically validated variants. Let (T_5(x_i)) be the five features with the largest absolute SHAP values for the primary model’s specified output, and (T_5(x_{ij}')) the equivalent set for variant (j). Define:

\[
S_i = \frac{1}{m_i}\sum_{j=1}^{m_i}
\frac{|T_5(x_i)\cap T_5(x_{ij}')|}{|T_5(x_i)\cup T_5(x_{ij}')|}.
\]

The score lies from 0 to 1, with larger values indicating more overlap in the top five attributed features. This is understandable, bounded, and consistent with the top-K Jaccard style used by Ahamed et al. in the adjacent URL-perturbation literature. It ignores attribution magnitudes and sign changes, so report cosine similarity or signed-rank agreement as a **secondary sensitivity analysis**, not as another primary endpoint. If the feature set is too small for top five to be meaningful, revise (k) before analysis and record mentor approval. [Ahamed et al.](https://www.frontiersin.org/journals/computer-science/articles/10.3389/fcomp.2026.1834407/full)

This score is a measure of attribution-set consistency only. A stable explanation can consistently rely on a spurious feature. Measure predictive calibration, prediction changes and explanation faithfulness separately if within scope.

### URL perturbation and validation strategy

**Primary recommendation:** restrict the primary stability protocol to standards-grounded, meaning-preserving URL representations and validate them deterministically before generating features. Candidate operations include:

- case normalization of scheme and host (URI scheme and host are case-insensitive);
- percent-encoding/decoding of **unreserved** characters only, where the parser’s normalized resource identifier remains equivalent;
- normalization of an explicit default port only when the scheme/port pairing is unambiguous.

Use the same URL parser and normalization rules for original and variant strings, log each transformation, and compare canonical parsed components (scheme, host, port, path, query and fragment) according to the approved equivalence rule. Avoid transformations of reserved characters, host labels, registrable domain, TLD, query values, or meaningful path segments. Do not use subdomain injection, homoglyph substitution, TLD swaps, or arbitrary path/query edits as *semantics-preserving* perturbations; they can change destination or meaning. Those are adversarial robustness operators and would require a separate question and label-validity protocol.

**Validation:** first use formal parser/canonicalization rules to reject non-equivalent transformations; then conduct a small blinded manual review of generated pairs and edge cases by two reviewers familiar with URL semantics, recording disagreements. No live phishing URL should be opened or fetched to validate its payload. If the candidate transformations produce no meaningful change to the model input after feature extraction, document that and reassess feasibility with the mentor rather than silently widening to meaning-changing attacks.

These operations test representation invariance under a narrow class of equivalent URL forms; they may not approximate attacker behavior broadly. The term “semantically valid” should be defined operationally in the protocol and not inferred just from syntactic validity.

### Data splitting and leakage prevention

**Proposed protocol:**

1. Normalize and deduplicate exact URLs; flag conflicting labels instead of silently resolving them. Keep all raw and normalized forms of the same URL together.
2. Parse registrable domains using a pinned Public Suffix List version. Use registrable domain as the minimum grouping unit; if campaign/brand/source identifiers exist, assess whether a stronger group is justified. Keep all URLs and generated variants from each group in exactly one partition.
3. Make group-disjoint development partitions for training/model selection, calibration, policy selection, and a locked in-domain test. Determine sizes only after inspecting group counts/class distributions; no split percentage is asserted as final here. Use grouped cross-validation inside training data for tuning where feasible.
4. Lock an external/temporal shifted set independently and remove exact URL/domain overlaps with development data. Keep final test labels inaccessible to threshold, feature, perturbation, calibration, SHAP-metric and model selection.
5. Record the source version/hash, snapshot retrieval time, label evidence, feature provenance, group assignment and split seed/manifest. Do not split random rows before domain deduplication.
6. Report the source and class distributions within each partition. Check whether a classifier can infer dataset/source from features; use multiple benign sources or sensitivity analyses to expose source-label shortcuts.

If no timestamp exists for PhiUSIIL rows, an in-domain group split is **not** a temporal split. The separate PhishTank cohort is prospective only relative to its documented collection cutoff and remains an external-source comparison with the caveats described above.

### Matched-coverage B/C procedure

- **B score:** calibrated confidence in the predicted class (for binary classification, the calibrated probability assigned to the model’s predicted class).
- **C score:** a simple correctness/acceptance score using B’s calibrated confidence plus the stability score. Recommended to fit a low-capacity two-input logistic selector on cross-fitted development predictions, with the target “base classifier correct/incorrect.” Generate out-of-fold confidence and stability scores so the selector never trains on in-sample predictions. Freeze the selector before final test. This is a proposal for mentor approval; a fixed non-learned combination is a lower-complexity alternative but must be declared in advance.
- At common pre-specified coverage points, accept the same number/proportion of test cases under B and C by ranking their scores; calculate risk on the accepted cases. This produces a fair **ranking-quality-at-matched-coverage** comparison without using test labels to select cases. It is not the same as a deployable fixed threshold.
- Separately choose operational thresholds on policy-validation data, freeze them, then apply them unchanged to in-domain and shifted test data. Report the achieved coverage and risk after transfer. This reveals whether the policy maintains its target behavior under shift.
- The primary coverage points and policy form must be set before test evaluation. If multiple models are studied, match B and C within each model, not across models.

### Primary selective risk and statistical evaluation

- **Primary risk:** 0–1 selective error rate among accepted cases, reported at one mentor-approved primary coverage and as a risk-coverage curve. This avoids inventing an unsupported false-positive/false-negative cost ratio.
- **Security-sensitive secondary risks:** phishing false-negative rate among accepted decisions and benign false-positive rate, shown separately at matched coverage. Also report deferred fraction and class-specific coverage.
- **Summary:** AURC can be secondary if its exact convention/integration is stated. Report precision/recall, F1, PR-AUC and confusion matrices for the full-coverage A reference; accuracy alone is insufficient.
- **Calibration:** Brier score, log loss, reliability diagram and one specified ECE procedure on held-out calibration/test data, overall and by evaluation domain where feasible. Report pre- and post-calibration values; a Brier score by itself is not proof that a calibrator was fit.
- **Primary effect:** paired risk difference (R_C-R_B) at matched coverage, separately for in-domain and shifted data; negative values favor C. Report the absolute estimate, 95% confidence interval and practical magnitude.
- **Uncertainty:** paired cluster bootstrap resampling at registrable-domain/campaign level so URLs and perturbation variants from one group remain together. Use the same sampled groups and test records for B/C within each replicate. Report between-split or seed variation if repeated grouped splits are feasible. Statistical tests are secondary to confidence intervals/effect sizes; predeclare any multiple-comparison handling.
- Do not bootstrap individual URL variants as if independent. A positive in-domain effect does not establish benefit under shift, and a higher AURC does not substitute for the pre-specified matched-coverage result.

## 4. Facts, assumptions and mentor decisions

### Verified from the accessible records

- UCI offers the PhiUSIIL dataset record with a raw URL field, 235,795 instances, 54 listed features, both class counts, URL-plus-webpage description, labels and CC BY 4.0 terms.
- UCI’s 2015 Phishing Websites dataset record lists 11,055 instances and 30 integer features and describes its source archives; no raw URL variable appears in its public variables listing.
- Mendeley’s LegitPhish v2 record lists raw URL, label, 17 engineered features, 101,219 total rows, the two class counts, CC BY 4.0 and publication date/version. The listed class counts sum to one less than the total.
- PhishTank official docs identify verified/online feed fields and timestamps; current feed documentation indicates hourly refresh and key-related access limits. Terms state the data are not warranted for accuracy.
- URLhaus official documentation describes a malware URL feed and expressly excludes phishing URLs from its submission scope.
- scikit-learn documents sigmoid/isotonic calibration and separation between fit/calibration samples. SHAP documents TreeExplainer’s tree-model support and dependence/background/output-space choices. XGBoost documents tree regularization and TreeSHAP prediction contributions.

### Assumptions/recommendations requiring confirmation

- PhiUSIIL’s raw URLs and labels will remain usable after cleaning and the domain-grouped test will retain adequate group/class counts.
- The mentor accepts a URL-string-only feature extractor even though the source dataset is named “Phishing URL (Website).”
- A prospective PhishTank snapshot can be collected and archived under current terms and the project can obtain/validate a benign control panel. No feed was downloaded and no current feed size was checked.
- A standards-based equivalence perturbation set will create enough feature variation to estimate per-instance stability without changing intended destinations/labels. This must be checked before committing to the protocol.
- A low-capacity C selector and group-disjoint calibration/policy partitions are feasible within the capstone timeline and sample size.
- XGBoost’s runtime will be acceptable on the available computer. No timing assessment was performed.

### Mentor approval requested before implementation

1. Approve or reject PhiUSIIL as the primary source **after** the file-level URL/label/domain/field audit.
2. Confirm whether the shifted set should be the proposed PhishTank-plus-verified-benign prospective cohort, LegitPhish v2 cross-dataset cohort, or both.
3. Approve the two-model maximum (XGBoost primary; optional Logistic Regression reference) and whether A/B/C is run for both or only the primary model.
4. Approve sigmoid/Platt as the pre-specified primary calibration approach and isotonic as a development-only sensitivity check.
5. Approve TreeExplainer settings/output space, top-5 absolute-SHAP Jaccard as the primary stability metric, and the proposed URL-equivalence perturbation set/validation process.
6. Approve the C selector form, primary coverage point(s), 0–1 selective risk endpoint and group-bootstrap plan.
7. Confirm schedule, compute limits, deliverable expectations and any institutional data handling/ethics requirements.

## 5. Decision summary

**Most suitable primary candidate among those named:** PhiUSIIL, after strict URL-only feature rebuilding and manual provenance/leakage audit. Its raw URL field, documented labels, scale, repository availability and license make it more adaptable to this question than UCI Phishing Websites, while LegitPhish presents source and count questions and lacks documented timestamps.

**Most credible feasible shifted-data direction:** a prospectively archived PhishTank verified-positive snapshot with a carefully verified, contemporaneous benign panel and explicit overlap/source-bias controls. It is a proposed cohort recipe, not an existing balanced benchmark. LegitPhish can be a cross-dataset fallback but should not be called independent before overlap analysis. URLhaus is excluded as a phishing source because its official scope is malware URLs.

**Smallest defensible model plan:** XGBoost primary with A/B/C and TreeSHAP stability; Logistic Regression as an optional small reference; omit Random Forest unless requested. The core project claim must remain about the selected model(s), not all classifiers.

No experiment, training run, dataset download, feature audit, or data-split calculation has been performed in preparing this audit. The source research documents have not been modified. This audit is a concrete proposal for the four mentor deliverables, not authorization to implement or a change to the approved research question/A/B/C comparison.
