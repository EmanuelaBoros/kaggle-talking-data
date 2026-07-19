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

## Expected Data Layout

Kaggle data is not included in this repository. Download the competition files and place them in a local `data/` folder:

```text
data/
├── gender_age_train.csv
├── gender_age_test.csv
├── phone_brand_device_model.csv
├── events.csv
├── app_events.csv
└── app_labels.csv
```

`simple_starter.py` currently reads all of these files, although the active feature set mainly uses device metadata and event-level aggregates.

## Feature Engineering

The script builds features from:

| Source file | Features used |
|---|---|
| `gender_age_train.csv` | Target label `group`; `gender` and `age` are dropped after deriving the target. |
| `gender_age_test.csv` | Test `device_id` values. |
| `phone_brand_device_model.csv` | Encoded `phone_brand` and `device_model`. |
| `events.csv` | Event count per device, mean event id per device, and timestamp parts: year, month, day, hour, minute. |
| `app_events.csv` / `app_labels.csv` | Loaded but not merged into the active feature table. |

Additional aggregate features:

- `device_model_freq`: number of rows sharing the same encoded device model.
- `device_model_prob`: mean day value grouped by encoded device model.

Missing values are filled with `-1`.

## Models

The active model is XGBoost:

```python
objective = "multi:softprob"
num_class = 12
eval_metric = "mlogloss"
```

The script also includes alternative helper functions for:

- K-nearest neighbors with `KNeighborsClassifier`.
- A legacy Keras `Convolution1D` neural network.

These alternatives are present but commented out in the main run section.

## Requirements

This is legacy code and uses older APIs:

- `sklearn.cross_validation.train_test_split`, now replaced by `sklearn.model_selection.train_test_split`.
- `DataFrame.as_matrix`, now replaced by `.values` or `.to_numpy()`.
- Older Keras argument names such as `nb_filter`, `filter_length`, `nb_epoch`, and `init`.

Original-style dependencies:

```bash
pip install numpy pandas scikit-learn xgboost keras
```

For modern Python environments, update the deprecated scikit-learn, pandas, and Keras calls before running.

## Usage

From the repository root:

```bash
mkdir -p data
# place the Kaggle CSV files in data/
python simple_starter.py
```

The script:

1. Reads event, app, brand/model, train, and test CSV files.
2. Encodes categorical fields.
3. Builds per-device aggregate features.
4. Splits the training data into train/validation subsets.
5. Trains an XGBoost multiclass model.
6. Writes a timestamped submission file.


