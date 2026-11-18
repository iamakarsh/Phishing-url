# Research Context

## Project identity

- **Title:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability
- **Domain:** Responsible, Secure & Applied AI; machine learning, cybersecurity, data science.
- **Project outcome stated in Phase 1:** Capstone project and research paper, with a functional web-based demonstration prototype.
- **Evidence date in the research documents:** 6 October 2026. The present handover was prepared on 18 November 2026. The literature findings below are not a claim that the literature was comprehensively refreshed through 18 November.
- **Workspace:** `C:\Users\akars\OneDrive\Desktop\Resources\Research Project`

## Research question

The current refined question, to preserve verbatim, is:

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

The original Phase 1 question was: “Can combining calibrated prediction confidence with explanation reliability reduce risky automatic decisions in phishing URL detection compared with confidence-based selective prediction alone?” The current wording is narrower and operationalizes the comparison. A supporting question in Phase 1 asks how well conventional models classify URL lexical/structural inputs, but that is a baseline objective, not the principal novelty claim.

## Background, problem and motivation

Phishing URLs can be represented using lexical and structural properties such as URL length, subdomain structure, character patterns, special characters and suspicious terms. Conventional classifiers force a phishing/legitimate label even when a prediction may be wrong. In this setting, a false negative can leave a user exposed to a malicious URL; a false positive can block or warn on a legitimate URL. Average accuracy alone does not tell an operator which individual decisions are safe to automate.

The project studies selective prediction: provide an automatic phishing/legitimate result for some inputs and defer other cases for review. It focuses on calibrated predictive confidence as the baseline trust signal and tests whether local explanation reliability/stability contributes independent information. SHAP is proposed for local explanations; stability would be measured by the consistency of attributions under controlled URL variants. This work is an empirical test of a triage signal, not a claim that an explanation is causal or intrinsically trustworthy.

**Important distinction:** “Explanation stability” means that attribution results remain sufficiently similar under a defined perturbation protocol. It is not the same as explanation faithfulness, model confidence, prediction robustness, calibration, or semantic validity of the perturbation. These must be measured or controlled separately. A stable explanation can still consistently reflect a spurious feature.

## Project objectives recorded in Phase 1

1. Build phishing/legitimate URL classifiers using lexical and structural URL features.
2. Compare suitable models, initially including Random Forest, XGBoost and Logistic Regression.
3. Evaluate and, where appropriate, calibrate predicted probabilities.
4. Implement selective prediction so low-trust cases can be deferred.
5. Use SHAP to explain individual predictions.
6. Test whether explanation reliability/stability adds signal beyond confidence.
7. Compare conventional classification, confidence-based selective prediction, and confidence-plus-explanation-reliability prediction.
8. Assess classification, calibration, selective risk and explanation metrics.
9. Evaluate on independent or shifted data.
10. Build a web prototype for demonstration, subject to remaining capstone requirements.

The initial scope excludes email phishing, full webpage-content analysis as the main task, browser extension development, malware analysis, LLM-based detection, and enterprise deployment. Phase 2 later flags that PhiUSIIL contains webpage/content/security-derived fields; a project described as URL-only must exclude these fields or report them in a separate condition.

## Literature-review scope and confidence in findings

The existing review is concept-led/scoping, not a fully reproducible PRISMA review. Its searches used combinations around phishing URL detection with XAI/SHAP/LIME; confidence, calibration and uncertainty; selective prediction, abstention and reject option; explanation stability/reliability/attribution drift; adversarial URL perturbation; and cross-dataset/domain shift. The earlier conversation asked for broad 2018–October 2026 coverage and high priority to 2024–2026, including the 28 supplied query combinations and named papers.

Publisher/repository/indexed pages reviewed included Frontiers, Scientific Reports/Nature, Springer Nature, ScienceDirect/Elsevier, arXiv, IEEE Xplore, SSRN and institutional repositories. Direct access to every database, Google Scholar/Semantic Scholar, subscription full text, and supplements was not confirmed. Therefore:

