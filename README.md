# DAF Research dashboard (public mirror)

A static research dashboard for Donor-Advised Fund (DAF) prospecting.
Deployed on GitHub Pages from the root of this repo.

Three pages:

- `/daf/` &mdash; DAF Sponsor Explorer (NPT 2025 Annual DAF Report; 1,594 sponsors, 2019-2024)
- `/charities/` &mdash; IRS Exempt Organizations Business Master File, filtered to ~697k orgs with reported revenue or assets (plus all DAF sponsors)
- `/metrics/` &mdash; analyst view computing payout rate, average grant per account, net flow, and velocity per sponsor

## What's in this repo

- `index.html` &mdash; redirects to `/daf/`
- `daf/`, `charities/`, `metrics/` &mdash; the three pages
- `data/bootstrap.json`, `data/daf_history.json`, `data/states/*.json` &mdash; the per-state shards the Charities page fetches at runtime
- `.nojekyll` &mdash; tells GitHub Pages to serve files verbatim (no Jekyll processing)

Everything is static. There is no backend, no database, no build step. `python3 -m http.server` from the repo root reproduces the deployed site exactly.

## Source code and data pipeline

The build scripts (`build_explorer.py`, `build_irs_bmf.py`, `build_metrics.py`, `build_shards.py`), the SQLite database, and the raw IRS BMF CSVs live in a separate repo that is not deployed. This repo only contains the generated public artifacts.

## Deploying

Settings -> Pages:

- **Source:** Deploy from a branch
- **Branch:** `main`
- **Folder:** `/ (root)`

First push wins. No CI, no Actions, no Jekyll.

## Data caveats

- DAF figures are nominal USD as reported on Form 990 Schedule D. Not inflation-adjusted.
- The IRS BMF is pre-filtered: rows where both `REVENUE_AMT` and `ASSET_AMT` are zero or null are dropped, with a carve-out that always keeps EINs appearing in the DAF dataset.
- DAF EIN match rate against the BMF is 98.6% (1,571 of 1,594).

## Sources

- National Philanthropic Trust, [2025 Annual DAF Report Updated Analysis Dataset](https://www.nptrust.org/reports/daf-report/)
- IRS, [Exempt Organizations Business Master File Extract (per-state)](https://www.irs.gov/charities-non-profits/exempt-organizations-business-master-file-extract-eo-bmf)
