# Leakage-aware evaluation on TON_IoT Windows 10: code and results

Revision materials for MDPI Analytics manuscript analytics-4589312.

## Data
The dataset is not redistributed. Download `Train_Test_Windows_10.csv` from the official TON_IoT page:
https://research.unsw.edu.au/projects/toniot-datasets

## How to reproduce
1. Open `revision_rerun.ipynb` in Google Colab (T4 GPU), upload `Train_Test_Windows_10.csv`, and run all cells in order.
   It produces the main results (Group 2 below) and downloads them as `rerun_results.zip`.
2. Open `revision_checks.ipynb`, upload the CSV and `rerun_results.zip`, and run all cells.
   It produces the verification checks (Group 3 below).

## Protocol
Raw file (21,104 x 126) -> text-to-numeric (unparseable cells set to 0) -> MITM removed (21,089) ->
per-class split in record order 70/10/20 (14,759 / 2,109 / 4,221) -> fitted on training rows only:
near-constant filter (104), 99th-percentile caps, correlation filter (78), feature reselection, standard scaling ->
8 models x 3 feature sets x 5 seeds (42-46), tuning and early stopping on validation only -> single test evaluation.

## Environment
Python 3.13.16; numpy 2.1.3, pandas 2.2.3, scikit-learn 1.6.1, scipy 1.16.3, torch 2.11.0+cu130, xgboost 3.4.1,
lightgbm 4.6.0, pytorch-tabnet 4.1.0, rtdl_revisiting_models 0.0.2; Google Colab T4 GPU.

## Files

### 1. Notebooks, settings and splits
- `revision_rerun.ipynb`: main experiment (8 models x 3 feature sets x 5 seeds, held-out-block test)
- `revision_checks.ipynb`: verification checks C1-C7
- `settings.json`: seeds, split fractions, thresholds, training settings
- `split_and_cleaning.json`: train/validation/test record indices, removed columns, eligible features
- `split_record_ids.csv`: partition, capture block and position of every record (0-based row in the raw CSV)
- `heldout_block_record_ids.csv`: train/validation/test records for each held-out-block scenario
- `split_sizes.csv`, `data_accounting.csv`: per-class split sizes and record/feature counts at each step
- `feature_sets.json`, `reselection_report.csv`: the three feature sets and the training-only selection

### 2. Main results (`revision_rerun.ipynb`)
- `benchmark_results.csv`: every run (model, feature set, seed, validation and test scores, time)
- `summary_mean_sd.csv`, `table_acc.csv`, `table_f1.csv`: mean +/- SD over seeds
- `stats_ft_vs_baselines.csv`: FT-Transformer vs RF / XGBoost / LightGBM (McNemar, bootstrap)
- `confusion_FT_seed42.csv/.png`, `confusion_RF_seed42.csv/.png`, `per_class_FT_seed42.csv`, `per_class_RF_seed42.csv`
- `capture_identity.csv`: capture-block separability of each feature
- `heldout_block_results.csv`: held-out capture block test
- `test_predictions.npz`, `test_index.npy`, `y_test.npy`: all test predictions and labels

### 3. Verification checks (`revision_checks.ipynb`)
- `C1_training_only_checks.csv`: counts and training-only fitting (tamper test)
- `C3_gap_results.csv`, `C3_gap_predictions.npz`, `C3_selected_features_by_gap.csv`, `C3_error_rate_by_test_position.csv`: boundary-gap test
- `C4_all_test_errors.csv`, `C4_block_middle_vs_end.csv`, `C4_block_end_stress_test.png`,
  `C4b_block_middle_vs_end_noLR.csv`, `C4b_block_end_stress_test_noLR.png`: errors near block ends (stress test)
- `C5_main_table_accuracy.csv`, `C5_main_table_macroF1.csv`: training-only feature set as the main result
- `C6_block_session_markers.csv`: session-information check
- `C7_block_bootstrap.csv`: block bootstrap intervals