- “No direct match found” means no directly matching study was located in the searched material, not proof of absence.
- Where the matrix says “verify,” “not captured,” or “not established,” do not replace that with a guessed value.
- Phase 2’s literature matrix in the DOCX is a short thematic table; the XLSX is a separate 17-study, 21-column matrix. They are complementary and have coverage gaps.
- The main reviewed evidence cutoff is 6 October 2026. Papers posted after that cutoff need a fresh search before a final novelty claim. A search result with a publication date after today (for example, the ExpertFusion record dated 15 January 2027) must not be treated as published as of this handover date.

## Literature synthesis and novelty boundaries

### What has already been studied

- Classical and neural phishing URL classification, including lexical/structural features and raw token/character representations.
- SHAP and LIME for phishing URL/website explanations, feature importance and feature selection. Adding SHAP/LIME is not a novelty claim.
- Probability and uncertainty estimates on phishing URLs, including probabilistic neural networks and confidence intervals.
- Calibration/probability-quality evaluation and threshold/risk framing in recent phishing classifiers. A Brier score or reliability diagram alone does not establish a fully calibrated selective policy.
- Selective prediction/reject-option methods in general ML, and an SSRN preprint applying calibrated confidence and abstention to a cross-modal forensics benchmark that includes phishing URLs.
- Cross-dataset generalization, UDA, external validation, domain shift and temporal data concerns.
- Adversarial URL manipulation and explanation-stability measurement. Ahamed et al. (2026) directly evaluates SHAP top-K stability under controlled URL perturbations and combines it with robustness/generalization/external evaluation.
- Security-focused evidence that predictive robustness and explanation stability can diverge.
- October 2026 SHAP-guided adaptive feature weighting and federated learning for phishing URLs. It studies interpretability/non-IID feature weighting, not attribution-stability-based deferral.

### What remains insufficiently explored in the reviewed evidence

The narrower, potentially defensible gap is the **incremental decision value** of per-instance explanation stability beyond properly calibrated prediction confidence: compare confidence-only selective prediction with a confidence-plus-stability policy at matched coverage, and test this on in-domain and shifted data. The reviewed sources do not establish that explanation stability is used as the abstention signal in a phishing URL system, or that an ablation demonstrates additional risk reduction beyond calibrated confidence.

This gap remains provisional. OACSP-Forensics (2026 SSRN) already contributes phishing URL selective prediction, calibration, coverage-risk curves, and external URLhaus/Tranco evaluation. It is therefore a close confidence-only selective baseline and must be discussed. It does not report explanation stability in the retrieved SSRN abstract. A future or later paper could invalidate the remaining gap; run updated database/full-text searches before freezing the Phase 2 paper.

### Boundaries on novelty claims

- **Invalid/broad claim:** “First to study explanation stability for phishing URL detection.” Ahamed et al. (2026) already study this directly.
- **Too broad:** “First to integrate XAI, probability quality and risk/threshold framing.” Khan et al. (2026) already combine SHAP, Brier/reliability evaluation and threshold/risk discussion.
- **Too broad:** “First phishing URL selective prediction or confidence-based abstention.” Ghalechyan et al. (2024) use uncertainty intervals to mark cases crossing a threshold as unknown; OACSP-Forensics (2026) explicitly evaluates calibrated selective prediction and abstention on phishing URLs.
- **Potentially defensible, subject to further search:** test whether per-instance local explanation stability supplies incremental selective-risk reduction beyond calibrated confidence at matched coverage, especially under domain/temporal shift.
- Do not call feature importance “explanation reliability,” fuzzy rule-based calibration “probability calibration,” or perturbation robustness “explanation stability” without describing the exact operation.

## Current hypotheses

These four hypotheses are written in the current Phase 2 document:

- **H1:** Lower local explanation stability is associated with higher prediction error after controlling for calibrated confidence, model, class and domain.
- **H2:** At matched coverage, a confidence-plus-stability policy reduces selective risk compared with calibrated confidence alone.
- **H3:** The incremental value of explanation stability is larger under temporal/domain shift and perturbation families that cause attribution drift.
- **H4:** Invalid or meaning-changing URL edits can produce misleading stability signals; perturbation validity must therefore be controlled.

## Proposed methodology

The methodology is still proposed, not implemented or locked. Current direction:

