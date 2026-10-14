# Image encodings for volatility-regime classification

Code and result files for:

> Goodarzi, N. *Do Image Encodings Improve Out-of-Sample Volatility-Regime Classification? Normalization, Capacity and Benchmarks.* Working paper, 2026.

Archived version: https://doi.org/10.5281/zenodo.23352045

## What the study does

Four equity indices (S&P 500, EuroStoxx 50, KOSPI, Shanghai Composite), daily data 2010 to June 2026, 48 annual walk-forward test folds (2015 to 2026). The target is the tercile of Yang-Zhang volatility over the next 20 trading days. All inputs end on the day before the prediction date, and training samples whose label window reaches the test year are purged. Gramian angular field and wavelet scalogram images (standard per-window scaling and fixed pre-2015 scaling) are classified with a pretrained ResNet18 and a small CNN, and compared with persistence, HAR-RV, a random forest and a temporal CNN.

## Contents

| Path | Content |
|---|---|
| `volatility_forecasting.ipynb` | Full pipeline: data download, labels, folds, images, models, tests, post-hoc and revision analyses |
| `requirements.txt` | Package versions used |
| `results/` | Result tables written by the notebook |
| `audit/fold_audit_*.csv` | Per market and fold: training size, purged samples, last training label date, first test information date |

## Paper tables and result files

| Paper | File(s) in `results/` |
|---|---|
| Table 1, Appendix A | `task1_seeds.csv`, `main_seed_averaged_tests.csv` |
| Table 2 | `level_preservation_tests.csv` |
| Section 4.3 (complete years, class-balanced folds) | `main_complete_years_tests.csv`, `main_class_sensitivity.csv` |
| Table 3 (stress class) | `stress_by_fold.csv`, `stress_tests_f1.csv`, `stress_tests_rec.csv` |
| Table 4 (training and test F1) | `task1_seeds.csv` (`train_f1`, `img_f1`) |
| Table 5 (probes) | `task3_probing.csv` |
| Table 6 (image embedding added to HAR) | `task4_stack.csv` |
| Table 7 (volatility-series scalograms) | `task2_volscal.csv`, `volscal_tests.csv`, `vol_vs_return_paired.csv` |
| Section 4.7 (QWK, volatility targeting) | `posthoc_qwk_tests.csv`, `posthoc_risk_summary.csv`, `posthoc_risk_by_market.csv` |
| Table 8 (close-to-close baselines) | `closeclose_tests.csv`, `calibration_control.csv` |
| Table 9 (intervals) | `equivalence_ci.csv`, `equivalence_ci_unrounded.csv` |
| Table 10 (chance level) | `chance_skill.csv`, `chance_circular.csv` |
| Optimizer control | `task5_lrcontrol.csv`, `lrcontrol_vs_original.csv` |
| Appendix B (VaR / ES) | `posthoc_risk_by_market.csv` |

## Reproducing

1. Create an environment with Python 3 and `pip install -r requirements.txt`.
2. Open `volatility_forecasting.ipynb` and run the cells in order. Flags at the top (`RUN_TASK_SEEDS`, `RUN_TASK_VOLSCAL`, `RUN_TASK_PROBING`, `RUN_TASK_STACK`, `RUN_POSTHOC`) switch each task on or off. `SYNTHETIC_TEST = True` runs a small end-to-end check without internet or GPU.
3. Prices are downloaded from Yahoo Finance at run time and are not included here. The notebook stores a hash of the downloaded data in `manifest.json` and binds result folders to it, so a later download with revised prices writes to a new folder.
4. Seeds are 42, 1337 and 2024. CUDA results are not bitwise reproducible, so retrained networks can differ slightly from the stored results; all tables can be recomputed from the stored files without retraining.

## License

MIT (see `LICENSE`).
