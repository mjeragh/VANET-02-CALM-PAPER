# CALM Paper — IEEE T-IV Submission Packet

Submission portal: https://ieee.atyponrex.com/journal/t-iv (IEEE Atypon ReX)

## Files in this folder

- `cover_letter.tex` / `cover_letter.pdf` — upload `cover_letter.pdf` under the cover-letter field.
- `abstract.txt` — plain-text abstract; paste into the abstract field (~231 words; T-IV limit is 150–250).
- `keywords.txt` — semicolon-separated keywords; paste or enter one per line per portal UI.
- `author_info.txt` — author info, ORCIDs, and declarations to fill on the portal.

## Paper files (in repo root, not here)

- `main.pdf` — compiled manuscript, 10 pages, IEEEtran journal class.
- `main.tex`, `references.bib`, `figures/` — source for production upload if the portal asks for it.

## Portal checklist (IEEE Atypon ReX)

The Atypon ReX wizard walks you through each step; the high-level flow is:

1. Log in / create an IEEE account at the portal URL above.
2. Start a new submission for *IEEE Transactions on Intelligent Vehicles*; paper type = Regular Paper.
3. Title & Abstract: paste from `main.tex` title and `abstract.txt`.
4. Keywords: paste from `keywords.txt`.
5. Authors & Affiliations: enter Mohammad (corresponding) and Ebrahim from `author_info.txt`. Atypon ReX matches affiliations against the Ringgold database.
6. Suggested reviewers: optional for T-IV; skip or supply 3–5 names.
7. Funding / conflicts: none; declarations as in `author_info.txt`.
8. Upload files: `main.pdf` (required); if source is requested, also `main.tex`, `references.bib`, `figures/*`, and `main.bbl`. Upload `cover_letter.pdf` under the cover-letter field.
9. Review the system-generated proof, then submit.

## Suggested reviewers (to be decided by authors)

Provide 3–5 names with affiliation and email. Pick people who work on: MARL for driving, imitation learning for AVs, cooperative/connected vehicles, or NGSIM-based behavior modeling. Avoid co-authors, recent collaborators, and same-institution colleagues. Pull candidates from the paper's reference list (e.g., authors of `yu2022surprising`, `shou2020multi`, `chen2020cooperative`, `kuefler2017imitating`, `bhattacharyya2022modeling`) and from recent T-IV papers on cooperative driving.