1. Select one or more phishing URL datasets and audit labels, provenance, feature computability, leakage, exact URL overlap and registrable-domain overlap.
2. Use at least training, calibration/validation and held-out test data. Keep calibration and policy/threshold selection off the final test set.
3. Train conventional URL classifiers; initial candidates include Random Forest, XGBoost and Logistic Regression. A character/token model is a potential comparison if scope permits.
4. Calibrate confidence appropriately and assess calibration on independent data.
5. Generate local SHAP explanations for individual predictions.
6. Define controlled, semantically valid URL perturbations. Separate prediction-preserving edits used to measure explanation stability from adversarial edits that change predictions. Validate semantic equivalence and do not infer equivalence from syntax alone.
7. Choose and preregister one primary stability metric after literature verification; candidates include top-k Jaccard, attribution rank correlation, cosine similarity, signed agreement, normalized L1/L2 drift.
8. Fit/choose policy thresholds on validation data and compare at matched coverage.
9. Include temporal and/or external cross-dataset evaluation.
10. Report confidence intervals, effect sizes and operational cost/risk, not just headline accuracy.

### Required comparison arms A, B and C

These are explicitly set in Phase 1 and should be preserved:

- **Approach A — Conventional classification:** model always returns phishing or legitimate; no abstention.
- **Approach B — Confidence-based selective prediction:** return a prediction only when calibrated confidence meets a selected threshold; otherwise defer for further review.
- **Approach C — Proposed risk-aware approach:** use both prediction confidence and explanation reliability/stability; defer potentially unreliable cases even when confidence alone appears adequate.

Compare B versus C to test the main question. A is the full-coverage reference and helps quantify the effect of abstention generally. Phase 2 also mentions confidence-only, stability-only, and confidence-plus-stability variants. Stability-only is a useful ablation, but it does not replace A/B/C.

## Evaluation metrics and statistical plan

The existing documents propose, but do not finalize, these metrics:

- **Classification:** precision, recall, F1, ROC-AUC, PR-AUC, false-negative rate; accuracy may be included but is not sufficient alone.
- **Calibration:** Brier score, log loss, ECE/adaptive ECE and reliability diagrams. Report class/domain breakdowns where feasible. Distinguish ranking quality from calibration.
- **Selective prediction:** coverage, selective risk/error, risk-coverage curve, area under the risk-coverage curve (AURC), fixed-coverage risk, defer/review rate, and false-negative rate among automatically accepted predictions. Include phishing-specific asymmetric costs.
- **Explanation stability:** select one primary metric among top-k Jaccard, rank correlation, cosine/signed agreement or normalized attribution drift. Report sensitivity analyses and perturbation validity.
- **Generalization:** performance and calibration on external/temporal/domain-shift test data.
- **Statistics:** paired bootstrap confidence intervals and practical effect sizes. Compare B and C on the same examples at matched coverage. The resampling unit should respect URL/domain/campaign clustering where possible; otherwise correlated variants can inflate apparent precision.

No performance results for this capstone have been generated. All figures reported below belong to cited papers, not this project.

## Candidate datasets and open choices

### Candidate datasets already named

- UCI Phishing Websites Dataset — Phase 1 candidate; verify whether its feature/table format matches a URL-only task and current provenance needs.
- PhiUSIIL Phishing URL Dataset — recommended in Phase 2 as a large reproducible starting point. Reported datasets differ slightly in counts (235,795 original in cited records; 235,370 after filtering/deduplication in Ahamed et al.). Audit for leakage and exclude or separately analyze non-URL fields.
- LegitPhish Dataset — named in Phase 1; availability and characteristics still need verification.
- PhishTank snapshot — candidate external phishing source. Needs time-stamped snapshot, label validation, benign comparison set, domain de-duplication and documentation. Some papers pair phishing-only PhishTank records with benign examples from another dataset, which can confound source/domain.
- URLhaus with Tranco benign controls — used by OACSP-Forensics for out-of-distribution phishing URL evaluation, according to its SSRN abstract. Verify full details and licensing/label protocol.
- Other recent publicly available datasets — not selected.

### Unresolved dataset decisions

