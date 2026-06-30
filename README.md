# Rising Waters – ML-Powered Flood Prediction System

A flood early-warning system trained on historical weather data, comparing
four classifiers (Decision Tree, Random Forest, KNN, XGBoost) and serving
the best one through a Flask web app.

## Project structure
```
rising_waters/
├── data/
│   ├── generate_data.py     # builds the historical weather dataset
│   └── flood_data.csv       # generated dataset (7 features + FLOOD label)
├── model/
│   ├── train.py             # trains & compares all 4 classifiers
│   ├── best_model.pkl       # saved best-performing model
│   ├── scaler.pkl           # saved StandardScaler
│   └── results.json         # accuracy/precision/recall/F1 per model
├── templates/
│   ├── base.html
│   ├── index.html           # prediction form + result
│   └── about.html           # model performance comparison page
├── app.py                   # Flask application
└── requirements.txt
```

## Features used
ANNUAL_RAINFALL, SEASONAL_RAINFALL, CLOUD_VISIBILITY, HUMIDITY,
RIVER_DISCHARGE, DRAINAGE_INDEX, TEMPERATURE

## Setup
```bash
pip install -r requirements.txt

# 1. Generate the dataset (or swap in your own real historical CSV with the same columns)
python data/generate_data.py

# 2. Train and compare all four models — saves the best one automatically
python model/train.py

# 3. Run the web app
python app.py
```
Visit `http://localhost:5000`.

## Using your own real dataset
Replace `data/flood_data.csv` with real historical weather + flood-occurrence
records using the same column names, then re-run `model/train.py`. No other
code changes are needed — the Flask app always loads whichever model
`train.py` determined was best.

## Deployment (IBM Cloud / any PaaS)
This is a standard Flask app — deploy with:
- `gunicorn app:app` as the production WSGI entrypoint
- IBM Cloud Code Engine, Cloud Foundry, or any container/PaaS that supports
  Python buildpacks
- Set `PORT` env var if the platform requires a specific port binding

## Notes on the dataset
`generate_data.py` produces a synthetic dataset with a physically-motivated
risk formula (rainfall, river discharge, drainage capacity, humidity, etc.)
plus noise, so model comparisons are meaningful but not exact replicas of
any real region. Swap in real historical records before operational use.
