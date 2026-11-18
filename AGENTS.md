# Instructions for Codex sessions on this project

## Project objective

The capstone is **Risk-Aware Explainable Phishing URL Detection Using Confidence and Explanation Reliability**. Its current exact research question is:

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

The main goal is to test whether local explanation stability contributes useful information beyond calibrated prediction confidence when deciding which URL predictions may be accepted automatically and which should be deferred for review.

## Preserve the A/B/C distinction

- **A — Conventional classification:** always outputs phishing or legitimate; no abstention.
- **B — Confidence-only selective prediction:** accepts a prediction when calibrated confidence meets a selected policy/threshold; otherwise defers.
- **C — Confidence plus explanation stability:** uses calibrated confidence and local explanation stability; it may defer cases that confidence alone would accept.

B versus C is the principal incremental-value comparison. A is the full-coverage reference. Stability-only can be an ablation, but it does not replace A/B/C. Compare B and C at matched coverage and use held-out/shifted data. Calibration, explanation stability, faithfulness, semantic validity and prediction robustness are different concepts; do not conflate them.

## Research integrity rules

- Never fabricate experimental results, citations, authors, titles, venues, dates, datasets, sample sizes, model details, results, mentor requirements, or research claims.
- Mark missing/unverified details as unknown and verify primary/publisher records before citing them as facts.
- Distinguish literature-reported results from capstone experimental results. No capstone experiments are recorded as complete in the current handover.
- Do not claim absolute novelty. The existing review is a concept-led scoping review through 6 October 2026, not a complete PRISMA search. State novelty as provisional and literature-search-dependent.
- Keep publisher/preprint status clear. OACSP-Forensics is an SSRN preprint in the reviewed record. Do not treat future-dated records as published as of the present date without verification.
- Do not label fuzzy-logic calibration as calibrated probability without evidence of a formal calibration procedure and metrics. Do not treat a Brier score alone as proof a calibrator was fit.
- Use semantically valid URL perturbations and report their validity. Do not call all adversarial changes meaning-preserving.

## Decision ownership and work style

- **ChatGPT guides research decisions; Codex implements approved tasks.** Codex may gather evidence, summarize options, audit files and prepare concrete recommendations. It should not silently decide the final dataset, model set, perturbation protocol, primary stability metric, calibration procedure, operational cost function or change the approved research question.
- Preserve existing work and ask before changing approved research decisions. If the user has already approved a specific change in the conversation, follow that authorization.
- Preserve all source documents and evidence. Do not overwrite or delete existing files. For edits, make a new version or ask first; add new work as new files unless the user explicitly authorizes editing an existing file.
- Do not start model training, collect/transform user datasets, alter the implementation or build the web prototype unless specifically authorized. Current status is research definition; no implementation/model training is recorded.
- No mentor/instructor requirements were recovered in the available handover. Do not invent them; ask the project lead to provide a rubric or instructions when they become necessary.

## Resume from these files

1. Read `HANDOVER.md` for exact status and next action.
2. Read `RESEARCH_CONTEXT.md` for the project background, reviewed findings, hypotheses, methods and unresolved decisions.
3. Inspect existing source documents and matrix before making changes: `phase1.docx`, `Capstone_Phase_2_Literature_Review.docx`, `phishing_xai_literature_review.docx`, `phishing_xai_literature_matrix.xlsx`.
4. Update literature from the 6 October 2026 evidence cutoff through the current date, particularly the possible selective-prediction and confidence/uncertainty studies. Verify primary papers and reconcile citation errors listed in `RESEARCH_CONTEXT.md`.
5. Prepare a decision memo for the project lead before changing the approved research plan or beginning implementation.

