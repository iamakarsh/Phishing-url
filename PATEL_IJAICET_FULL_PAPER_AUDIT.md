# Full-Paper Access Audit: Patel et al. (IJAICET)

**Article:** “Robust Explainable and Calibrated Machine Learning for Phishing URL Detection under Constrained Feature Perturbations”  
**Authors listed by publisher:** Dhyanendra Singh Patel, Prakash Kumar Maravi, Kamlesh Raghuwanshi and Surabhi Karsoliya  
**DOI:** [10.68307/a3mqv686](https://doi.org/10.68307/a3mqv686)  
**Publisher page:** [IJAICET article record](https://ijaicet.com/index.php/ijaicet/article/view/41)  
**Audit date:** 18 November 2026

## Access outcome and evidence boundary

**The full paper or an accessible author manuscript was not obtained.** This audit therefore stops at the publisher abstract and bibliographic record. It does not treat abstract-level descriptions as a substitute for methods or results sections.

Access attempts:

1. Opened the publisher article page and DOI URL through the available web reader. The direct URL opens were reported inaccessible, although search indexing returned the publisher article record and abstract.
2. Followed the publisher’s PDF/galley link. The reader resolved this to the article galley page (`/article/view/41/45`), then returned **Cache miss** when opened.
3. Tried direct publisher PDF/download URL variants through the web reader. They were inaccessible through that reader.
4. Tried a direct HTTPS download from the publisher host to a temporary location. The environment returned **“No such host is known.”** No file was downloaded.
5. Searched the exact title, DOI and author/title combinations for a separate PDF or author manuscript. The results returned the same publisher landing page/abstract and galley reference; no distinct accessible manuscript surfaced.

The publisher record identifies this as an article published 1 November 2026, *International Journal of Artificial Intelligence, Cybersecurity and Emerging Technologies*, 1(2), 36–42. These are landing-page metadata, not findings about the article’s methods. The abstract reports a PhiUSIIL-based study, five model families, two feature conditions, calibration analysis, TreeSHAP and greedy feature-space evasion. It names no complete model list, feature inventory, calibration algorithm, perturbation operators, or selective-prediction experiment. The page is the evidence source for all abstract-level details below: [publisher record and abstract](https://ijaicet.com/index.php/ijaicet/article/view/41).

## Requested-method audit

| Question | What the accessible abstract/record says | Full-text finding / status |
|---|---|---|
| **1. Dataset** | Abstract names the **PhiUSIIL** dataset. It also says the study uses a “normalized domain grouped development protocol” and one frozen test assessment. | **Unavailable from full text.** The exact downloaded dataset version, original/processed sample count, class balance, deduplication/filtering, label handling, data source timestamps, and any exclusions cannot be verified. The record does not name another evaluation dataset. Do not infer that none was used beyond the abstract’s summary. No page/section reference can be supplied without the paper. |
| **2. Features** | Abstract says “conservative and full sensitivity feature conditions” and mentions “calibration permutation importance Tree SHAP explanations and greedy feature space evasion.” | **Unavailable from full text.** No exact features or feature counts, definitions, provenance, URL-only versus derived fields, condition membership, leakage controls, or transformation/normalization details are exposed in the abstract. |
| **3. Models** | Abstract says **five model families** were compared and names **HistGradientBoosting** as the conservative uncalibrated model associated with the reported locked-test values. | **Partially known from abstract; complete list unavailable.** The other four models, hyperparameters, tuning protocol, selected model for each condition, and whether the same models were assessed across both feature conditions are not stated in the accessible abstract. |
| **4. Train/test splitting protocol** | Abstract describes a “normalized domain grouped development protocol” and “one frozen test assessment.” | **Unavailable from full text.** Exact group key, partition proportions, date boundaries, seed(s), train/validation/calibration/test assignment, how model selection and threshold choice were separated from final test, and whether multiple test conditions were used are unknown. The phrases in the abstract do not establish a temporal or external split. |
| **5. Calibration: method and evaluation** | Abstract says calibration was analysed; it reports a Brier score of **0.000041** for conservative **uncalibrated** HistGradientBoosting on the locked test assessment. | **Unavailable from full text.** No calibration methods (e.g., temperature scaling, isotonic, Platt/sigmoid), calibration-fit split, calibration target/model, reliability diagrams, ECE definition/binning, log loss, uncertainty interval, or baseline comparison are available. The reported Brier score is an abstract-level number and does not by itself prove calibration quality or a calibrated policy. |
| **6. Perturbation types and semantic validity** | Abstract describes “constrained adversarial robustness,” “greedy feature space evasion,” and attack success at **three changes (0.7042)** and **five changes (0.8462)**. | **Unavailable from full text.** The exact feature changes, constraints, attack objective, whether changes map back to syntactically valid URLs, whether benign/phishing meaning is preserved, and whether semantic validity was reviewed or tested are not stated. “Constrained” alone does not establish semantic validity. |
| **7. TreeSHAP and explanation-change measurement** | Abstract says TreeSHAP explanations were analysed and reports mean SHAP cosine similarity falling to **−0.350** (context: perturbation/robustness analysis). | **Partially known from abstract; procedure unavailable.** The tree model(s) explained, SHAP background/reference data, per-instance versus aggregate computation, feature attribution normalization, cosine formula/aggregation, comparison pairs, sample scope, and uncertainty estimates are not available. We cannot determine how the reported mean was computed or whether it is a local per-instance stability score suitable for a decision policy. |
| **8. Confidence-based abstention/selective prediction** | The abstract does not mention abstention, reject options, deferral, coverage, selective risk, risk–coverage curves, or a selective policy. | **Cannot determine from abstract whether the full paper implements any.** Absence from the abstract is not enough to conclude absence from the article. The full text is required. |
| **9. Explanation stability as a per-instance decision/deferral signal** | The abstract describes explanation stability as part of analysis/evaluation; it does not say that stability controls a prediction, acceptance, or deferral decision. | **Not established.** The accessible record provides no evidence that explanation stability is fed into an inference-time per-instance policy. Full-text verification is needed before making a definitive negative claim. |
| **10. Matched-coverage confidence-only versus confidence-plus-stability** | No such comparison is described in the abstract. | **Not established in the accessible evidence.** No coverage points, confidence-only arm, combined arm, matched-coverage risk comparison, or paired statistical test is mentioned. Do not assert the full paper definitively lacks one without the methods/results text. |
| **11. External or shifted-data evaluation** | Abstract names PhiUSIIL and a “frozen test” but does not identify a separate external dataset, temporal holdout, or distribution-shift protocol. | **Unavailable from full text.** The abstract does not establish external or shifted evaluation. The domain-grouped protocol is a split-control description, not by itself proof of distribution shift. |

## Abstract-level numerical claims (not independently reproduced)

The publisher abstract reports for the conservative **uncalibrated HistGradientBoosting** condition: locked-test recall **0.999897**, F1 **0.999949**, and Brier score **0.000041**. It also reports attack success **0.7042** after three changes and **0.8462** after five changes, and mean SHAP cosine similarity **−0.350**. These are recorded only as claims printed in the abstract. The inaccessible full paper prevents checking denominators, exact definitions, uncertainty, test-set size, and computation. They are not results from the capstone project.

## Implications for the capstone research gap

The abstract establishes that this article is a close adjacent study in phishing URL detection: it combines grouped evaluation, probability/calibration analysis, TreeSHAP explanation analysis, and constrained feature-space attacks. It therefore further weakens broad claims that no prior work has examined calibration, explanation changes, and perturbation robustness together for phishing URLs.

However, **the accessible evidence does not show that explanation stability is used as a per-instance deferral signal, or that a confidence-plus-stability policy is compared with calibrated confidence alone at matched coverage.** That remains a plausible distinction from this paper, but it cannot yet be asserted as a verified full-text difference. The article’s full methods and results must be obtained before using it to make a definitive novelty-boundary claim. The capstone question should remain unchanged pending that verification and the research lead’s approval.

## What remains necessary to complete this audit

Obtain the publisher galley PDF or an author manuscript through a working publisher/author/institutional source. Then inspect the methods and results pages, record page/section references, and replace each “unavailable/not established” entry only with directly observed evidence. Until then, do not infer exact features, calibration methods, semantic validity, selective-policy absence, or shift-evaluation absence from the abstract.
