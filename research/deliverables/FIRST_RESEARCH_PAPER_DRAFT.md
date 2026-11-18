# Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability

**First research paper draft for B.Tech CSE (AI) capstone discussion**  
**Status:** Proposed study; empirical results are pending. This draft does not report completed experiments.

## Abstract

Phishing URL classifiers commonly assign a phishing or legitimate label even when the evidence supporting an individual decision may be unreliable. Selective prediction can defer uncertain cases, but a confidence-only policy may not capture whether the model’s local explanation remains consistent under equivalent changes in URL representation. This proposed study asks whether a local explanation-stability score adds selective-risk value beyond a properly calibrated confidence-only policy when both operate at matched coverage on in-domain and shifted phishing URL data. The planned comparison includes a full-coverage classifier (A), calibrated-confidence-only selection (B), and calibrated confidence combined with local explanation stability (C). The proposed primary dataset is PhiUSIIL, conditional on a manual URL-only feature, label and leakage audit. A prospective dated PhishTank phishing cohort paired with independently verified benign controls is proposed for shifted evaluation; a balanced cohort is not yet available. XGBoost is the proposed primary classifier, with optional Logistic Regression as a reference. The proposed analysis uses sigmoid probability calibration, SHAP TreeExplainer, mean top-5 absolute-attribution Jaccard similarity across narrowly defined representation-preserving URL variants, matched-coverage 0–1 selective error, class-specific false-negative and false-positive rates, and paired domain/campaign-cluster bootstrap intervals. No capstone data have been downloaded or analyzed, no model has been trained, and no experimental results are available. The paper therefore presents a research plan and result-analysis framework; whether explanation stability improves selective risk remains an open empirical question.

**Keywords:** phishing URL detection; selective prediction; probability calibration; explainable AI; SHAP; explanation stability; distribution shift; risk coverage

## 1. Introduction

URLs are an important signal for phishing detection because a link may be screened before a user reaches the destination. Lexical and structural URL characteristics can support automated classification, but a conventional classifier still produces a label for every input, including cases where the model is uncertain or relies on brittle patterns. False negatives may allow phishing links through; false positives may incorrectly warn on or block legitimate resources. Accuracy alone does not describe whether individual predictions are appropriate for automatic action.

Selective prediction provides an alternative to forced full-coverage decisions. A system accepts some predictions and defers others for review. Calibrated confidence offers a principled baseline for deciding which cases to accept, but score calibration and ranking quality may change under distribution shift. Explanation methods such as SHAP can describe feature contributions for individual predictions. The consistency of those contributions under equivalent URL representations may provide additional information about prediction reliability, although stability is not the same as faithfulness or correctness.

The proposed project evaluates this idea as a decision-policy question. It compares full-coverage classification (A), calibrated-confidence-only selective prediction (B), and confidence combined with local explanation stability (C). The primary comparison is between B and C on the same held-out examples at matched coverage. The study will use a URL-only feature representation and will separately evaluate in-domain and shifted data, subject to data availability and validation.

### Research question

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

## 2. Problem Statement

Phishing URL classification is often evaluated as a binary prediction problem, but a deployed decision system must also decide when not to automate. A high-confidence prediction can still be wrong, and a confidence score calibrated on one data distribution may become less reliable after a shift in domains, collection sources or time. Post-hoc explanations may appear plausible while changing substantially under URL transformations that preserve the same intended destination. It is not established whether measuring local explanation stability improves case selection beyond calibrated confidence alone.

The problem addressed here is therefore not to propose a new phishing classifier or claim that explanations are inherently trustworthy. It is to evaluate whether an explicitly defined explanation-stability signal contributes incremental information for selective decisions under controlled URL-only and shifted evaluation.

## 3. Background

### 3.1 URL-only phishing detection

URL-only detection uses information that can be extracted from the address string, such as length, host/path structure, subdomain count, special-character patterns and query structure. Datasets called phishing URL datasets can also contain HTML/page, DNS, reputation or other security-derived features. A URL-only experiment must exclude fields that require page retrieval or external lookups and must use the same feature extractor in training and evaluation.

### 3.2 Calibration and selective prediction

Calibration concerns whether predicted probabilities correspond to observed frequencies. It is distinct from classification accuracy and from whether the model ranks correct predictions above incorrect ones. A selective policy uses a score to accept predictions at some coverage and defer the rest. Risk is measured among accepted cases. This study proposes a confidence-only policy as the baseline and a confidence-plus-stability policy as the treatment. Comparison at matched coverage isolates differences in ranking quality for accepted cases more fairly than comparing policies with different defer rates.

