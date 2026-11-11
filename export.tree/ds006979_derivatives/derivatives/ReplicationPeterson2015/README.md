# EEG/ERP Processing Pipeline Summary

Ramzaoui & Beck — Registered Report, Stage 2 · Updated November 2026 (OpenNeuro ds006979, version 1.1.5)

This document describes the complete pipeline of the study, organized by folder and processing phase. The pipeline converts raw EEG data into the ERP measures of the registered analyses (N2pc, CDA, N2, FN400) and includes three exploratory analyses: early CDA, negative slow wave (NSW) and alpha power.

All ERP analyses use correct-response trials only, as specified in the Stage 1 protocol. Stimulus-locked analyses (N2pc, CDA, early CDA, NSW, alpha) use the bin descriptor `BDF_stimulus_correct.txt`, and their outputs carry the suffix `_correct`. Probe-locked analyses (N2pc, N2, FN400) use the bin descriptor `BDF_probe.txt`, which also selects correct-response trials.

## Top-level directory structure

- **EEG_ERP_Processing**: MATLAB Scripts 1–8 (Phases I–III) and the TXT/XLSX files they need (bin descriptors, channel and bin operations, re-referencing lists, IC removal lists, artifact-rejection parameters, subject lists). Subfolders:
  - **Grand_Average_ERPs**: Scripts 9–10 and the manuscript-figure scripts.
  - **ERP_Measurements**: Script 11 and the measurement output files.
- **ERP_Cluster_Analyses**: cluster-based permutation tests defining the ERP time windows (Phase IV).
- **Exploratory_Analyses**: NSW and alpha-power scripts and their outputs (Phase V).
- **Data_Analysis_Scripts_R**: R statistical scripts (Phase VI), measurement files and MATLAB CSV exports.
- **Individual subject folders** (e.g., `1`, `76`; `sub-01`, `sub-76` in the OpenNeuro derivatives): raw and processed data files and individual figures.
- **Get_BDF_Timestamps**: script extracting the BDF timestamps (`BDF_Timestamp_Verification.m`).
- **script0**: conversion of the BDF data into the EEGLAB datasets used for preprocessing (`Script0.m`). This conversion is already done (`1.set`, `2.set`, …); preprocessing starts with Script 1.

Files in the main directory:

- **Laboratory_log_VWM-PerceptualGrouping_2024-2025.pdf**: project metadata (title, PIs, data collection dates, location, equipment: BioSemi ActiveTwo, 64 electrodes).
- **allERPs_Subject_Rejection_List.xlsx**: participant-specific exclusion flags and artifact parameters (Phase II).
- **allERPs_Participant_Demographics.xlsx**: demographics of the final sample (N = 53) and inclusion in the stimulus-locked analyses (N = 39).
- **allERPs_Event_Code_Scheme.xlsx**: definition of the event codes (Phase II).

The raw data in BIDS format are the OpenNeuro dataset itself (ds006979).

## Phase I: Pre-processing and artifact correction

### Script 1: `Script1_Import_Raw_EEG_DS_Reref_Hpfilt.m`

- Loads the EEG data and downsamples to 256 Hz.
- Mild high-pass filter (0.01 Hz) and DC offset removal.
- Channel locations (3-D coordinates, 10-10 system).
- Re-reference to the left mastoid and creation of uncorrected bipolar channels (VEOG, HEOG).

### Scripts 2–3: ICA preparation and computation

- Script 2 (`Script2_artifact_correction_phase1.m`): downsampling to 100 Hz, 1 Hz high-pass, removal of breaks between blocks and of continuous segments with large artifacts.
- Script 3 (`Script3_artifact_correction_phase2.m`): ICA weights and sphere matrices on this optimized dataset (saved as `<id>_allERPs_optimized_weights.set`).

### Script 4: `Script4_artifact_correction_phase3.m`

- The ICA solution is applied to the 256 Hz data; manually identified artifactual components (`ICs_to_Remove.xlsx`) are removed.
- Re-reference to the average of the left and right mastoids.
- Spherical-spline interpolation of bad channels (`interpolate.xlsx`).

## Phase II: Epoching and artifact rejection

### Script 5: `Script5_Elist_Bin_Epoch.m`

- Event list and bin assignment with ERPLAB Binlister:
  - stimulus-locked: `BDF_stimulus_correct.txt` → `<id>_StimulusERPs_Eventlist_Bins_correct.txt`;
  - probe-locked: `BDF_probe.txt` → `<id>_ProbeERPs_Eventlist_Bins.txt`.
- Epochs with baseline correction (−200 to 0 ms): stimulus −200 to 1000 ms (`…_epoch_correct.set`), probe −200 to 800 ms (`…_epoch.set`).

### Script 6: artifact rejection (6a, 6b, 6c)

