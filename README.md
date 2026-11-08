# BIDS-EEG Dataset: Examining Perceptual Grouping on Stages of Processing in Visual Working Memory: An ERP Study

**Authors:** Hanane Ramzaoui, Melissa Beck

## 1. Description: Project Overview

We present an electrophysiological dataset recorded from fifty-three subjects performing a bilateral change-detection task to investigate how perceptual grouping, based on color repetition, influences Visual Working Memory (VWM) processing efficiency.

The study is designed to temporally isolate and measure the neural correlates of several critical VWM stages: individuation during encoding, maintenance, initial comparison, percept-memory comparison, and decision making/late comparison. This is achieved using specific Event-Related Potential (ERP) markers (N2pc, CDA, N2, FN400).

## 2. Experimental Task and Conditions

Subjects were cued to encode the colors of 2 or 3 squares in one visual hemifield. After a maintenance period, a single-item probe was presented to determine if its color had changed. Each trial consists of a memory array, a probe presented 1 s later, and a manual response.

### Key Manipulations

The memory array contained four primary conditions:

- **Unrepeated (UR):** Arrays with 2 or 3 unique colors (2-UR, 3-UR).
- **Repeated (R):** Arrays with 3 items, where two colors were repeated. This condition was further subdivided based on spatial arrangement:
  - Two repeated colors with strong spatial proximity (3-RSP).
  - Two repeated colors with weak spatial proximity (3-RWP).

### The Probe in Repeated Conditions

In the repeated conditions (3-RSP and 3-RWP), the single-item probe could test two different item types for change detection:

- **Repeated Item:** The probe tests one of the two squares that share the same color.
- **Unrepeated Item (Singleton):** The probe tests the single square with the unique color.

## 3. Primary Measurements

The study leverages the following ERP components to index different VWM processing stages:

| VWM Stage | ERP Marker | Event Locking |
| --- | --- | --- |
| VWM Individuation during Encoding | N2pc | Stimulus-locked (memory array) |
| Maintenance/Load | CDA (Contralateral Delay Activity) | Stimulus-locked (memory array) |
| Initial Comparison | N2pc | Probe-locked |
| Percept-Memory Comparison | N2 | Probe-locked |
| Decision Making/Late Comparison | FN400 | Probe-locked |

The main behavioral measure is **accuracy** (proportion of correct responses per condition), analyzed for all 53 participants. All behavioral analyses use N = 53.

### Exploratory Analyses

In addition to the registered analyses above, the following exploratory analyses were run (behavioral: N = 53; stimulus-locked EEG: N = 39):

| Analysis | Measure | Event Locking |
| --- | --- | --- |
| Sensitivity (d′) | d′ = z(hit rate) − z(false-alarm rate) per condition, with change trials as signal; log-linear correction for hit or false-alarm rates of 0 or 1 (0.5 added to hit and false-alarm counts, 1 to the number of change and no-change trials) | Behavioral (manual response to the probe) |
| Early CDA | Contralateral minus ipsilateral amplitude, 300–450 ms (transition from selection to stable maintenance) | Stimulus-locked (memory array) |
| Negative slow wave (NSW) | Non-lateralized amplitude over the 14 posterior channels; cluster-based permutation tests across time between conditions, and Bayesian paired t-tests on the mean amplitude in the cluster windows and in an a priori 330–880 ms set-size window | Stimulus-locked (memory array) |
| Alpha-power suppression | 8–12 Hz power over the 14 posterior channels, % change from the −200–0 ms baseline, averaged over 300–996.1 ms; Bayesian paired t-tests | Stimulus-locked (memory array) |

## 4. Acquisition Details and Structure

### Acquisition Parameters

| Parameter | Detail |
| --- | --- |
| Subjects (N) | 53 (all 53 in the behavioral and probe-locked ERP analyses; N = 39 in the stimulus-locked ERP analyses; see `participants.tsv`) |
| Electrode System | BioSemi ActiveTwo System |
| Recorded Channels | 69 (64 scalp, 3 EOG, 2 mastoids); CMS/DRL as recording reference |
| Sampling Rate | 512 Hz |
| Trials | 1,248 experimental trials per participant (fewer for a few participants) |

### BIDS Compliance

The data is structured following the Brain Imaging Data Structure (BIDS) standard for EEG.