### 3.3 Explanation stability

For this study, local explanation stability is the similarity of feature-attribution patterns for an original URL and accepted variants judged to preserve its meaning. The proposed primary score is the mean Jaccard overlap of top-five features ranked by absolute SHAP value. This score measures consistency of the selected attribution features; it does not establish that the explanation is faithful, causal, security-relevant or correct. A stable explanation may consistently reflect a spurious feature.

## 4. Literature Review

Phishing URL detection has been studied using engineered URL features, webpage features, tree ensembles and neural representations. The use of SHAP/LIME, probability estimates, uncertainty, URL perturbation or external evaluation is not novel on its own.

Ghalechyan et al. (2024) study deterministic and probabilistic neural-network approaches for phishing URL detection. Their probabilistic approach uses weight uncertainty and an interval-based “unknown” outcome when the interval crosses a decision threshold. This is relevant precedent for uncertainty-based deferral, but the reviewed evidence does not establish an explanation-stability signal or a formal confidence-plus-stability comparison. The correct DOI is 10.1038/s41598-024-74725-6; an incorrect DOI appears in the earlier Phase 2 bibliography. [3]

Rashid et al. (2024) study cross-dataset phishing URL generalization and unsupervised domain adaptation. This motivates explicit shifted evaluation but does not address local explanation stability as a selective decision signal. [4]

Ahamed et al. (2026) directly evaluate URL-level explanation stability. Their published *Frontiers in Computer Science* study measures SHAP top-K Jaccard similarity under structured phishing URL perturbations, alongside robustness and generalization. The paper reports instability under some perturbation families. It establishes that explanation stability under URL perturbation is prior work; its reviewed protocol evaluates stability as a robustness/evaluation axis, not as the added signal in a matched-coverage confidence-only versus confidence-plus-stability comparison. [2]

Panthi et al. (2026) compare SHAP and LIME for boosting models in phishing URL detection. Their work supports the relevance of explanation analysis but does not establish the proposed selective decision comparison in the reviewed record. [5]

Khan’s OACSP-Forensics (2026) is an SSRN preprint whose abstract describes calibrated selective prediction, coverage-risk analysis and phishing URL evaluation under external shift. The accessible record is abstract-level; the full text was not verified in the project review. It is an important close study for confidence-based deferral and shift, but the abstract does not report local explanation stability. [6]

PhishLens (October 2026) uses a calibrated uncertainty band to route ambiguous phishing website cases to a local language model for evidence selection. The reviewed full paper describes the classifier label and probability as frozen, so this routing selects an explanation/evidence pathway rather than clearly withholding the classifier decision. It does not establish the proposed local attribution-stability signal or matched-coverage B/C comparison. [7]

The IJREK article dated 28 October 2026 proposes feature-selection stability across resamples or time and uncertainty-based abstention for phishing URL detection. The article describes these as proposed design choices whose benefits remain to be demonstrated. Its feature stability is global feature-selection consistency rather than per-instance local explanation stability. Peer-review status was not established in the project review. [8]

Patel et al. (2026) is a particularly close study. Its publisher abstract reports a PhiUSIIL evaluation involving calibration analysis, TreeSHAP explanations and constrained feature-space perturbations. However, the full paper could not be retrieved in the project audit. Exact features, calibration, perturbation validity and whether any selective-policy comparison is present remain unknown. Accordingly, it cannot yet be treated as verified evidence that the exact matched-coverage comparison has or has not been conducted. [9]

Buu and Cho (2025) publish “A Transformer network calibrated with fuzzy logic for phishing URL detection” in *Fuzzy Sets and Systems* 517, article 109474, DOI 10.1016/j.fss.2025.109474. This corrects a venue discrepancy in the earlier Phase 2 document. The exact calibration procedure and detailed findings require full-text verification; “calibrated” in the title is not by itself evidence of a statistical probability calibrator. [10]

Overall, existing work covers explanation stability, uncertainty/abstention, calibration, distribution shift, and combinations of explanation and robustness analysis. The current review is a targeted scoping review, not a fully reproducible systematic review; lack of a located direct match is not proof of absence.

## 5. Research Gap

