# EEG-Based Depression Screening from 8-Channel Resting-State EEG

**Subject-wise evaluation, compact baselines, connectivity ablation, and EEG-specific deep-learning baselines**

By **Hassan Ugail, Newton Howard, Ali Ahmed Elmahmudi, and Zied Mnasri**

Cite as: H Ugail, N Howard, A A Elmahmudi, and Z Mnasri, *Subject-Wise Depression Screening from 8-Channel Resting-State EEG using Asymmetry-Aware Spectral Features and Connectivity Ablation*, Sensors, To Appear, 2026.

## Motivation

A central problem in the related literature is that many EEG classification studies are evaluated in ways that are overly optimistic. Resting-state EEG is commonly segmented into multiple epochs per participant before feature extraction or model training. If train and test partitioning is then performed at the segment level rather than the participant level, segments from the same individual can appear in both sets. Because EEG contains substantial within-subject identity information, a classifier evaluated under this regime may learn person-specific patterns rather than disease-related signal, inflating apparent performance without reflecting true diagnostic generalisation. Related concerns arise from dataset heterogeneity, including inconsistencies in diagnostic definition, medication exposure, and population composition, all of which complicate cross-study comparison and external validity. For depression-related EEG research, these are not minor technicalities. They directly affect whether the reported accuracy of a model can be interpreted as meaningful.

This notebook addresses these concerns through a strictly leakage-free, subject-wise evaluation pipeline tailored for compact eight-channel EEG configurations.


<img width="3034" height="1474" alt="fig_baselines_8ch" src="https://github.com/user-attachments/assets/746c74d8-7e3a-42fa-a231-ab75cb3b0525" />


## What this notebook does

It loads the processed outputs from the preprocessing notebook and reproduces the final experimental package.

1. **Correct subject identity handling**
   - uses `subject_key = label + "_" + subject_id`
   - avoids merging `H_S1` and `MDD_S1`

2. **Experiment 1 — Compact 8-channel baselines**
   - Extra Trees on asymmetry-aware spectral features
   - MLP on the same feature vector
   - compact 1D CNN on raw 8-channel segments
   - **EEGNet** (Lawhern et al., 2018) on raw 8-channel segments
   - **ShallowConvNet** (Schirrmeister et al., 2017) on raw 8-channel segments

3. **Experiment 2 — Connectivity ablation**
   - Extra Trees on spectral features only
   - Extra Trees on connectivity features only
   - Extra Trees on spectral and connectivity fusion
   - MLP on the same fused representation

4. **Experiment 3 — Spectral feature-selection ablation**
   - Top-K Extra Trees over K in {5, 10, 15, 20, 30, 50, 70, 90} features
   - Tests whether the 90-dimensional spectral vector is over-parameterised
   - Importance ranking is recomputed inside every repeat from training data only

5. **Leakage-free training regime**
   - Repeated subject-wise holdout with 10 repeats
   - All preprocessing steps either subject-independent or fitted on training subjects only
   - Per-channel z-score normalisation for the convolutional networks fitted on the inner training split alone

6. **Bootstrap inference**
   - 95% percentile bootstrap confidence intervals of the mean across the 10 repeats
   - Reported for every metric in every experiment, alongside per-repeat standard deviation
   - Treated as descriptive uncertainty summaries rather than formal population-level bounds

7. **Publication-ready outputs**
   - manuscript tables with both standard deviation and 95% CI columns
   - compact results CSV files
   - high-resolution figures with 95% CI error bars and a colour-blind-safe palette

## Dataset

This notebook assumes you already generated the processed files from the public dataset:

**Mumtaz, Wajid (2016).** *MDD Patients and Healthy Controls EEG Data (New).* figshare. Dataset.
DOI: `10.6084/m9.figshare.4244171.v2`

Diagnostic labels were assigned at the time of original data collection by a qualified psychiatrist according to DSM-IV criteria, with severity quantified using the Beck Depression Inventory, second edition. The public release contains only major depressive disorder and healthy-control labels.

## Input assumption

This notebook expects the processed outputs from the earlier preprocessing stage under:

`/content/drive/MyDrive/.../run_subjectwise_baselines_v1/processed/`

The processed inputs are available at:
https://drive.google.com/drive/folders/1K4J-jqfWtoG7njM21aOgc2Ct7QgYf4W9?usp=sharing

## Output

The notebook produces a compact set of files for downstream analysis and manuscript preparation.

- `final_repeat_metrics.csv` — per-repeat per-model metrics
- `final_summary.csv` — per-model mean, SD, and 95% bootstrap CI
- `final_subject_predictions.csv` — subject-level predictions across repeats
- `final_top_features.csv` — top Gini-importance features per Extra Trees model
- `final_manuscript_table.csv` and `.xlsx` — publication-ready combined table
- `final_feature_selection.csv` and `final_feature_selection_summary.csv` — Experiment 3 results
- `fig_baselines_8ch.png` — Compact baselines comparison (Figure 1)
- `fig_fusion_ablation.png` — Spectral vs connectivity vs fusion (Figure 2)
- `fig_feature_selection.png` — Spectral feature-selection curve (Figure 3)
- `fig_top_feature_importance.png` — Top features panel (Figure 4)
- `manifest.json` — full configuration record for reproducibility

## Cohort summary

After applying the class-aware composite key correction, the final analysis cohort comprised 56 valid unique subjects, of whom 31 belonged to the MDD group and 25 to the healthy-control group.

## Headline result

Under repeated subject-wise holdout evaluation, the best-performing model is an Extra Trees classifier trained on eight-channel asymmetry-aware spectral features, achieving a mean balanced accuracy of 93.5% (95% bootstrap CI 89.6 to 96.8) and a mean AUROC of 98.6% (95% bootstrap CI 96.2 to 100.0). EEGNet and ShallowConvNet reach competitive but lower mean balanced accuracy under the same protocol.

## Reproducibility

All experiments use seed 42, incremented across the 10 repeats. The full configuration, including the channel selection, connectivity bands, feature-selection grid, bootstrap parameters, and output paths, is captured in `manifest.json` written at the end of the run.