- **Acquisition Parameters:** Detailed recording specifications are provided in the task-level BIDS JSON file (`task-visualworkingmemorytask_eeg.json`) and in each participant's `*_eeg.json` and `*_channels.tsv`.
- **Methodology:** Comprehensive details on offline preprocessing (e.g., re-referencing to average mastoids, ICA artifact removal, 0.1 Hz high-pass filtering) and the precise analysis plan (e.g., ERP measurement windows, HEOG artifact thresholds, channel clusters) are provided in the Stage 1 protocol on OSF (https://doi.org/10.17605/OSF.IO/8ZS96).
- **Participants:** `participants.tsv` indicates, for each participant, the analyses in which the data were included, and the reason for exclusion from the stimulus-locked ERP analyses (residual horizontal eye movements, HEOG).

## 5. Event Codes/Triggers

The following table maps the trigger codes recorded in the EEG data to the specific experimental events. Codes 51–58 mark the **memory array** onset (time-locking event of the stimulus-locked N2pc and CDA analyses); codes 11–42 mark the **probe** onset 1 s later (time-locking event of the probe-locked N2pc, N2, and FN400 analyses).

Acronym Key: UR = Unrepeated; RWP = Repeated Weak Proximity; RSP = Repeated Strong Proximity.

| Trigger Code | Event Description |
| --- | --- |
| 51 | Memory array onset: 2-UR \| Left Cue |
| 52 | Memory array onset: 3-UR \| Left Cue |
| 53 | Memory array onset: 3-RWP \| Left Cue |
| 54 | Memory array onset: 3-RSP \| Left Cue |
| 55 | Memory array onset: 2-UR \| Right Cue |
| 56 | Memory array onset: 3-UR \| Right Cue |
| 57 | Memory array onset: 3-RWP \| Right Cue |
| 58 | Memory array onset: 3-RSP \| Right Cue |
| 11 | Probe onset: 2-UR \| Left Cue \| Change \| Unrepeated Probe |
| 12 | Probe onset: 3-UR \| Left Cue \| Change \| Unrepeated Probe |
| 13 | Probe onset: 3-RWP \| Left Cue \| Change \| Unrepeated Probe |
| 14 | Probe onset: 3-RSP \| Left Cue \| Change \| Unrepeated Probe |
| 17 | Probe onset: 3-RWP \| Left Cue \| Change \| Repeated Probe |
| 18 | Probe onset: 3-RSP \| Left Cue \| Change \| Repeated Probe |
| 19 | Probe onset: 2-UR \| Left Cue \| No-Change \| Unrepeated Probe |
| 20 | Probe onset: 3-UR \| Left Cue \| No-Change \| Unrepeated Probe |
| 21 | Probe onset: 3-RWP \| Left Cue \| No-Change \| Unrepeated Probe |
| 22 | Probe onset: 3-RSP \| Left Cue \| No-Change \| Unrepeated Probe |
| 25 | Probe onset: 3-RWP \| Left Cue \| No-Change \| Repeated Probe |
| 26 | Probe onset: 3-RSP \| Left Cue \| No-Change \| Repeated Probe |
| 27 | Probe onset: 2-UR \| Right Cue \| Change \| Unrepeated Probe |
| 28 | Probe onset: 3-UR \| Right Cue \| Change \| Unrepeated Probe |
| 29 | Probe onset: 3-RWP \| Right Cue \| Change \| Unrepeated Probe |
| 30 | Probe onset: 3-RSP \| Right Cue \| Change \| Unrepeated Probe |
| 33 | Probe onset: 3-RWP \| Right Cue \| Change \| Repeated Probe |
| 34 | Probe onset: 3-RSP \| Right Cue \| Change \| Repeated Probe |
| 35 | Probe onset: 2-UR \| Right Cue \| No-Change \| Unrepeated Probe |
| 36 | Probe onset: 3-UR \| Right Cue \| No-Change \| Unrepeated Probe |
| 37 | Probe onset: 3-RWP \| Right Cue \| No-Change \| Unrepeated Probe |
| 38 | Probe onset: 3-RSP \| Right Cue \| No-Change \| Unrepeated Probe |
| 41 | Probe onset: 3-RWP \| Right Cue \| No-Change \| Repeated Probe |
| 42 | Probe onset: 3-RSP \| Right Cue \| No-Change \| Repeated Probe |
| 120 | Manual Response: Correct |
| 121 | Manual Response: Incorrect |
| 101, 111, 254 | Technical markers / sync pulses |

In versions 1.0.0 and 1.0.1, the labels of codes 11–42 and 51–58 were swapped in this README and in `task-visualworkingmemorytask_events.json`; the codes in the data files were always correct.

## 6. Derivatives

`derivatives/ReplicationPeterson2015/` contains the analysis code and intermediate files of the corrected analysis (correct-response trials only, as specified in the Stage 1 protocol), including the original ICA weights for each participant, the MATLAB (EEGLAB/ERPLAB) scripts and bin descriptors, the intermediate ERP files, and the R scripts and data used for the statistical analyses. See the README in that folder for details.

An independent Julia reimplementation of the pipeline, validated against these derivatives, is available at https://github.com/HananeRam/vwm-erp-julia.

## 7. Notes on Specific Files

- **sub-10 and sub-49:** in versions 1.0.0 and 1.0.1, the files of these two participants were not the recordings used in the analysis (incomplete events; sub-49 stored at 2048 Hz). Since version 1.1.0, they contain the continuous recordings used as input of the analysis (512 Hz, practice trials removed), with matching events.

See `CHANGES` for the full version history.

## 8. Protocol Registration and Reference

For this dataset project, the approved Stage 1 protocol (registered report) can be found at this OSF link (2024, October 15): https://doi.org/10.17605/OSF.IO/8ZS96

## 9. Contact and Ethics

- **Affiliation (data collection):** Louisiana State University
- **Ethical Approval:** Institutional Review Board of Louisiana State University (IRBAM-23-0273 from March 1, 2023)
- **Contact:** ramzaoui.hanane@gmail.com