- Participant- and ERP-specific parameters (`AR_*.xlsx`): blink and saccade detection (step function), simple voltage threshold, moving-window peak-to-peak amplitude.
- Outputs: 6a stimulus-locked N2pc/CDA (`_correct`), 6b probe-locked N2pc, 6c probe-locked N2/FN400.

## Phase III: Individual ERPs

### Script 7: averaging, difference waves, filtering (7a, 7b, 7c)

- Averaged ERPs (ERPsets) and difference waves with bin operations:
  - 7a: contralateral − ipsilateral (stimulus-locked N2pc, CDA; `_correct`);
  - 7b: contralateral − ipsilateral of change − no-change (probe-locked N2pc);
  - 7c: change − no-change (probe-locked N2, FN400).
- Averages over the clusters of interest; 20 Hz non-causal low-pass filter (`_lpfilt`) for plotting.

### Script 8: individual plots (8a, 8b, 8c)

- 8a: stimulus-locked N2pc/CDA difference waves and HEOG difference wave (`_correct`); 8b: probe-locked N2pc; 8c: probe-locked N2/FN400.

## Phase IV: Group analysis and measurement

- **Script 9** (9a, 9b, 9c; `Grand_Average_ERPs/`): grand averages with standard errors. 9a uses the `_correct` files.
- **Script 10** (10a, 10b, 10c): grand-average plots with shaded SEM. 10a uses the `_correct` files.
- **Manuscript figures**: `Script_Plots_N2pc_CDA_paper.m`, `Script_Plots_N2_FN400_paper.m`.
- **Permutation tests** (`ERP_Cluster_Analyses/`, Steps 1–4): cluster-based tests (clustGND) defining the time windows of the stimulus-locked N2pc/CDA and the probe-locked N2pc, N2 and FN400.
- **Script 11** (`ERP_Measurements/`): mean amplitude and latency (pop_geterpvalues) in these windows, exported as TXT files for R. 11a stimulus-locked N2pc/CDA (`_correct`); 11b probe-locked N2pc; 11c probe-locked N2/FN400; 11d early CDA, exploratory (`_correct`).

## Phase V: Exploratory analyses

Stimulus-locked, correct trials, N = 39. Outputs: `Alpha_correct/` and `Negative_Slow_Wave_correct/`.

### Negative slow wave (`Negative_Slow_Wave/`)

- `Step1_Compute_Negative_Slow_Wave.m`: NSW over the 14 posterior channels, trial-level baseline correction; filtered and unfiltered waveforms.
- `Step2_Permutation_Test_Negative_Slow_Wave.m`: cluster-based permutation tests between conditions.
- `Step3_Plots_Negative_Slow_Wave.m`: grand averages and mean-amplitude bar charts in the cluster windows.
- `Step4_Bayesian_Window_Extraction_NSW.m`: mean amplitudes in the cluster windows and in the a priori 330–880 ms set-size window, exported as CSV for R (Step 9).

### Alpha power (`Alpha_Oscillations/`)

- `Step1_Compute_Alpha_Power.m`: 8–12 Hz power (Hilbert transform), % change from the −200–0 ms baseline.
- `Step2_Plots_Alpha_Power.m`: time-resolved power and mean bar charts (300–996.1 ms).
- `Step3_Bays_Factor_Alpha_Power.m`: paired t-tests and CSV export for the Bayesian t-tests in R (Step 8).

## Phase VI: Statistical analyses in R

Folder `Data_Analysis_Scripts_R`. Set the working directory to the R project folder (the scripts use `here()`). All scripts reproduce the values reported in the Stage 2 manuscript.

| Script | Analysis | N |
|---|---|---|
| Step 1a | Accuracy (Models 1.a, 1.b, 7) | 53 |
| Step 1b | d′ with log-linear correction, Bayesian t-tests (exploratory) | 53 |
| Step 2 | N2pc, sustained CDA, early CDA mean amplitude (Models 2, 3; early CDA exploratory) | 39 |
| Step 3 | N2/FN400 onset latency by probe type (4.b, 4.c); exploratory probe × stimulus type | 40 / 29* |
| Step 4 | N2/FN400 mean amplitude by stimulus type (5.a, 6.a) | 53 |
| Step 5 | N2/FN400 peak latency by stimulus type (5.b, 6.b) | 53 |
| Step 6 | N2/FN400 peak latency by probe type (8.b, 9.b), collapsed bins | 53 |
| Step 7 | N2/FN400 mean amplitude by probe type (8.a, 9.a), collapsed bins | 53 |
| Step 8 | Bayesian t-tests, alpha power (exploratory) | 39 |
| Step 9 | Bayesian t-tests, negative slow wave (exploratory) | 39 |

\* Participants with an onset latency that could not be measured in any probe bin are excluded (N2: 40; FN400: 29).

Probe-type analyses use the bins in which 3-RWP and 3-RSP are collapsed (bin50 = repeated item probe, bin51 = unrepeated item probe).

## Contact

Hanane Ramzaoui, ramzaoui.hanane@gmail.com
