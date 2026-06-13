# Gridlock 2.0 – Traffic Demand Prediction

This project was developed for the Gridlock 2.0 Traffic Demand Prediction Hackathon.

The objective was to predict the continuous traffic `demand` value using features such as location, timestamp, road type, number of lanes, weather, temperature, landmarks, and large vehicle access.

## Dataset

* Training records: 77,299
* Test records: 41,778
* Target column: `demand`
* Evaluation metric: R² score

The competition dataset is not included in this repository.

To run the project, place the following files inside a `dataset/` folder:

```text
train.csv
test.csv
sample_submission.csv
```

## Approach

The project uses an end-to-end regression pipeline consisting of:

* Missing-value handling
* Timestamp conversion into hour, minute, and minute-of-day
* Cyclic time features using sine and cosine
* Geohash prefix features for location information
* Road, weather, lane, and vehicle interaction features
* Time-aware train-validation split
* Two-model ensemble prediction
* Final submission generation

## Models Used

### ExtraTreesRegressor

ExtraTrees was used to capture complex and non-linear relationships between traffic demand, location, time, and road-related features.

### HistGradientBoostingRegressor

HistGradientBoosting was used as a second regression model to provide different prediction patterns and improve ensemble diversity.

## Ensemble Method

Both models were evaluated using validation R² score.

Different weight combinations were tested:

```text
Final prediction =
ExtraTrees weight × ExtraTrees prediction
+
HistGradientBoosting weight × HistGradientBoosting prediction
```

The best weights were selected using the time-aware validation set.

## Validation Strategy

A time-aware validation split was used instead of a random split because the test data represents future timestamps.

Earlier records were used for training, while later records were used for validation. This helped reduce data leakage and provided a more realistic estimate of model performance.

## Result

The final pipeline achieved an R²-based leaderboard score of approximately **86/100**.

## Technologies Used

* Python
* Pandas
* NumPy
* Scikit-learn
* ExtraTreesRegressor
* HistGradientBoostingRegressor
* Jupyter Notebook

## Output

The final submission file contains exactly two columns:

```text
Index,demand
```

Example:

```text
Index,demand
0,0.1051
1,0.0824
2,0.1347
```

## Repository Files

```text
README.md
gridlock_two_model.ipynb
requirements.txt
.gitignore
```

## Installation

```bash
pip install -r requirements.txt
```

## Running the Project

1. Add the dataset files to the `dataset/` folder.
2. Open `gridlock_two_model.ipynb`.
3. Run all cells from top to bottom.
4. The final submission CSV will be generated automatically.