The accessible literature establishes that URL phishing explanation stability under perturbations has been measured, and that confidence/uncertainty-based deferral has been studied. It also includes studies combining calibration, explanation analysis and perturbation robustness. What remains insufficiently established in the reviewed evidence is whether a **per-instance local explanation-stability score** computed under **semantically valid URL perturbations** adds incremental selective-risk value beyond a **properly calibrated confidence-only policy**, when compared at **matched coverage** on both in-domain and shifted phishing URL data.

This gap is provisional. The full Patel et al. paper remains inaccessible, and unreviewed work or a more exhaustive search may narrow or close it. The study will not claim absolute novelty or first use of SHAP, calibration, selective prediction, perturbation or explanation stability. The bounded contribution is the empirical decision comparison, if the final search and experiment design support it.

## 6. Research Objectives

1. Establish a leakage-aware URL-only phishing classification baseline.
2. Evaluate and calibrate XGBoost probabilities using data separate from the locked test.
3. Generate local SHAP explanations and define a reproducible per-instance explanation-stability score under validated URL variants.
4. Compare full-coverage classification, calibrated-confidence-only selection, and confidence-plus-stability selection.
5. Evaluate B and C at matched coverage on the same examples in an in-domain test and a documented shifted cohort.
6. Quantify uncertainty in B–C risk differences and report calibration, class-specific error, perturbation validity and failure cases.

## 7. Research Question and Hypotheses

### Research question

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

### Hypotheses

- **H1:** Lower local explanation stability is associated with higher prediction error after accounting for calibrated confidence, class, model and evaluation domain.
- **H2:** At matched coverage, a confidence-plus-stability policy has lower selective risk than a calibrated-confidence-only policy.
- **H3:** The incremental value of explanation stability is greater under temporal/domain shift or perturbation conditions that cause attribution drift.
- **H4:** Invalid or meaning-changing URL edits can produce misleading stability signals; perturbation validity must therefore be controlled and reported.

These are untested hypotheses. H1 is associational; H2 is the primary policy test. H3 may be underpowered if only one shifted cohort is feasible. H4 motivates validation rather than presuming all candidate transformations are invalid.

## 8. Proposed Methodology

### 8.1 Research design and experimental arms

The study is a controlled empirical evaluation of one frozen primary classifier and selective policies. A is full-coverage classification. B accepts cases ranked by calibrated confidence in the predicted class and defers the remainder. C uses the same confidence signal plus a local explanation-stability score to rank/accept cases. The primary comparison is B versus C on identical held-out examples at matched coverage. A is the non-selective reference.

### 8.2 Dataset and feature setup

PhiUSIIL UCI ID 967 is the proposed primary in-domain candidate, conditional on manual file, label and leakage audit. The URL-only condition will use raw URL and label, recomputing documented lexical/structural features with a single parser. Page/HTML, host-reputation, security lookup, source identifier and non-reproducible dataset-derived features will be excluded. Exact class counts, duplicate rates, final features, label checks and domain counts are pending actual file inspection.

For shift evaluation, the proposal is to collect and archive a dated PhishTank verified-online positive cohort after a declared development-data cutoff. PhishTank supplies no benign class. A separate contemporaneous benign panel must be validated with independent evidence. Tranco may support reproducible sampling, but membership is not a benign label. LegitPhish v2 is a fallback cross-dataset cohort after overlap and provenance inspection; it is not assumed independent. URLhaus will not be used as a phishing-positive source.

### 8.3 Cleaning, splitting and leakage control

Verify labels, malformed/missing URLs, duplicate URLs and conflicting labels. Parse registrable domains using a pinned Public Suffix List. Keep all URLs, variants and same-domain records in one partition; use campaign/source groups if reliable identifiers exist. Partition development data into training/model-selection, calibration, policy-development and locked in-domain test sets. Determine partition sizes after checking distinct groups and class counts; fixed percentages are not yet approved. Keep the shifted cohort separate, remove URL/domain overlap, and preserve its snapshot date and label evidence. Domain grouping alone is not temporal shift.

### 8.4 Classifier and calibration

XGBoost is the proposed primary model because it can represent nonlinear relationships in structured URL features and is supported by TreeSHAP. Constrain complexity and tune using grouped development data only. Logistic Regression is an optional low-compute reference; Random Forest is excluded from the minimum set unless the mentor requests it or data justify it.

Fit sigmoid/Platt scaling on a separate group-disjoint calibration set with the classifier frozen. Isotonic calibration may be a development-only sensitivity check if enough independent calibration groups remain. Do not select the calibrator using locked-test results. Evaluate calibration both in-domain and under shift.