- Final primary training set and its feature representation.
- Whether project remains strictly URL-only; if yes, remove/segregate webpage/content/security features.
- Exact external/temporal test source, snapshots and dates; availability and terms.
- How to obtain independent benign URLs and prevent source/label shortcuts.
- Exact URL and registrable-domain deduplication protocol; campaign-level overlap proxy.
- Whether resources/time support a neural character/token model in addition to tree/linear baselines.

## Important papers and matrix-level findings

“Unknown/not verified” below reflects the source material inspected. Check primary full text before promoting a claim. The full existing 17-row extraction is in `phishing_xai_literature_matrix.xlsx` (sheets: Literature matrix, Categories, Novelty audit). The Phase 2 DOCX adds items absent from that workbook.

| Paper | Methods/data/results documented in available records | Relevance and limitations |
|---|---|---|
| Ahamed, Kakon, Farid, Uddin & Abdul Karim (2026), “An integrated evaluation protocol for adversarial robustness, generalization, and explanation stability in URL-based phishing detection,” *Frontiers in Computer Science* 8, DOI 10.3389/fcomp.2026.1834407. | LR, XGBoost, CharCNN, BERT-base; PHIUSIIL after validation/deduplication 235,370 URLs (100,520 phishing; 134,850 benign) and 49,121 PhishTank phishing URLs. Controlled subdomain, padding, path, homoglyph/character and compositional attacks; SHAP top-K Jaccard drops to about 0.526–0.535 under subdomain perturbation; leave-one-family-out and external composite evaluation. | Closest stability study. It evaluates robustness/stability, not confidence-calibrated selective deferral or the incremental value of stability beyond confidence. Its cross-source composite uses a PHIUSIIL benign split because PhishTank has no benign class; this construction needs careful interpretation. |
| Ghalechyan, Israyelyan, Arakelyan, Hovhannisyan & Davtyan (2024), “Phishing URL detection with neural networks: an empirical study,” *Scientific Reports* 14, 25134, DOI 10.1038/s41598-024-74725-6. | Deterministic BERT-based NN and probabilistic NN/weight-uncertainty approach; URL-only feature engineering. Training pool: 220k phishing and 220k benign from PhishTank, OpenPhish, private data and Alexa benigns. Test includes PhishTank/OpenPhish and production-collected URLs; 37.5k phishing and 40.5k legitimate production URLs, plus additional feeds. PNN draws 100 inferences per URL; 95% probability intervals; 18.13% of false deterministic predictions vs 1.97% of true predictions had uncertainty intervals crossing the decision boundary in their sampled validation comparison. | Directly relevant: probabilistic uncertainty and “unknown” outputs already exist for phishing URLs. Not explanation-stability-aware; calibration in the formal calibration sense is not established by the abstracted evidence. Data-source mixture, possible label errors, and incomplete source tracking after shuffle are limitations. DOI in Phase 2 reference list is wrong; correct DOI verified above. |
| Rashid, Doyle, Han & Seneviratne (2024), “Phishing URL detection generalisation using Unsupervised Domain Adaptation,” *Computer Networks* 245, 110398, DOI 10.1016/j.comnet.2024.110398. | Three datasets; own PhishStats collection included 46,250 phishing URLs plus benign sources. Evaluates existing ML/DL across datasets and feature alignment/UDA; average cross-dataset F1 gain 0.06 and up to about 0.2. | Motivates shift evaluation. No XAI, calibrated confidence, explanation stability or selective prediction. |
| Buu & Cho (2025), “A Transformer network calibrated with fuzzy logic for phishing URL detection,” *Fuzzy Sets and Systems* 517, 109474, DOI 10.1016/j.fss.2025.109474. | Accessible record claims over one million URLs and reports 98.93% accuracy, 98.54% precision, 97.84% recall, F1 98.03%; Transformer plus fuzzy logic. Exact data, calibration definition and evaluation require full paper. | Do not equate fuzzy-logic calibration with statistical probability calibration without full methods. Phase 2 document lists *Future Generation Computer Systems*, conflicting with DOI/publisher metadata; resolve/correct before citation. No explanation stability found in accessible record. |
| Tawfik, Abu-Ein, Abdelhaliem et al. (2025), “Explainable few-shot learning with modern BERT for detecting emerging phishing attacks using XF PhishBERT,” *Scientific Reports* 15, 42821, DOI 10.1038/s41598-025-27500-0. | Phase 2 record says DATA1 has 112 engineered features and PhiUSIIL has 235,795 URLs; BERT few-shot/meta-learning, SHAP/counterfactual explanations; uncertainty based on information completeness is mentioned in accessible excerpt. | Relevant uncertainty/XAI/novel-attack overlap. Exact model, uncertainty formulation, calibration, temporal split and abstention are not fully verified; no stability-driven deferral identified. |
| Panthi, Patidar, Zalawadia, Vekariya et al. (2026), “A performance analysis of SHAP and LIME for interpreting boosting models in phishing URL detection,” *Scientific Reports*, DOI 10.1038/s41598-026-66800-x, published 25 Aug 2026. | Dataset 11,054 URLs, 30 URL-based features; GBM, XGBoost, LightGBM; SHAP/LIME, 10-fold CV; LightGBM accuracy 97.60%. Paper describes differing stability/scope and SHAP global consistency. | Not local explanation-stability-driven abstention; no calibration, selective comparison, controlled URL perturbation or cross-dataset test identified. |
| Khan, Akter, Lindon, Akter, Rasul, Rahman, Tohfa & Hossain (2026), “Explainable AI for phishing URL detection: a Bayesian-optimized stacking ensemble framework with SHAP-guided feature learning,” *Frontiers in Artificial Intelligence* 9, art. 1854934, DOI 10.3389/frai.2026.1854934, published 2 Oct 2026. | Phishing URL Detection Dataset: 247,950 samples, 41 features (128,541 legitimate; 119,409 phish); 12 classifier baselines and stacking ensemble, 13 model outputs; SHAP feature selection/explanations, Optuna tuning; Brier score 0.0246 and reliability diagrams; threshold/risk framing. | Closest XAI+probability/risk bundle but no attribution stability or formal matched-coverage selective risk; one random in-dataset split, no cross-dataset/temporal OOD. Brier score assesses probabilistic accuracy but does not by itself establish that a post-hoc calibrator was fit. |
| Alsuqayh, Alhogail & Mirza (2026), “Hybrid feature selection with SHAP-based explainable AI for interpretable phishing website detection,” *Journal of King Saud University Computer and Information Sciences* 38, 894, DOI 10.1007/s44443-026-01281-6, published 30 Sep 2026. | PhiUSIIL 235,795 instances, 53 URL/web/content/security features; DT, RF, XGBoost, LightGBM, CatBoost; mutual information + chi-square + SHAP selection, fold-wise 10-fold CV and external URL-only dataset. | Recent XAI/generalization work. Not a confidence/stability decision signal; external dataset details should be recovered from the full paper. |
| Kouser, Aknan, Singh, Saleem et al. (2026), “SHAP-guided adaptive feature weighting for phishing URL detection with a federated learning extension,” *Scientific Reports*, DOI 10.1038/s41598-026-71965-6, published 5 Oct 2026. | Hannousse–Yahiouche benchmark: 11,430 URLs, 87 features; seven architectures including DistilBERT; XGBoost central accuracy 97.00%; 20 simulated federated clients; federated SAFW-Hybrid 95.4% ±0.5%; reports non-IID/feature weighting and latency. | Important late paper in current Phase 2 record, absent from earlier XLSX. SHAP initializes trainable feature weighting and federated interpretability; not local explanation stability, calibrated confidence or abstention. |
| Khan, Muhammad Shahnawaz (2026), “OACSP-Forensics: A Cross-modal Selective Prediction Benchmark for Court-admissible Digital Forensic Triage,” SSRN, posted 1 Aug 2026, DOI 10.2139/ssrn.7140758. | Ordinal-aware class-conditional selective head + post-hoc temperature scaling. PhiUSIIL 235,795; OOD URLhaus phishing with Tranco benign controls; coverage-risk curves and calibration diagnostics. Abstract reports URL classifiers collapse to chance-level on live phishing feeds and audits PhiUSIIL for one label-leaking feature and 28 webpage-derived features unavailable at URL-only triage. | Important near-neighbor because calibrated confidence, abstention, shift, and phishing URLs coexist. SSRN preprint, not peer-reviewed evidence in the available record; no explanation-stability signal in abstract. Read full methods/code, audit metrics and verify claims before reliance. |
| Rajhans & Khawarey (2026), “Beyond Gradient-Based Attacks: Adversarial Robustness and Explainability Stability in Cybersecurity Classifiers,” arXiv:2607.01679. | Multi-domain security tabular study including phishing URLs, UNSW-NB15, NF-ToN-IoT, HIKARI-2021; RF/XGBoost; TreeSHAP/Explainability Stability Index; five attacks. Abstract reports ESI about 0.06–0.16 for XGBoost under attacks despite apparently high robustness in one attack condition. | Preprint; supports distinctness of predictive robustness and explanation drift; not calibrated selective prediction. Phishing-specific sample/protocol details remain to verify. |
| “Phishing URL detection generalisation using UDA” is above; “Can Features for Phishing URL Detection Be Trusted Across Diverse Datasets? A Case Study with Explainable AI” (arXiv:2411.09813). | SHAP feature-trust comparison across multiple URL datasets; author list, models, sample counts and exact measures not recovered from abstract in prior review. | Cross-dataset feature attribution consistency is related but not equivalent to local perturbation stability or selective deferral. Verify the full paper. |
| “Explainable phishing website detection for secure and sustainable cyber infrastructure” (2025), *Scientific Reports*, DOI 10.1038/s41598-025-27984-w. | ML website detection with SHAP, per the article record; dataset details and authors not extracted in existing matrix. | Conventional explainability context, not stability/selective decision. Recover full metadata before citing. |
| “An explainable feature selection framework for web phishing detection with machine learning,” *Data Science and Management* 8(2), 127–136, DOI 10.1016/j.dsm.2024.08.004, journal issue 2025/online 2024. | SHAP/LIME aggregated feature selection; three ML models according to indexed abstract; exact sample counts/authors not captured. | Feature selection, not explanation reliability or selective risk. Verify full record. |
| “Explainable AI For Phishing Detection: Techniques, Challenges, and Experimental Validation,” IEEE 2025 RAICS, document 11332593. | Decision Tree + SHAP/LIME; Kaggle Phishing URLs; abstract reports 96% baseline and 90% under PGD-inspired bounded perturbations. | Predictive robustness under perturbation is reported; attribution-stability/abstention not established. Metadata/sample size not recovered. |
| “Evaluating Large Language Models for Phishing Detection, Self-Consistency, Faithfulness, and Explainability,” arXiv:2506.13746 (2025). | Abstract describes CC-SHAP/token alignment and phishing classification for LLMs; datasets/models need full-text check. | Related consistency vocabulary, but LLM rationale consistency is not necessarily SHAP stability of URL classifiers. |
| “EXPLICATE: Enhancing Phishing Detection through Explainable AI and LLM-Powered Interpretability,” arXiv:2503.20796 (2025). | Dual LIME/SHAP and LLM natural-language explanation layer per abstract; counts/models not captured. | Explanation usability, not measured reliability or abstention. |
| “Transparent and trustworthy CyberSecurity: an XAI-integrated big data framework for phishing attack detection,” *Frontiers in Big Data* (2025), DOI 10.3389/fdata.2025.1688091. | LIME/SHAP and phishing detection per indexed article. | Trust framing does not establish measured explanation stability; details require full paper. |
| “Phishing URL detection with neural networks: an empirical study” above is the source for Phase 2 uncertainty claim. | It uses 100 probabilistic inferences and interval bounds; an interval crossing 0.5 may be treated as “unknown.” | This is a confidence/uncertainty deferral precedent and should be explicitly discussed in final novelty audit. |
| “Effect of Confidence and Explanation on Accuracy and Trust Calibration in AI-Assisted Decision Making,” arXiv:2001.02114 (2020). | Human-subject AI-assisted decision studies with displayed confidence and local explanations. | Human trust calibration, not model-level explanation reliability/URL abstention. Authors were not verified in existing report. |
| “Survey on Leveraging Uncertainty Estimation Towards Trustworthy Deep Neural Networks: The Case of Reject Option and Post-training Processing,” arXiv:2304.04906 (2023). | General survey of uncertainty and reject-option methods. | Supports methodological background, not phishing-specific novelty. Author metadata should be checked. |
| OACSP-Forensics is above. The former XLSX also has category/novelty sheets noting general selective prediction as mature. | Generic literature includes confidence/uncertainty-based selective classification. | Do not claim novelty for abstention itself. |

