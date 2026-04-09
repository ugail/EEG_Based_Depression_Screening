EEG-Based Depression Screening from 8-Channel Resting-State EEG
by Hassan Ugail, Newton Howard, Ali Ahmed Elmahmudi and Zied Mnasri

Subject-wise evaluation, compact baselines, connectivity ablation
This notebook consolidates the final experimental pipeline into one place.

What this notebook does
It loads the processed outputs from the preprocessing notebook and reproduces the final experimental package:

Correct subject identity handling

uses subject_key = label + "_" + subject_id
avoids merging H_S1 and MDD_S1
Experiment 1 — Compact 8-channel baselines

Extra Trees on asymmetry-aware spectral features
MLP on the same feature vector
compact 1D CNN on raw 8-channel segments
Experiment 2 — Connectivity ablation

Extra Trees on spectral features only
Extra Trees on connectivity features only
Extra Trees on spectral + connectivity fusion
MLP on spectral + connectivity fusion
Publication-ready outputs

manuscript tables
compact results CSV files
high-resolution figures with non-overlapping labels
Dataset
This notebook assumes you already generated the processed files from the public dataset:

Mumtaz, Wajid (2016). MDD Patients and Healthy Controls EEG Data (New). figshare. Dataset.
DOI: 10.6084/m9.figshare.4244171.v2

Input assumption
This notebook expects the processed outputs from the earlier preprocessing stage under:

/content/drive/MyDrive/.../run_subjectwise_baselines_v1/processed/

Note: "run_subjectwise_baselines_v1/processed/" is available at: https://drive.google.com/drive/folders/1K4J-jqfWtoG7njM21aOgc2Ct7QgYf4W9?usp=sharing

Output philosophy
Only a small set of result files is written so analysis can be done elsewhere if needed.
