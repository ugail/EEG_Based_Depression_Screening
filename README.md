EEG-Based Depression Screening from 8-Channel Resting-State EEG (Subject-wise evaluation, compact baselines, connectivity ablation)
by **Hassan Ugail, Newton Howard, Ali Ahmed Elmahmudi and Zied Mnasri
**

Motivation of this work: 
A central problem in the related literature is that many EEG classification studies are evaluated in ways that are overly optimistic. Resting-state EEG is commonly segmented into multiple epochs per participant before feature extraction or model training. If train/test partitioning is then performed at the segment level rather than the participant level, segments from the same individual can appear in both sets. Because EEG contains substantial within-subject identity information, a classifier evaluated under this regime may learn person-specific patterns rather than disease-related signal, inflating apparent performance without reflecting true diagnostic generalisation. This issue has been explicitly identified as a barrier to clinical translation. Related concerns arise from dataset heterogeneity, including inconsistencies in diagnostic definition, medication exposure, and population composition, all of which complicate cross-study comparison and external validity. For depression-related EEG research, these are not minor technicalities. In fact, they directly affect whether the reported accuracy of a model can be interpreted as meaningful.


What this notebook does:

- It loads the processed outputs from the preprocessing notebook and reproduces the final experimental package

- Correct subject identity handling

 - uses subject_key = label + "_" + subject_id
 - avoids merging H_S1 and MDD_S1


Experiment 1 — Compact 8-channel baselines:
- Extra Trees on asymmetry-aware spectral features
- MLP on the same feature vector
- compact 1D CNN on raw 8-channel segments
  
Experiment 2 — Connectivity ablation:
 - Extra Trees on spectral features only
 - Extra Trees on connectivity features only
 - Extra Trees on spectral + connectivity fusion
 - MLP on spectral + connectivity fusion

Output:
 - figures
 - tables
 - results CSV files


Dataset:
This notebook assumes you already generated the processed files from the public dataset:
Mumtaz, Wajid (2016). MDD Patients and Healthy Controls EEG Data (New). figshare. Dataset.
DOI: 10.6084/m9.figshare.4244171.v2

Input assumption:
This notebook expects the processed outputs from the earlier preprocessing stage under:
/content/drive/MyDrive/.../run_subjectwise_baselines_v1/processed/

Note: "run_subjectwise_baselines_v1/processed/" is available at: https://drive.google.com/drive/folders/1K4J-jqfWtoG7njM21aOgc2Ct7QgYf4W9?usp=sharing

Output philosophy:
Only a small set of result files is written so analysis can be done elsewhere if needed.
