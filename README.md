# 🌦️ Weather Forecast Data Processing Pipeline

An end-to-end data engineering project built on Google Cloud Platform (GCP) to ingest weather forecast data from the OpenWeather API, process and store it in Google BigQuery using PySpark on Dataproc Serverless, orchestrated through Cloud Composer (Airflow) with CI/CD enabled via GitHub Actions.

---

## 🚀 Key Features

- 📡 Fetches weather data from **OpenWeather API**
- ☁️ Stores raw forecast data in **Google Cloud Storage (GCS)**
- 🔄 Transforms CSV data using **PySpark**
- 🗃️ Loads cleaned data into **BigQuery**
- 🪄 Orchestrated via **Airflow DAGs** on **Cloud Composer**
- ✅ **CI/CD** enabled via **GitHub Actions** for seamless deployment

---

## 🛠️ Tech Stack

| Component            | Usage                                         |
|----------------------|-----------------------------------------------|
| Python               | API interaction and DAG scripting             |
| Airflow (Composer)   | Workflow orchestration                        |
| PySpark              | Data transformation and cleansing             |
| Google Cloud Storage | Staging layer for raw weather data            |
| Dataproc Serverless  | Executes PySpark job without cluster overhead |
| BigQuery             | Final analytics data warehouse                |
| GitHub Actions       | CI/CD deployment automation                   |

---

## 📂 Project Structure

```bash
weather-data-processing/
├── extract_data_dag.py         # DAG 1: Extract weather data, upload to GCS, trigger DAG 2
├── transform_data_dag.py       # DAG 2: PySpark batch job on Dataproc Serverless
├── weather_data_processing.py  # PySpark job to transform and write to BigQuery
├── ci-cd.yaml                  # GitHub Actions workflow for deployment
└── README.md                   # Project documentation
````

---

## 🧪 Data Flow Overview

### 1️⃣ **DAG 1 – `openweather_api_to_gcs`**

* Fetches 5-day forecast using OpenWeather API for Toronto
* Normalizes JSON into tabular CSV via Pandas
* Uploads CSV to GCS partitioned by date
* Triggers DAG 2 upon successful upload

### 2️⃣ **DAG 2 – `transformed_weather_data_to_bq`**

* Invoked via Airflow `TriggerDagRunOperator`
* Submits PySpark batch to **Dataproc Serverless**
* Transforms and cleans weather data
* Writes it to BigQuery (`forecast.weather_data`)

---

## 🧼 Sample BigQuery Schema

| Column         | Type      | Description                   |
| -------------- | --------- | ----------------------------- |
| dt             | TIMESTAMP | Epoch timestamp converted     |
| forecast\_time | TIMESTAMP | Forecasted timestamp          |
| temp           | FLOAT     | Current temperature           |
| feels\_like    | FLOAT     | Feels like temperature        |
| min\_temp      | FLOAT     | Minimum temperature           |
| max\_temp      | FLOAT     | Maximum temperature           |
| humidity       | INT       | Humidity percentage           |
| wind\_speed    | FLOAT     | Wind speed in m/s             |
| clouds\_all    | INT       | Cloudiness percentage         |
| rain\_3h       | FLOAT     | Rainfall in mm (last 3 hours) |
| ...            | ...       | Additional metrics            |

---

## 🧪 Local Testing

### Set up Python virtualenv:

```bash
python -m venv venv
source venv/bin/activate  # or .\venv\Scripts\activate on Windows
pip install -r requirements.txt
```

### Run DAG 1 manually:

```bash
airflow dags trigger openweather_api_to_gcs
```

### Run PySpark script locally (for testing):

```bash
spark-submit weather_data_processing.py
```

---

## 🔁 CI/CD with GitHub Actions

* ✅ GitHub Actions (`ci-cd.yaml`) validates and deploys DAGs on push to main branch
* Automatically syncs DAGs to Composer environment
* Helps with scalable deployment and rollback if needed

---

## 👨‍💻 Author

**Pankaj**
GCP Data Engineer | PySpark | BigQuery | Composer | Dataproc

```
