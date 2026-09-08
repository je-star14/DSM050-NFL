# Raw data

`pbp-2025.csv` belongs here. It is the nflverse / nflfastR play-by-play release for the 2025 NFL
season - 48,771 plays across 372 columns, about 115 MB.

It is **not committed**. GitHub rejects files over 100 MB, and the release is public and versioned
upstream, so a copy in this repository would add nothing. The 31-column analysis subset that every
figure is actually built from is committed instead, at
`data/processed/pbp_2025_analysis_subset.csv.gz`.

## Getting it

On Python 3.9 or newer:

```python
import nflreadpy
nflreadpy.load_pbp([2025]).to_pandas().to_csv("data/raw/pbp-2025.csv", index=False)
```

`nflreadpy` does not support Python 3.7, which is the environment this analysis was run in, so the
file was downloaded rather than loaded at run time. The releases are also available directly:

https://github.com/nflverse/nflverse-data/releases/tag/pbp

The notebook looks for the file here first, then next to the notebook, then at
`data/pbp-2025.csv`. If it finds none it prints every location it tried.