### Other items listed but not validated in the current workspace

- **“ExpertFusion: Calibrated multi-expert decision fusion for phishing URL detection under distribution shift and target-prior uncertainty.”** Phase 2 DOCX lists ExpertFusion as an adjacent work but gives no full citation. A later indexed result surfaced with *Expert Systems with Applications* date **15 January 2027**, which is future relative to 18 November 2026. Do not treat this as an available published study until a valid publication/preprint date is established. If it becomes available, it is close on calibrated confidence + distribution shift but the retrieved abstract does not indicate explanation stability.
- Phase 2 DOCX includes the OACSP-forensics title/date only in its summary matrix and references; use its SSRN record for complete citation and retain its preprint status.
- The older XLSX includes generic rows P16/P17 and a category row P17; not all are individual primary-paper records. Do not misstate the workbook as a 17-paper systematic dataset.

## Dataset/validation cautions taken from reviewed papers

- The Ahamed et al. study’s PHIUSIIL version is 235,370 after processing; other records cite original PhiUSIIL at 235,795. Track the actual file/version and preprocessing.
- OACSP reports one label-leaking feature and 28 webpage-derived features not computable at URL-only decision time in PhiUSIIL. The capstone needs its own explicit feature audit before selecting a benchmark.
- Ghalechyan et al. report that source identifiers were not retained through random train/validation shuffling and note possible label errors in live feeds. Avoid treating high accuracy across mixed feeds as clean independent generalization.
- A dataset-shift evaluation that pairs phishing URLs from one source with benign URLs from another can introduce source/domain shortcuts. Use independently sourced controls and disclose this risk.
- Near duplicates, same registrable domains, campaigns, and collection-time leakage can invalidate random splitting. Split and bootstrap at an appropriate group/time unit.

