# Provisional Conclusion

**Project title:** Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability  
**Status:** This is a pre-experiment conclusion framework. No capstone empirical results exist, so no conclusion about classifier performance or risk reduction can yet be made.

## Research question (retained verbatim)

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

## Provisional conclusion

Phishing URL classifiers can be required to make automatic phishing/legitimate decisions even when their predictions may be wrong. In an operational setting, false negatives may allow malicious links through, while false positives may disrupt access to legitimate sites. Aggregate classification accuracy alone does not identify which individual predictions are suitable for automatic action. Selective prediction addresses this by allowing uncertain cases to be deferred for review.

This project proposes to test whether a local explanation-stability score adds useful selection information beyond calibrated prediction confidence. The study retains three conditions: A, a conventional full-coverage classifier; B, a calibrated-confidence-only policy; and C, calibrated confidence combined with local explanation stability. The primary comparison is B versus C at matched coverage, evaluated on a held-out in-domain set and a documented shifted cohort.

The proposed setup uses a conditionally selected PhiUSIIL primary dataset with URL-only features rebuilt from raw URL strings, a prospective dated PhishTank positive cohort paired with separately verified benign controls, XGBoost as the primary classifier, sigmoid/Platt probability calibration, SHAP TreeExplainer, and mean top-5 absolute-SHAP feature-set Jaccard similarity across accepted URL variants. The dataset, shifted cohort, transformations, model configuration, C selector and statistical plan remain subject to manual checks and mentor approval. They have not been implemented or evaluated.

The study is worth conducting because it can test whether explanation consistency contributes practically useful information to a selective policy, while directly comparing it to a calibrated confidence-only baseline. A finding that C improves at matched coverage could support the use of this signal for the evaluated model and data conditions. A null or negative finding would also be useful: it would indicate that the added explanation computation did not improve selection under the tested protocol, or that the proposed stability measure did not reliably identify errors. Neither outcome should be generalized beyond the study design without further evaluation.

## Empirical status and conclusion boundary

**No empirical conclusion can yet be made.** No dataset has been downloaded and audited for this capstone; no models have been trained; and no calibration, perturbation, SHAP, A/B/C, shifted-data or statistical analysis has been run. Therefore, this document does not claim that explanation stability improves detection, improves selective risk, reduces phishing exposure, or benefits users.

After experiments, the final conclusion should report the matched-coverage B–C risk difference and its paired domain/campaign-cluster bootstrap interval separately for in-domain and shifted evaluation. It should also report phishing false-negative and benign false-positive rates, calibration behavior, achieved operational coverage under frozen policy thresholds, and perturbation validity. If evidence is inconclusive, conflicting across conditions, or sensitive to the perturbation/metric definition, the conclusion should say so directly.

## Limitations to carry into the final conclusion

- The literature review is a targeted scoping review rather than an exhaustive systematic review. The full Patel et al. IJAICET paper was not accessible in the project audit; its full-text methods and any selective-policy experiment remain unknown.
- PhiUSIIL is a URL-and-webpage dataset. The project must rebuild URL-only features, inspect label/provenance fields and prevent domain/source leakage before calling it suitable.
- A prospective PhishTank cohort contributes verified-online phishing examples but no benign class. Benign controls require a separate verification process. Tranco is a sampling frame, not benign ground truth.
- LegitPhish v2 has a one-record discrepancy between its reported total and listed class counts, and its source provenance needs inspection before it is used as an external set.
- Strictly meaning-preserving URL rewrites may produce little feature variation; broader URL modifications can change the destination or intent. The scope of valid perturbations may limit what the stability score measures.
- Explanation stability is not explanation faithfulness, causality, predictive robustness or calibrated confidence. A stable explanation can consistently reflect a spurious feature.
- Calibration can degrade under distribution shift. Matching coverage by test-set score ranking measures ranking quality and differs from applying fixed validation thresholds to a future stream; both analyses should be reported distinctly.
- Domain/campaign clustering, source confounding, label uncertainty, class prevalence and limited capstone time/compute may constrain generalization.

## Future work

Subject to the mentor’s requirements and the results, future work may include broader time-stamped cohorts, additional independent datasets, more diverse and carefully validated perturbation families, alternative local-stability metrics, additional classifier families, recalibration under shift, human review studies, and operational testing. These are future directions only and are not claims of current implementation.

## Literature and novelty boundary

Prior work already studies phishing URL classification, SHAP/LIME, uncertainty or abstention, URL perturbation, explanation stability, cross-dataset evaluation, and combinations of calibration with explanation or robustness analysis. The project does not claim first use of these components. Its proposed contribution is the bounded empirical comparison of whether per-instance local explanation stability contributes incremental selective-risk value beyond a calibrated-confidence-only policy at matched coverage under in-domain and shifted evaluation. The novelty claim remains provisional until the final search and full-text screening are complete.
