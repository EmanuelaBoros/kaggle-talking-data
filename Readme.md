# Kaggle TalkingData Mobile User Demographics

Legacy Kaggle experiment code for the **TalkingData Mobile User Demographics** competition. The goal is to predict a mobile user’s demographic group from device metadata, app/event activity, and timestamp-derived features.

The repository contains a single starter script, `simple_starter.py`, which reads the Kaggle CSV files, builds a compact feature table, trains a multiclass model, and writes a Kaggle-style submission file.

## Competition Task

Given anonymized mobile-device data, predict one of 12 demographic classes:

```text
F23-, F24-26, F27-28, F29-32, F33-42, F43+,
M22-, M23-26, M27-28, M29-31, M32-38, M39+
```

The output submission contains one probability column for each class.

## Repository Contents

```text
.
├── simple_starter.py
└── Readme.md
```
