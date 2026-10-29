# Beyond the DCO-OFDM Clipping Ceiling

Clipping-coupled loading and peak-matched probabilistic shaping for LED links.

Digital assignment for BECE306L Digital Communication Systems, VIT (2026–27).
Topic area: advanced modulation formats for high-speed digital communication.
Author: Yugeshwaran P (23BEC1404).

This repository holds the LaTeX source and compiled PDFs for two documents:

| Document | Source | PDF |
|---|---|---|
| Project report (13 pages, IEEE conference format) | [`report/`](report/) | [`report/report.pdf`](report/report.pdf) |
| Literature survey (2 pages, 26 journal references, 2021–2026) | [`literature-survey/`](literature-survey/) | [`literature-survey/literature-survey.pdf`](literature-survey/literature-survey.pdf) |

## What the project is about

LEDs used for visible light communication are linear only over a limited current range.
DC-biased optical OFDM (DCO-OFDM) produces a signal with a high peak-to-average power
ratio, so it has to be clipped to fit that range. Probabilistic constellation shaping
(PCS) sends low-energy symbols more often and normally improves the rate. The question
is how shaping should be designed when clipping, or the LED's peak limit, is the real
constraint.

The base paper is Pham and Ishihara, *Optimization of probabilistic constellation
shaping for optical OFDM systems with clipping distortion* (IEEE Commun. Lett., 2026;
preprint arXiv:2507.07507). It optimizes one shaping distribution for all subcarriers of
a clipped DCO-OFDM link.

The work has three parts:

1. **Re-implementation of the base paper.** As printed, the clipping-noise variance uses
   `1/(2π)` where the standard normal density needs `1/√(2π)`. Against a Monte Carlo
   estimate, the corrected expression agrees to within 0.4 %, while the printed one
   overestimates the clipping noise 6–13 times. With the correction, the PCS gain over
   uniform 16-QAM at 15 dB is 21.1 %, not the reported 72.6 %. With the printed settings,
   the optimizer stops after one iteration in our implementation. It converges once the
   step size is controlled.

2. **CC-EPL (clipping-coupled entropy-and-power loading).** Chooses a Maxwell–Boltzmann
   distribution and a power for each subcarrier, together with the clipping operating
   point. Because clipping couples the subcarriers only through the total signal power,
   the problem reduces to a scalar search plus table look-ups.

3. **PM-PCS (peak-matched PCS).** Moves the shaping to a DFT-spread (single-carrier)
   block of the same bandwidth. Each time sample is then a constellation point, so the
   constellation can be designed to fit the LED range directly and nothing is clipped.

## Main results

All numbers come from the report's tables. Gains are relative, `(new − ref) / ref`.

| Result | Value |
|---|---|
| Base-paper PCS gain over uniform 16-QAM at 15 dB, corrected model | 21.1 % (reported: 72.6 %) |
| CC-EPL vs. Gaussian-input bound of clipped DCO-OFDM (analytical) | 93.2–99.1 % of the bound |
| CC-EPL vs. coupled power loading (analytical, all tested points) | +2.4 to +4.0 % |
| CC-EPL vs. base paper at 15 dB (analytical, three channels) | +5.1 to +22.2 % |
| PM-PCS vs. Gaussian-input bound of clipped DCO-OFDM at 15 dB | +6.0 to +15.0 % |
| PM-PCS + turbo equalizer, LDPC-coded goodput, 12 operating points | highest at all 12 (tied with uniform single-carrier PAM at 6) |
| PM-PCS + turbo vs. CC-EPL, coded, 10–20 dB | +20.7 to +24.6 % |
| PM-PCS vs. CC-EPL through the ngspice circuit (uncoded rate, 10–25 dB) | +12.8 to +13.1 % |

Things to keep in mind when reading these:

- The coded margin of CC-EPL over coupled power loading is small. A repeated run on the
  10 MHz LED gave 1.3–2.6 %, and through the circuit (12 frames per rate) coupled power
  loading came out marginally ahead at 15 dB.
- The "Gaussian-input bound" is the bound for the clipped DCO-OFDM model used here. It is
  not a general capacity bound for the LED channel.
- Everything is simulation. The ngspice model uses a behavioural LED with generic
  parameters, not one fitted to a measured device. A stronger LED nonlinearity than the
  model has would hurt the full-range PM-PCS signal more than clipped OFDM.

## Repository layout

```
report/
  main.tex            report source (IEEEtran)
  refs.bib
  tables/             tables and numeric macros (numbers.tex) used by main.tex
  figures/            figure PDFs used by main.tex
  report.pdf          compiled report
literature-survey/
  main.tex            two-page survey source (IEEEtran, figures drawn in TikZ/pgfplots)
  refs.bib            26 journal references, 2021-2026
  figures/third_party/  empty; for CC BY figures from cited papers (see docs/)
  literature-survey.pdf
docs/
  reference-verification.md     how each survey reference was checked, with confidence
  figure-and-reference-todo.md  what is still open for the survey
```

## Building

Both documents build with pdfLaTeX and BibTeX (TeX Live 2023 or newer, or Overleaf).

```bash
cd report
pdflatex main && bibtex main && pdflatex main && pdflatex main
```

```bash
cd literature-survey
pdflatex main && bibtex main && pdflatex main && pdflatex main
```

On Overleaf: zip the folder, then New Project → Upload Project, with the compiler set to
pdfLaTeX and `main.tex` as the main document.

## What is not in this repository

- **Simulation code.** The figures and tables in `report/` were generated by Python
  scripts (`make_figures.py`, `make_tables.py`, the turbo receiver and the ngspice
  netlists) that are not included here. The tables are committed as generated `.tex`
  files; don't edit them by hand if you have the scripts to regenerate them.
- **Figures from cited papers.** The survey's figure is original. Two slots in
  `literature-survey/main.tex` are ready for figures from open-access (CC BY) papers.
  They appear only once an image file is placed in `figures/third_party/`. See
  `docs/figure-and-reference-todo.md`.

## Status of the literature-survey references

All 26 references were found through web search results (publisher listings, university
repositories, library catalogues, dblp, arXiv). The DOI links and full texts have not
yet been opened and checked one by one. Twenty are marked high confidence. Six have a
detail to confirm (pages, issue, full author list, or the journal version of the base
paper). `docs/reference-verification.md` lists each one and what to check.

## Licence

MIT, see [`LICENSE`](LICENSE). The licence covers the material in this repository.
It does not cover the cited papers or any figures reproduced from them, which stay
under their own licences.

## Course

BECE306L Digital Communication Systems, School of Electronics Engineering,
Vellore Institute of Technology.
