# Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability

## Project overview

This final-year capstone studies whether the reliability of local explanations can help decide when a phishing URL classifier should make an automatic prediction and when it should defer the case for review.

**Research question**

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

The project is in the research-definition and literature-audit stage. No capstone model training, data analysis, or experimental results are recorded as complete. This repository preserves the research documents and provides a small workspace for approved future implementation.

## Experimental comparison

- **A — Conventional classification:** predicts phishing or legitimate for every evaluated URL.
- **B — Confidence-only selective prediction:** accepts predictions based on calibrated confidence and defers the rest.
- **C — Confidence plus explanation stability:** combines calibrated confidence with local SHAP explanation stability and may defer cases that B accepts.

The main incremental-value comparison is B versus C at matched coverage. A is the full-coverage reference. Calibration, explanation stability, explanation faithfulness, perturbation validity, and prediction robustness are distinct quantities and will be reported separately.

## Proposed dataset strategy

PhiUSIIL (UCI dataset 967) is the proposed in-domain dataset, conditional on manual inspection of URL fields, label provenance, duplicates, source composition, and leakage risks. It will be used for URL-only features only if that audit supports the choice.

A time-bounded PhishTank cohort is the proposed shifted evaluation source, but it is not finalized. PhishTank contributes phishing URLs; independently verified benign controls and a defensible sampling procedure are still needed. Dataset access, labels, timestamps, licenses, overlap, and suitability must be audited before use. No dataset is bundled in this repository.

## Proposed methodology and tools

The research plan proposes URL-only feature extraction, leakage-aware data partitioning, a calibrated classifier, local SHAP explanations, and explanation-stability scoring under semantically valid URL perturbations. Candidate tools include Python, pandas, scikit-learn, XGBoost, SHAP, Jupyter, and standard plotting libraries. The specific model set, calibration method, perturbation protocol, primary stability metric, data split, and operating costs remain subject to the documented audit and mentor approval; this README does not make those decisions final.

## Current status

- Literature review, research context, methodology proposal, dataset/model feasibility audit, and analysis plan are documented.
- Dataset selection and manual data audits are pending.
- The implementation pipeline and prototype have not been started.
- Model training and capstone experiments have not been conducted.
- There are no empirical results or final conclusions.

## Planned development stages

1. Obtain mentor approval for unresolved research and evaluation decisions.
2. Inspect candidate datasets, provenance, licensing, labels, timestamps, duplicates, and domain/campaign leakage; document the selection before using data.
3. Freeze the URL-only feature definition and leakage-aware train, calibration, validation, and test partitions.
4. Implement and validate the full-coverage classifier and confidence-only policy; fit calibration without using the locked test data.
5. Define and manually validate meaning-preserving URL perturbations, calculate local explanation-stability scores, and implement policy C.
6. Evaluate in-domain and shifted cohorts, compare B and C at matched coverage, quantify uncertainty statistically, and report limitations and failure cases.

No stage should be treated as complete until its artifacts and evidence are recorded.

## Environment setup when implementation begins

Use a supported Python 3 version selected by the team, create an isolated virtual environment, and install only the pinned dependencies approved for the implementation. For example, on Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

A `requirements.txt` should be added after the team and mentor approve the implementation dependencies and versions. Do not install or download datasets as part of this setup guidance.

## Research integrity and limitations

The literature review is a targeted scoping review with a documented evidence cutoff and refresh, not a claim of exhaustive systematic coverage. The novelty assessment is provisional and search-dependent. Dataset quality, label provenance, external controls, distribution shift, semantic validity of perturbations, and explanation-stability interpretation are material limitations. Explanation stability is not evidence of faithfulness, causal relevance, model confidence, or robustness by itself. All empirical results remain pending.

## Documentation

The four files below are **preliminary research drafts**, not final experimental results. No capstone experiments or model training have been completed.

### Preliminary research drafts

- [First research paper draft](research/deliverables/FIRST_RESEARCH_PAPER_DRAFT.md)
- [Proposed methodology](research/deliverables/METHODOLOGY.md)
- [Result analysis plan](research/deliverables/RESULT_ANALYSIS.md)
- [Provisional conclusion](research/deliverables/PROVISIONAL_CONCLUSION.md)

### Literature review and evidence

- [Phase 2 literature review](research/literature/Capstone_Phase_2_Literature_Review.docx)
- [Phishing XAI literature review source](research/literature/phishing_xai_literature_review.docx)
- [Phishing XAI literature matrix](research/literature/phishing_xai_literature_matrix.xlsx)
- [Literature update for 7 October–18 November 2026](research/literature/Literature_Update_Oct07-Nov18_2026.md)
- [Patel et al. full-paper audit](research/literature/PATEL_IJAICET_FULL_PAPER_AUDIT.md)

### Planning, decisions, and handover

- [Dataset and model selection audit](research/planning/DATASET_AND_MODEL_SELECTION_AUDIT.md)
- [Final research direction and proposed methodology](research/planning/FINAL_RESEARCH_DIRECTION_AND_METHODOLOGY.md)
- [Updated decision memo](research/planning/Updated_Decision_Memo_2026-11-18.md)
- [Research context](research/planning/RESEARCH_CONTEXT.md)
- [Project handover](research/planning/HANDOVER.md)
- [Codex project instructions](AGENTS.md)

### Original phase documents

- [Original Phase 1 document](research/source_documents/phase1.docx)
- [Original Phase 2 literature review](research/literature/Capstone_Phase_2_Literature_Review.docx)

The original Word and Excel research sources are retained. Markdown remains the preferred maintained format for editable research notes and deliverables. Converted Word copies of the four preliminary drafts are retained locally under `research/deliverables/` but remain excluded from Git as duplicate convenience formats.
