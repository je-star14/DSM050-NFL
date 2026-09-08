# Predictable by Design? Play-Calling Tendency and Offensive Efficiency in the 2025 NFL Season

DSM050 Data Visualisation - final coursework. MSc Data Science, University of London.

## Question

How much of NFL play-calling is philosophy rather than circumstance, and does being harder to
predict actually make an offence better?

## Data

nflverse / nflfastR play-by-play release, 2025 NFL season - 48,771 plays across 372 columns,
covering all 285 games.

The notebook reads a local copy of the release. It looks for `data/raw/pbp-2025.csv`, then
`pbp-2025.csv`, then `data/pbp-2025.csv`, so the CSV can simply sit next to the notebook if you
prefer. If none is found it prints where it looked and the working directory. It then writes the
31-column analysis subset to `data/processed/pbp_2025_analysis_subset.csv.gz` (about 1.5 MB), which
is what is committed and what the report links to. The full season CSV is 115 MB and would exceed
GitHub's 100 MB file limit, so it is not committed.

On Python 3.9 or newer the season file can be regenerated from source:

```python
import nflreadpy
nflreadpy.load_pbp([2025]).to_pandas().to_csv("data/raw/pbp-2025.csv", index=False)
```

`nflreadpy` does not support Python 3.7, which is why the committed copy is the documented path.

Upstream: https://github.com/nflverse/nflverse-data · https://nflreadpy.nflverse.com

## Reproducing

```bash
pip install -r requirements.txt
jupyter notebook DSM050Final_Coursework_V1.ipynb
```

Put the season CSV either at `data/raw/pbp-2025.csv` or next to the notebook, then run all cells.
No figure is pre-rendered; every one is generated from the data.

### Environment

Verified against two stacks. The first code cell prints which one it is running on.

| | tested |
|---|---|
| Python 3.7.3, pandas 1.3.5, numpy 1.21.6, matplotlib 3.5.3, sklearn 1.0.2 | target environment, pinned in `requirements.txt` |
| Python 3.11, pandas 3.0, numpy 2.4, matplotlib 3.10, sklearn 1.8 | development |

A `UserWarning` about `numexpr` 2.6.9 is harmless and changes no result. To silence it:
`pip install --user "numexpr>=2.7.0"`.

`requirements.txt` pins the versions used. To run on a newer Python instead, drop the `==` pins;
nothing in the notebook depends on a specific release.

## Structure

```
├── DSM050_Final_Report_V1.docx     # the report, for editing and PDF export
├── DSM050_Final_Report_V1.pdf      # submitted PDF
├── DSM050Final_Coursework_V1.ipynb # notebook: same commentary plus all the code
├── data/
│   ├── raw/README.md               # download instructions
│   └── processed/                  # gzipped analysis subset
├── figures/                        # exported figures, numbered
└── requirements.txt
```

## Methods

- Situational entropy (Shannon binary entropy of pass rate within cells matched on down, distance,
  lead state, field position, formation and game half) as a predictability measure that controls
  for game state - 79.4% of qualifying plays fall in a cell of ten or more
- The fall in entropy when formation is added to the conditioning set, used as a direct measure of
  how much a team's alignment gives away (correlates +0.84 with the raw shotgun / under-centre gap)
- A specification curve over the conditioning set, to show which findings survive rebanding and
  which do not
- Bootstrap confidence intervals on every team-level comparison and every correlation
- PCA over nine standardised tendency features, with silhouette validation used to test, and
  reject, the existence of stylistic archetypes

## Before submitting

- [ ] Put the season CSV next to the notebook, or at `data/raw/pbp-2025.csv`, before running
- [ ] Commit `data/processed/pbp_2025_analysis_subset.csv.gz` - the brief requires a copy of the
      data in the repository
- [ ] Run the word count cell in **classic Jupyter Notebook 6** - the JS uses the `Jupyter.notebook`
      API, which neither JupyterLab nor Notebook 7 provides, so it fails silently on both
- [ ] State the word count at the end of the report (5 marks lost if omitted) - currently 3,268
- [ ] Check the Appendix A dates against your own session history before submitting
- [ ] Insert the GitHub repository link in the title cell (still reads `[insert your GitHub link here]`)
- [ ] Add the missing space after each number in the References list (`1.Baldwin` -> `1. Baldwin`)
- [ ] Edit `DSM050_Final_Report_V1.docx` in Word, then export to PDF from Word (not LibreOffice)
- [ ] Re-check the word count in Word after editing and update the stated figure
- [ ] Confirm every figure rendered and none is split across a page break