## Important decisions and rejected approaches

- Project does **not** claim novelty for phishing detection, SHAP/LIME, calibration, confidence, uncertainty, selective classification, adversarial robustness, or explanation stability considered individually.
- Original preliminary gap was updated after the literature review: measure whether stability adds value beyond calibrated confidence; broad first-use framing rejected.
- The project's current primary question should remain the exact wording above unless the research lead explicitly approves a change.
- No final classifier, primary dataset, external dataset, perturbation set, calibration algorithm, primary explanation metric, operational cost matrix, or model complexity has been approved in available files.
- No experiments or models have been run for this project. Published results in the matrix are literature results only.
- No mentor/advisor feedback, formal rubric, deadlines, or required format checklist were found in the accessible conversation/workspace. Do not invent them.
- Preserve all existing files. Do not overwrite or delete project materials; create versioned additions or ask before edits.

## Current implementation status and next steps

**Status:** research-definition and literature-review stage. No implementation files, dataset files, trained models, evaluation outputs, web prototype or code repository were present in the inspected project root. Phase 1 names Python, scikit-learn, XGBoost, Pandas, NumPy, SHAP, Matplotlib/Seaborn, FastAPI, React/Vite/Tailwind and optional MongoDB, but these are tentative stack choices, not an approved implemented system.

