# Remaining work: reference checks and figures from cited papers

The Overleaf project is in `literature-survey/`. It currently builds to exactly
**2 pages** with **26 journal references (2021–2026)**. The requirement is a 2-page write-up,
at least 20 references, journal papers only, published 2021 or later.

## Task 1: Check every reference
For each row in `REFERENCE_VERIFICATION.md`:
1. Open the DOI (https://doi.org/<DOI>) or search the title on the publisher's site.
2. Check the title, authors, journal, volume, issue, pages and year against `refs.bib`, and fix any differences.
3. Fill in everything marked missing in the verification sheet (DOIs, pages, full author lists).
4. Note whether the paper is open access and what licence it has.
5. Report honestly: which references were confirmed, which were corrected, and which could not be found.
   If a paper doesn't exist or isn't a journal paper, remove it. If fewer than 20 remain,
   replace it with a confirmed 2021+ journal paper on the same topic.

## Task 2: Add figures from cited papers (only where the licence allows)
- Only reuse a figure if the paper is published under **CC BY** (or another licence allowing reuse)
  **and** you have confirmed that licence on the article page. Likely candidates (not yet confirmed):
  saied2022 (IEEE Access); saied2023, liu2022, park2022, alrakah2024 (Photonics, MDPI); bai2021, guo2026 (Optics Express); deng2022, gao2025 (IEEE Photonics Journal).
- Do not reuse figures from subscription-only papers (most JLT and IEEE Trans. papers).
- The project has two ready slots in `main.tex`, both using the `\paperfig` macro:
  - `figures/third_party/fig_ref5.png`: a figure from saied2022 (e.g. its PAPR comparison).
  - `figures/third_party/fig_alrakah2024.png`: a figure from Alrakah et al., Photonics 2024 (clipping noise in OFDM VLC).
  Save the image under that exact name (PNG), replace the `TODO` caption text with a
  one-line description, and keep the credit line ("Reproduced from [n], CC BY 4.0.", plus the
  figure number from the original paper). You can change which papers the slots use, as long as
  the file name, the `\cite` key and the credit are updated to match.
- A slot does nothing if its file is missing, so the PDF always builds.

## Task 3: Keep it to 2 pages
Adding one 1-column figure pushes the document to 3 pages (tested). To get back to 2:
- delete Table I (`tab:sum`), and/or
- shorten Section IV.
The page is already tight: the reference list uses \scriptsize and reduced spacing.
Rebuild with pdfLaTeX → BibTeX → pdfLaTeX → pdfLaTeX and confirm the PDF has 2 pages.
