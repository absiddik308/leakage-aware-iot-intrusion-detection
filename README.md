# Leakage-aware evaluation on TON_IoT Windows 10: code and results

Revision materials for MDPI Analytics manuscript analytics-4589312.

## Data
The dataset is not redistributed. Download `Train_Test_Windows_10.csv` from the official TON_IoT page:
https://research.unsw.edu.au/projects/toniot-datasets

## How to reproduce
1. Open `revision_rerun.ipynb` in Google Colab (T4 GPU), upload `Train_Test_Windows_10.csv`, run all cells in order.
   It produces every file in `results/`.
2. Open `revision_checks.ipynb`, upload the CSV and `rerun_results.zip`, run all cells. It produces `checks/`.

## Protocol
Raw file (21,104 x 126) -> text-to-numeric (unparseable cells set to 0) -> MITM removed (21,089) -> per-class split in
record order 70/10/20 (14,759 / 2,109 / 4,221) -> fitted on training rows only: near-constant filter (104), 99th-percentile
caps, correlation filter (78), feature reselection, standard scaling -> 8 models x 3 feature sets x 5 seeds (42-46),
tuning and early stopping on validation only -> single test evaluation.
Exact record IDs per split: `checks/split_record_ids.csv`; held-out-block splits: `checks/heldout_block_record_ids.csv`.

## Environment
Python 3.13.16; numpy 2.1.3, pandas 2.2.3, sklearn 1.6.1, scipy 1.16.3, torch 2.11.0+cu130, xgboost 3.4.1, lightgbm 4.6.0, pytorch_tabnet ?; rtdl_revisiting_models 0.0.2; Google Colab T4 GPU.

## Files
- `checks/C1_training_only_checks.csv`
- `checks/C3_error_rate_by_test_position.csv`
- `checks/C3_gap_predictions.npz`
- `checks/C3_gap_results.csv`
- `checks/C3_selected_features_by_gap.csv`
- `checks/C4_all_test_errors.csv`
- `checks/C4_block_end_stress_test.png`
- `checks/C4_block_middle_vs_end.csv`
- `checks/C4b_block_end_stress_test_noLR.png`
- `checks/C4b_block_middle_vs_end_noLR.csv`
- `checks/C5_main_table_accuracy.csv`
- `checks/C5_main_table_macroF1.csv`
- `checks/C6_block_session_markers.csv`
- `checks/C7_block_bootstrap.csv`
- `checks/heldout_block_record_ids.csv`
- `checks/split_record_ids.csv`
- `data_accounting.csv`
- `feature_sets.json`
- `reselection_report.csv`
- `results/benchmark_results.csv`
- `results/capture_identity.csv`
- `results/confusion_FT_seed42.csv`
- `results/confusion_FT_seed42.png`
- `results/confusion_RF_seed42.csv`
- `results/confusion_RF_seed42.png`
- `results/data_accounting.csv`
- `results/feature_sets.json`
- `results/heldout_block_results.csv`
- `results/per_class_FT_seed42.csv`
- `results/per_class_RF_seed42.csv`
- `results/reselection_report.csv`
- `results/settings.json`
- `results/split_and_cleaning.json`
- `results/split_sizes.csv`
- `results/stats_ft_vs_baselines.csv`
- `results/summary_mean_sd.csv`
- `results/table_acc.csv`
- `results/table_f1.csv`
- `results/test_index.npy`
- `results/test_predictions.npz`
- `results/y_test.npy`
- `settings.json`
- `split_and_cleaning.json`
- `split_sizes.csv`
- `revision_rerun.ipynb (add)`
- `revision_checks.ipynb (add)`