Recommended next sequence:

1. Obtain/confirm instructor or mentor rubric, deliverables, deadlines, feasibility and requested scope; no mentor requirements were recoverable.
2. Re-run the literature search from 6 October through current date in Google Scholar/Semantic Scholar and subscription databases; screen OACSP and every close study in full; resolve the citation errors listed here.
3. Approve the narrowed RQ and A/B/C test plan with the research lead. Keep the distinction between B and C explicit.
4. Audit candidate dataset versions and fields; decide strictly URL-only vs separate feature conditions; select a valid temporal/external split and independent benign controls.
5. Define semantically valid perturbations and a validation protocol; select/preregister the primary stability metric.
6. Lock model/calibration/selection protocol; then implement A/B/C and no other model training until the research lead approves the plan.
7. Evaluate, report uncertainty and failure cases; only then build/update the prototype and write the Phase 2 paper deliverables.

## Existing research files reviewed

- `phase1.docx` — project problem, motivation, objectives, proposed approach, question, scope, stack, tentative datasets, metrics and A/B/C definitions.
- `Capstone_Phase_2_Literature_Review.docx` — updated Phase 2 narrative, refined RQ/hypotheses/methodology, an abbreviated 12-row thematic table and references through 6 October 2026.
- `phishing_xai_literature_review.docx` — earlier generated review from the prior conversation; includes executive summary, top-15 audit, recommendations and search limitations.
- `phishing_xai_literature_matrix.xlsx` — 17 body rows across 21 columns; sheets: `Literature matrix`, `Categories`, `Novelty audit`. Several entries are summary/generic rows and several author/dataset/method fields are explicitly unverified.

