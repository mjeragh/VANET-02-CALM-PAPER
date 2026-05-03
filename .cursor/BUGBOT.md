# Bugbot Review Rules — VANET-02 CALM Paper (IEEE T-IV, in review)

## Project context

LaTeX source for the **CALM** paper, submitted to IEEE Transactions on Intelligent Vehicles (manuscript **T-IV-26-04-0319**, submitted 2026-04-25). Editorial assistant: Ayush Sharma <tiv-eic@ieee.org>. Class: `IEEEtran`. The submitted version is in active editorial review — this repo holds the working copy for any minor revisions, the resubmission, and the camera-ready cycle.

Authors: Dr. Mohammad Jeragh (corresponding), Dr. Ibrahim Alrashed (Kuwait University, Computer Engineering Dept).

Companion code repo: `mjeragh/VANET` — all numerical results, figures, and statistical analyses originate there.

## Review priorities — flag these aggressively

### Submission-state preservation (highest priority)

- **The paper is in review.** Flag any change that alters claims, theorems, numbers, or proofs without a stated review-response justification in the PR description.
- Flag substantive structural edits (renaming sections, reordering theorems, dropping references) — these are not "minor edits" and risk diverging from the submitted record.
- Author list, affiliations, and `\thanks{}` footnote text are locked to the submitted version. Flag any edit unless the PR cites a T-IV editorial request.
- The `references.bib` has 48 entries that match the submitted record. Flag entry deletions; flag silent renaming of existing keys (downstream `\cite{...}` will rot).

### Paper-claim ↔ source-data consistency

- Numerical claims in `main.tex` (NGSIM agreement %, action entropy, reward values, p-values, run counts, scalability numbers) must match the underlying data files in the `mjeragh/VANET` companion repo (`MyScenarios/AnyScenario/Documentation/WEEK*_*.md`, `outputs/`, ablation JSONs).
- Flag any change to a cited statistic without evidence the underlying data file was regenerated.
- Reference numbers to verify on every PR: DAgger 45.3% ± 0.9%, QMIX-CALM 40.1% ± 2.3%, CALM (MADDPG+BC) 34.0% ± 1.2%, CALM reward 1.35, action entropies in the 0.06–0.88 range, 25 runs (5 methods × 5 seeds), Welch's t-test, p < 0.001 vs all others (DAgger).
- Flag claim changes that contradict the theoretical results (Theorem 1 KL bound, Proposition 1 entropy bound via Pinsker, Proposition 2 asymptotic convergence).

### Bibtex hygiene

- Every `\cite{key}` in `main.tex` must resolve to an entry in `references.bib`. Flag dangling cites.
- Every entry in `references.bib` must have the required fields for its `@type` (T-IV is strict on `@article` needing journal+volume+year, `@inproceedings` needing booktitle+year, `@misc` needing howpublished+year). The Week 8 polish round fixed 4 entries; don't regress.
- Flag duplicate `@key{...}` entries.
- Flag entries that mix BibTeX and BibLaTeX field names (`date` vs `year`+`month`).

### IEEEtran style compliance

- Use `\section{}`, `\subsection{}`, `\subsubsection{}` — not `\section*{}` (breaks IEEE TOC + counter logic).
- Tables use `[!t]` or `[!tb]` placement (IEEE house style) — flag `[H]`, `[h]`, or `[h!]`.
- Figures use `\includegraphics[width=\columnwidth]` or `\textwidth` for spans — flag absolute `width=Xcm`.
- Math: use `\mathbf` / `\bm` consistently; use `\eqref{}` for equation refs (not `eq.~(\ref{})`).
- IEEEbiography blocks at end-of-file: do not delete; flag content edits unless tied to author-info changes.
- No `microtype` warnings as code review issues — that's a build-time concern.
- Author notation must stay consistent: `π_BC`, `π_t`, `D_KL`, `λ(t)`. Flag drift.

### File and figure hygiene

- Every `\includegraphics{figures/...}` path must exist in the repo. Flag broken figure paths.
- Don't commit `*.aux`, `*.log`, `*.out`, `*.bbl`, `*.synctex.gz` — those should be gitignored. Flag accidental check-ins.
- The Week-5 figures (`figures/week5_*.pdf`) are the load-bearing visual results. Flag replacements without evidence the underlying data was rerun.

### Compile cleanliness

- The Week 8 polish landed 0 errors / 0 warnings. Flag PRs that introduce new LaTeX warnings or overfull/underfull boxes without justification.
- Don't switch between `\usepackage{amsmath}` orderings or remove `amsthm` — proof environments depend on it.

## What NOT to flag

- Spelling/grammar nits in prose — author voice is intentional.
- US vs UK spelling preferences — leave it alone.
- Hyphenation, microtypography, line-break adjustments, ragged-right vs justified — IEEEtran handles that.
- `\,` / `\;` / `\:` / `\!` spacing tweaks in math — those are author calibration.
- Comments in `.tex` (`% ...`) — they're author notes.
- `.gitignore`, `.DS_Store`, editor swap files — the existing `.gitignore` is canonical.
- Reordering of `\bibliography{...}` style options — cosmetic.
- Italicization of theorem names, abbreviations like "i.e.", "e.g.", "et al." — IEEE style is stable here.

## When in doubt

- Sibling repo `mjeragh/VANET` is the source of truth for any numerical claim — refer reviewers there.
- The CoEx companion paper (`mjeragh/coex-journal-paper`, JITS / Taylor & Francis `interact` class) shares some background material but uses a different style; don't suggest cross-paper consistency edits.
- For T-IV editorial-correspondence questions, flag and tag the corresponding author rather than suggesting blind edits.
