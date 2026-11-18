# Project Handover

## Current status

**As of 18 November 2026:** The project is in research-definition and literature-review stage. Phase 1 problem identification and a Phase 2 literature-review DOCX are present. A prior Codex review and Excel matrix are also present. No source code, model training, experiment output, selected dataset files, web prototype, or implementation plan approved for execution was found in the project root. This handover itself is new context documentation; no existing file has been modified.

The reviewed literature evidence is dated through **6 October 2026** in the Phase 2 DOCX. It is not an exhaustive review current through 18 November 2026. The next Codex session should update the literature before relying on the novelty finding.

**Current research question:**

> For phishing URL classifiers evaluated on in-domain and shifted data, does a local explanation-stability score under semantically valid URL perturbations improve selective risk over a properly calibrated confidence-only policy at matched coverage?

**Current preliminary novelty verdict:** Partially studied. URL-level SHAP stability under controlled perturbations has already been evaluated. Phishing URL uncertainty-based unknown/deferral and calibrated selective prediction with shifted test data also appear in existing work. The remaining possible contribution is a matched-coverage test of the incremental value of local explanation stability over calibrated confidence-only selection. This is provisional, not an absolute novelty claim.

## Existing research source documents

These four research files existed before this handover and were left unchanged:

1. `research/source_documents/phase1.docx` — Phase 1 problem identification and proposed project. Includes the original research question, objectives, broad methodology, tentative datasets/stack, scope, evaluation plan, and formal A/B/C comparison definitions.
2. `research/literature/Capstone_Phase_2_Literature_Review.docx` — newer Phase 2 review (evidence cutoff 6 October 2026), revised gap/question/hypotheses, proposed methodology, abbreviated literature table, and references. It adds Ghalechyan et al. (2024), OACSP-Forensics (2026 SSRN) and Kouser et al. (2026) beyond the earlier workbook.
3. `research/literature/phishing_xai_literature_review.docx` — earlier Codex-generated review with executive summary, top-15 audit, recommendation, novelty risk and search-scope limitations.
4. `research/literature/phishing_xai_literature_matrix.xlsx` — earlier Codex-generated matrix, 17 body rows and 21 columns, with Literature matrix, Categories and Novelty audit sheets. Some rows are category/summary records rather than individual papers, and many entries explicitly need verification.

The persistent project instructions are in the repository-root `AGENTS.md`; the research context and handover are `research/planning/RESEARCH_CONTEXT.md` and `research/planning/HANDOVER.md`. No files were overwritten or deleted.

## Work completed

- Reviewed available conversation history, including the original detailed systematic-review request and its specified searches, extraction fields, novelty audit and requested outputs.
- Inspected the project root and extracted the content of `research/source_documents/phase1.docx`, `research/literature/Capstone_Phase_2_Literature_Review.docx`, and the earlier review DOCX.
- Read all three sheets and the study rows in `research/literature/phishing_xai_literature_matrix.xlsx`.
- Reconciled the two review versions and captured important papers present in Phase 2 but missing from the spreadsheet.
- Recorded the research motivation, exact question, A/B/C comparison, hypotheses, metrics, dataset candidates, unresolved decisions, implementation status and next steps in `research/planning/RESEARCH_CONTEXT.md`.
- Noted reference conflicts requiring correction/verification: Ghalechyan DOI mismatch and Buu & Cho venue mismatch. Also noted that ExpertFusion’s retrieved publication date is in the future relative to this handover and must not be counted as a currently published study without a valid earlier record.
- Established persistent future-session rules in the repository-root `AGENTS.md` against fabricated results/citations and unapproved changes.

## Work not yet started or not confirmed

- No reproducible PRISMA search, logged database queries, deduplication or complete full-text screening.
- No post-6 October 2026 literature refresh.
- No full-text verification of every record in the matrix; several author lists, dataset sizes, methods and calibration/uncertainty definitions remain unknown.
- No final dataset or source snapshots selected; no field/leakage audit run by this capstone.
- No final model set, calibration method, valid perturbation set, primary explanation-stability metric, policy threshold or operational cost function approved.
- No code, model training, experiment, prototype, research-paper implementation or experimental result.
- No mentor/instructor feedback, rubric, deadline or formal deliverable checklist found in the available materials. Phase 1 does state a project/research paper outcome and expected web-based prototype.

## Immediate next task

**Refresh and validate the novelty audit from 7 October 2026 through 18 November 2026 before locking the Phase 2 research question.** Search and screen primary/publisher sources for (a) phishing URL selective prediction and abstention, (b) confidence intervals/uncertainty used to defer URL predictions, (c) calibration plus cross-dataset or temporal shift, (d) explanation stability as a decision signal, and (e) the cited close papers. Include at least the OACSP-Forensics SSRN preprint and find any newer work. Verify the complete source details and full methods, update the matrix in a new version or separate addendum (preserve the existing file), then prepare a concise decision memo showing whether A/B/C and the current exact question remain defensible. Do not train models or alter implementation while doing this literature task.

Also resolve these reference issues from the existing Phase 2 DOCX:

- Ghalechyan et al. DOI should be checked against primary record; available verified record shows `10.1038/s41598-024-74725-6`, whereas Phase 2 currently says `10.1038/s41598-024-77170-7`.
- Buu & Cho DOI-linked record is *Fuzzy Sets and Systems* 517, 109474, DOI `10.1016/j.fss.2025.109474`, whereas Phase 2 lists *Future Generation Computer Systems*.
- ExpertFusion’s indexed date is 15 January 2027, after the handover date. Confirm if any preprint/early-release record existed by 18 November 2026 before treating it as evidence.

## Resume instructions for the new Codex account

1. Open the project folder and read the [repository-root AGENTS.md](../../AGENTS.md), this handover, and `research/planning/RESEARCH_CONTEXT.md` first.
2. Treat the existing DOCX/XLSX files as source material and preserve them unchanged.
3. Continue with the immediate next task above. Keep verified publisher facts separate from claims needing full-text verification.
4. ChatGPT should guide/approve research decisions; Codex may implement only the tasks that have been approved. Do not begin model training or modify implementation as part of the literature refresh.
5. Before using this question in a final proposal or report, update the search and verify the gap. Do not claim absolute novelty.