### Bibliographic issues to resolve

1. Phase 2 DOCX cites Ghalechyan et al. DOI `10.1038/s41598-024-77170-7`; the verified publisher/PMC record is `10.1038/s41598-024-74725-6` (Scientific Reports, 14, 25134; 24 Oct 2024). Do not propagate the wrong DOI.
2. Phase 2 DOCX lists Buu & Cho in *Future Generation Computer Systems*. The DOI-linked record and prior review identify *Fuzzy Sets and Systems* 517, 109474 (2025), DOI `10.1016/j.fss.2025.109474`. Confirm against the final publisher record and correct the Phase 2 bibliography when authorized.
3. The online record for the October 2 Khan et al. article identifies eight authors: Hafiz Aziz Khan, Sonia Akter, Abdur Rahman Lindon, Taslima Akter, Iftekhar Rasul, Mamunur Rahman, Nasrin Akter Tohfa and Iftekhar Hossain. Do not cite only “Khan et al.” in the final bibliography without checking the required style.
4. Phase 2 lists an ExpertFusion study with no full citation; the indexed publication date retrieved is Jan. 15, 2027, after the current handover date. Verify a valid preprint/online-first record before including it in an as-of-2026 review.

## References and primary links available from the reviewed conversation/files

- Ahamed et al. 2026: https://doi.org/10.3389/fcomp.2026.1834407
- Ghalechyan et al. 2024: https://doi.org/10.1038/s41598-024-74725-6
- Rashid et al. 2024: https://doi.org/10.1016/j.comnet.2024.110398
- Buu & Cho 2025: https://doi.org/10.1016/j.fss.2025.109474
- XF PhishBERT 2025: https://doi.org/10.1038/s41598-025-27500-0
- Panthi et al. 2026: https://doi.org/10.1038/s41598-026-66800-x
- Rajhans & Khawarey 2026: https://arxiv.org/abs/2607.01679
- Alsuqayh et al. 2026: https://doi.org/10.1007/s44443-026-01281-6
- Khan et al. 2026 stacking paper: https://doi.org/10.3389/frai.2026.1854934
- Kouser et al. 2026: https://doi.org/10.1038/s41598-026-71965-6
- OACSP-Forensics SSRN: https://doi.org/10.2139/ssrn.7140758
- SHAP/LIME feature-selection framework: https://doi.org/10.1016/j.dsm.2024.08.004
- 2025 explainable phishing website detection: https://doi.org/10.1038/s41598-025-27984-w
- Phishing URL feature-trust preprint: https://arxiv.org/abs/2411.09813
- RAICS 2025 IEEE record: https://ieeexplore.ieee.org/document/11332593
- LLM phishing self-consistency preprint: https://arxiv.org/abs/2506.13746
- EXPLICATE preprint: https://arxiv.org/abs/2503.20796
- Transparent/trustworthy cybersecurity XAI: https://doi.org/10.3389/fdata.2025.1688091
- Generic uncertainty/reject-option survey: https://arxiv.org/abs/2304.04906
- Confidence + human explanation study: https://arxiv.org/abs/2001.02114

