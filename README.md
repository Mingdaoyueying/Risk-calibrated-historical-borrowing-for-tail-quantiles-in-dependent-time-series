[README.md](https://github.com/user-attachments/files/33423326/README.md)
# Risk-calibrated historical borrowing for tail quantiles: code and data

Version 1.1.0 · 13 October 2026

Compact release below 20 MB: complete model code, frozen settings, prepared
financial data, complete main-design financial arrays, all summary tables,
all figures, and small numerical validation references. Original provider files,
bulk secondary financial and simulation arrays, and the duplicate daily CSV
export are omitted; full reproduction commands regenerate them.

## Contents

| Path | Contents |
|---|---|
| `code/` | Models, backtests, simulation generators, plotting, export, and validation |
| `data/` | Frozen value-weighted daily returns for 12 industries, 2000–2025 |
| `docs/` | Configurations, seeds, provenance, hashes, data definitions, and audits |
| `results/financial/main/` | Complete forecasts and diagnostics for all 12 industries, 2,766 dates each, two quantiles, and 15 methods |
| `results/reference/` | Numerical references: nine simulation replications and four additional financial origins |
| `tables/` | All 12 summary CSVs covering nine financial designs and all 174,000 simulation sequences |
| `figures/` | All nine reference plots |
| `reproduce.py` | Command-line reproduction entry point |
| `requirements.txt` | Pinned Python dependencies |
| `SHA256SUMS.txt` | Checksums for every included file |

The financial sample contains 6,539 dates; evaluation uses 2,766 dates per
industry. Returns are in percent: **1.00 means 1%**. Both the original
eight-method and extended 15-method BH comparisons are retained.

## Installation and reproduction

Use Python 3.11 or later; tested with Python 3.12.14. Run from this directory:

```bash
python -m pip install -r requirements.txt
python reproduce.py --stage verify
python reproduce.py --stage smoke --output-dir reruns/smoke
python reproduce.py --stage summarize --output-dir reruns/summaries
python reproduce.py --stage all --output-dir reruns/full --workers 2 --resume
```

The included prepared sample supports all default stages offline. `verify`
checks retained arrays, hashes, summary coverage, and images. `smoke` recomputes
nine simulation replications and seven financial origins against retained
references. `summarize` recreates figures and overview tables from the included
main arrays and published summaries. `all` regenerates every omitted output
and independently recomputes all summaries; full reruns exceed 20 MB. Separate
`financial`, `simulation`, and `precision` stages are also available. Reruns
use separate directories.

## Original data downloads

Source: Kenneth French Data Library, 12 Industry Portfolios.

- [Daily returns CSV ZIP](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/12_Industry_Portfolios_daily_CSV.zip)
- [Dataset description](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library/det_12_ind_port.html)
- [Industry SIC definitions ZIP](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/Siccodes12.zip)

To rebuild from the original official archive or request a live download:

```bash
python reproduce.py --stage prepare --input-zip /path/to/12_Industry_Portfolios_daily_CSV.zip --output-dir reruns/prepared
python reproduce.py --stage prepare --download-raw --output-dir reruns/downloaded
```

Both routes require the frozen CRSP vintage **202608** and stored source hashes.
The provider may revise historical observations; the bundled prepared sample
preserves the exact data used for all included results.

Simulation observation vectors can be regenerated from the retained seeds:

```bash
python code/export_simulation_data.py --family core1000 --scenario recur_scale50 --n-recent 250 --phi 0.6 --count 3 --out reruns/observations
```

Creators: Yangxin Wang, Yixuan Zeng, Yuxuan Zhang, and Bo Huang.
Corresponding author: Bo Huang, `13548030232@163.com`.
Citation and rights: `CITATION.cff`, `ZENODO_METADATA.txt`, and `RIGHTS.md`.