### 8.5 Explanation and stability score

Use SHAP TreeExplainer for the primary XGBoost model. Freeze output space/class orientation, explainer version, feature-dependence setting and background convention. Proposed setting: explain the same base model output for original/variant pairs using tree-path-dependent explanations. This explains the base model; the post-hoc calibration mapping is separate and is not automatically explained by TreeSHAP.

For each original URL (x_i), generate (m_i) accepted variants. Let (T_5(x_i)) and (T_5(x'_{ij})) be the top five features ranked by absolute SHAP value for the original and variant. Define the primary stability score as:

\[
S_i = \frac{1}{m_i}\sum_{j=1}^{m_i}
\frac{|T_5(x_i)\cap T_5(x'_{ij})|}{|T_5(x_i)\cup T_5(x'_{ij})|}.
\]

The score ranges from 0 to 1; higher values indicate more overlap. It ignores magnitude/sign changes, so cosine or signed-rank agreement may be reported as secondary sensitivity analyses. The value of (k=5) must be reconsidered only before analysis if the final feature set makes it unsuitable.

### 8.6 Perturbation generation and validation

Use a narrow, standards-grounded set of representation-preserving rewrites: scheme/host case normalization; percent encoding/decoding of unreserved characters only where canonical resource identifiers remain equivalent; and unambiguous default-port normalization. Exclude subdomain injection, homoglyph replacement, TLD changes, arbitrary query edits and path changes from the primary semantic-equivalence protocol because they may change destination or content.

Validate variants with documented parser/canonicalization checks, log component changes, reject ambiguous/non-equivalent pairs, and have two reviewers inspect a sample if feasible. Do not open or fetch live malicious destinations. If allowed transformations do not change extracted features, document the limitation rather than broadening the perturbation set without approval. Syntactic validity alone is not proof of semantic equivalence.

### 8.7 Operational policies and matched coverage

- **A:** issue a phishing/legitimate label for every URL.
- **B:** rank by calibrated probability of the model’s predicted class; accept the highest-confidence cases and defer the remainder.
- **C:** proposed low-capacity logistic correctness selector trained on cross-fitted development predictions with calibrated confidence and stability as inputs and base-model correctness as target. Freeze it before testing. This policy form requires mentor approval; any simpler fixed combination must also be predeclared.

At common pre-specified coverage points, accept the same number of held-out cases under B and C by ranking policy scores and compute accepted-case risk. This matched-coverage ranking analysis is not a fixed-threshold deployment simulation. Also choose thresholds on policy-validation data and apply them unchanged to shifted test data, reporting achieved coverage and risk.

### 8.8 Metrics and statistics

For A, report confusion matrix, phishing precision/recall, benign specificity, false-positive/negative rates, F1, PR-AUC, ROC-AUC and accuracy as secondary context. For calibration, report Brier score, log loss, reliability diagrams and a stated ECE/binning protocol. For selection, use 0–1 error among accepted predictions as the proposed primary risk; report risk-coverage curves, risk at matched coverage, defer rate, phishing false-negative rate and benign false-positive rate separately. AURC may be secondary if its convention is stated.

The primary effect is `R_C - R_B` at matched coverage, separately in-domain and shifted. Use paired domain/campaign-cluster bootstrap intervals; keep records and variants from a group together and use the same sampled groups for both policies. Report effect sizes and confidence intervals. Statistical tests are secondary. Coverage points, risk definition, multiplicity handling and bootstrap details require approval before final test use.

## 9. Dataset and Experimental Setup

| Element | Proposed setup | Status |
|---|---|---|
| Primary data | PhiUSIIL (UCI 967), URL and label only | Conditional; file audit pending |
| URL features | Project-recomputed lexical/structural features | Feature list not frozen |
| Shift cohort | Dated PhishTank positives + verified benign panel | Not collected; benign protocol pending |
| Fallback | LegitPhish v2 cross-dataset test | Conditional; provenance/count/overlap checks pending |
| Primary model | XGBoost | Proposed; no model trained |
| Optional reference | Logistic Regression | Requires scope approval |
| Calibration | Sigmoid/Platt on separate calibration partition | Proposed; not fitted |
| Explanation | SHAP TreeExplainer, fixed output/background convention | Proposed; not run |
| Stability | Mean top-5 absolute-SHAP Jaccard over accepted variants | Proposed; not computed |
| Split | Group-disjoint train/calibration/policy/test plus shifted cohort | Split sizes pending actual data audit |

## 10. Result Analysis Framework

All empirical cells are **Pending experiments**. The result-analysis document will report dataset composition, feature provenance, full-coverage metrics, calibration, perturbation validity and prediction-change rates, stability distributions, risk-coverage curves, B–C matched-coverage differences, in-domain versus shift results, paired bootstrap intervals and error cases. It will include tables and planned graphs without fabricated values.

Possible outcomes include C lowering risk at matched coverage, showing no meaningful difference, performing worse, or helping in-domain but not under shift. Each outcome will be interpreted only for the tested model, feature representation, datasets and perturbation protocol. A null or negative result does not justify an improvement claim and remains scientifically informative.

## 11. Discussion

The proposed comparison is motivated by a gap between explanation stability as an evaluation statistic and explanation stability as a per-instance selection signal. Ahamed et al. (2026) already study SHAP stability under URL perturbation, while OACSP-Forensics and Ghalechyan et al. cover related uncertainty/selective decisions. Patel et al. is close on calibration, TreeSHAP and constrained perturbation, but the accessible evidence is abstract-level because full text could not be retrieved. Therefore, the contribution is narrowly framed as an incremental matched-coverage test; it is not an absolute novelty claim.

The study’s value depends on data quality and protocol validity. PhiUSIIL must be reduced to a reproducible URL-only feature condition; the prospective shift cohort requires defensible benign labels; perturbations must preserve relevant URL meaning; and calibration, policy development and final test must remain separate. No claim can be made yet about which policy is better.

## 12. Provisional Conclusion

This paper proposes an empirical evaluation of whether local explanation stability adds selective-risk value beyond calibrated confidence for phishing URL classifiers at matched coverage. The design uses a full-coverage reference and two selective policies, with in-domain and shifted evaluation. The study is worth conducting because it can establish whether an additional explanation-based signal improves case selection, has no measurable benefit, or behaves differently under shift.

**No empirical conclusion is currently possible.** No dataset has been downloaded or audited for this capstone, no model has been trained and no A/B/C experiment has been run. The claim that explanation stability improves selective risk remains a hypothesis, not a finding.

## 13. Limitations and Future Work

The literature review is scoping rather than exhaustive, and the Patel full text remains unavailable. PhiUSIIL includes webpage-associated fields and lacks documented row-level timestamps in the inspected repository schema. PhishTank contributes positive cases only; an independently verified benign panel is a separate requirement. LegitPhish v2 has a listed count mismatch and source overlap requires evaluation. Narrow standards-based perturbations may not generate enough feature variation; broader attacks may change URL destination or semantics. Explanation stability does not establish explanation faithfulness. Results from one model or one shift cohort will not generalize automatically to all classifiers or deployments.

Future work may evaluate additional time periods/datasets, alternate stability metrics, additional classifier families, calibration under shift, and human review outcomes. Any extension is subject to feasibility and mentor approval.

## 14. References

**Reference-status note:** Entries below use metadata verified in project source documents and the literature update. Where only a publisher abstract/preprint record was inspected, that status is stated. Incomplete bibliographic details are not guessed.

1. Prasad, A., & Chandra, S. (2024). “PhiUSIIL: A diverse security profile empowered phishing URL detection framework based on similarity index and incremental learning.” *Computers & Security*. DOI: [10.1016/j.cose.2023.103545](https://doi.org/10.1016/j.cose.2023.103545). UCI dataset record: [PhiUSIIL Phishing URL (Website)](https://archive.ics.uci.edu/dataset/967/phiusil+phishing+url+dataset).
2. Ahamed, T., Kakon, S. C., Farid, F. A., Uddin, J., & Abdul Karim, H. B. (2026). “An integrated evaluation protocol for adversarial robustness, generalization, and explanation stability in URL-based phishing detection.” *Frontiers in Computer Science*, 8, 1834407. DOI: [10.3389/fcomp.2026.1834407](https://doi.org/10.3389/fcomp.2026.1834407). Publisher full text reviewed.
3. Ghalechyan, H., Israyelyan, E., Arakelyan, A., Hovhannisyan, G., & Davtyan, A. (2024). “Phishing URL detection with neural networks: an empirical study.” *Scientific Reports*, 14, 25134. DOI: [10.1038/s41598-024-74725-6](https://doi.org/10.1038/s41598-024-74725-6). Publisher full text reviewed; use this corrected DOI.
4. Rashid, S., Doyle, J., Han, Y., & Seneviratne, A. (2024). “Phishing URL detection generalisation using Unsupervised Domain Adaptation.” *Computer Networks*, 245, 110398. DOI: [10.1016/j.comnet.2024.110398](https://doi.org/10.1016/j.comnet.2024.110398).
5. Panthi et al. (2026). “A performance analysis of SHAP and LIME for interpreting boosting models in phishing URL detection.” *Scientific Reports*. DOI: [10.1038/s41598-026-66800-x](https://doi.org/10.1038/s41598-026-66800-x). Full author list and volume/article number should be recovered from the publisher record before final submission.
6. Khan, M. S. (2026). “OACSP-Forensics: A Cross-modal Selective Prediction Benchmark for Court-admissible Digital Forensic Triage.” SSRN preprint, posted 1 August 2026. DOI: [10.2139/ssrn.7140758](https://doi.org/10.2139/ssrn.7140758). Abstract-level evidence in project review; full text not verified.
7. Nandhini, S., Saranya, M., & Jegathambal, P. M. G. (2026). “PhishLens: Calibration-Aware Selective Routing with Verified Local-LLM Evidence Selection for Phishing Website Detection.” *International Journal of Engineering and Management Research*, 16(5), 68–79. DOI: [10.31033/IJEMR/16.5.2026.1951](https://doi.org/10.31033/IJEMR/16.5.2026.1951). Publisher full paper reviewed.
8. IJREK article record (2026). “Adaptive phishing URL Detection using feature stability and uncertainty based Decision making Information technology.” DOI: [10.5281/zenodo.23641293](https://doi.org/10.5281/zenodo.23641293). Article record describes a proposal; full author metadata and peer-review status require verification.
9. Patel, D. S., Maravi, P. K., Raghuwanshi, K., & Karsoliya, S. (2026). “Robust Explainable and Calibrated Machine Learning for Phishing URL Detection under Constrained Feature Perturbations.” *International Journal of Artificial Intelligence, Cybersecurity and Emerging Technologies*, 1(2), 36–42. DOI: [10.68307/a3mqv686](https://doi.org/10.68307/a3mqv686). Publisher abstract/record reviewed; full paper unavailable in project audit.
10. Buu, S.-J., & Cho, S.-B. (2025). “A Transformer network calibrated with fuzzy logic for phishing URL detection.” *Fuzzy Sets and Systems*, 517, 109474. DOI: [10.1016/j.fss.2025.109474](https://doi.org/10.1016/j.fss.2025.109474). Venue corrected from DOI-linked publisher record; detailed methods require full-text verification.
11. Potpelwar, R. S., Kulkarni, U. V., & Waghmare, J. M. (2025). “LegitPhish: A large-scale annotated dataset for URL-based phishing detection.” *Data in Brief*, 63, 111972. DOI: [10.1016/j.dib.2025.111972](https://doi.org/10.1016/j.dib.2025.111972). Dataset record: [Mendeley Data v2](https://data.mendeley.com/datasets/hx4m73v2sf/2).
12. Mohammad, R., & McCluskey, L. (2012). *Phishing Websites* [Dataset]. UCI Machine Learning Repository. DOI: [10.24432/C51W2X](https://doi.org/10.24432/C51W2X).
13. PhishTank. [Developer Information and Feed Fields](https://phishtank.org/developer_info.php). Access and terms must be checked at collection time.
14. URLhaus. [Community API and Submission Policy](https://urlhaus.abuse.ch/api/).
15. Le Pochat, V., Van Goethem, T., Tajalizadehkhoob, S., Korczyński, M., & Joosen, W. (2019). “Tranco: A Research-Oriented Top Sites Ranking Hardened Against Manipulation.” *Proceedings of the 26th Annual Network and Distributed System Security Symposium*. [Tranco project](https://tranco-list.eu/).
16. Berners-Lee, T., Fielding, R., & Masinter, L. (2005). *Uniform Resource Identifier (URI): Generic Syntax*. RFC 3986. [RFC Editor](https://www.rfc-editor.org/rfc/rfc3986).
17. scikit-learn developers. [Probability calibration and `CalibratedClassifierCV` documentation](https://scikit-learn.org/stable/modules/calibration.html). Software documentation; cite the version actually used in future experiments.
18. SHAP developers. [`TreeExplainer` documentation](https://shap.readthedocs.io/en/latest/generated/shap.TreeExplainer.html). Software documentation; cite the version actually used in future experiments.
