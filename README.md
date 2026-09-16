

# NYC Mobility Demand Analysis

Large-scale **data engineering and machine learning project** focused on analyzing New York City taxi and for-hire vehicle mobility using tens of millions of trip records.

The system integrates **NYC TLC transportation data** with external sources including **weather, urban events and geographic information** to analyze mobility patterns, identify high-demand areas and build predictive models.

The project covers the complete data workflow: from raw data extraction and distributed processing with **Apache Spark**, to machine learning, a **FastAPI REST API** and an interactive **React dashboard**.

![NYC Mobility Dashboard](docs/images/dashboard.png)

---

## Key Features

- Processing of **tens of millions of NYC taxi and for-hire vehicle trips** with PySpark
- Multi-layer pipeline for data exploration, validation, standardization and aggregation
- Integration of heterogeneous transportation, weather, event and geographic datasets
- Feature engineering for temporal and geographic mobility analysis
- Machine learning models for demand prediction, tip estimation and urban mobility analysis
- REST API built with **FastAPI**
- Interactive dashboard built with **React**
- Object storage and data management through **MinIO**

---

## Data Sources

The project integrates several real-world datasets.

### NYC Taxi & Limousine Commission

Trip records from:

- Yellow Taxi
- Green Taxi
- High Volume For-Hire Vehicles (FHVHV)

The datasets contain information such as:

- Pickup and drop-off timestamps
- Pickup and drop-off zones
- Trip distance
- Fare information
- Tips
- Passenger information
- Service type

### Weather

Hourly meteorological information obtained through the **Open-Meteo API**.

Weather variables are combined with transportation data to analyze their relationship with mobility demand.

### Urban Events

Event data obtained from **NYC Open Data** is incorporated to study the relationship between urban activity and transportation patterns.

### Geographic & External Data

NYC TLC taxi-zone information is used to associate trips with:

- Boroughs
- Taxi zones
- Geographic areas

Additional external information, including restaurant and rental-related data, is incorporated during the enrichment and aggregation stages.

---

# Data Pipeline

The project implements a layered data architecture that transforms raw data into validated, standardized and ML-ready datasets.

```text
External Data Sources
        │
        ▼
Raw Data
        │
        ▼
Layer 0 — Exploration
        │
        ▼
Layer 1 — Validation
        │
        ▼
Layer 2 — Standardization
        │
        ▼
Layer 3 — Aggregation & Feature Engineering
        │
        ▼
Machine Learning
        │
        ├──► FastAPI Backend
        │
        └──► React Dashboard
```

---

## Layer 0 — Exploration

The first stage performs exploratory analysis over raw NYC TLC data to estimate the scale and characteristics of the datasets.

It analyzes:

- Dataset volume
- Average trip prices
- Peak demand periods
- Distribution by transportation service
- Monthly patterns
- Dataset structure

The resulting summaries provide an initial overview before executing the complete processing pipeline.

---

## Layer 1 — Validation & Data Quality

The first processing layer performs structural and logical validation based on the official **NYC TLC Data Dictionaries**.

Validation includes:

- Data-type verification
- Allowed-domain validation
- Detection of future dates
- Detection of negative trip durations
- Plausibility checks
- Derived-speed validation
- Identification of malformed records

Records are separated into:

```text
clean/
bad_rows/
```

The pipeline also generates JSON validation reports.

---

## Layer 2 — Standardization

Different transportation services use different schemas.

This layer transforms them into a common representation.

Main operations include:

- Timestamp normalization
- Price-column normalization
- Schema unification
- Service-type identification
- Trip-duration calculation
- Weekday extraction
- Geographic enrichment
- TLC zone integration

The resulting datasets are stored in **Parquet** format and partitioned by:

```text
year / month / service
```

---

## Layer 3 — Aggregation & Feature Engineering

The final processing layer generates datasets ready for analytics, visualization and machine learning.

Generated information includes:

- Daily transportation demand
- Zone-hour mobility hotspots
- Price variability
- Geographic demand patterns
- Temporal mobility indicators

Transportation data is enriched with external sources including:

- Weather
- Events
- Geographic information
- Rental-related data
- Restaurant information

Additional features include:

- Temporal variables
- Lag features
- Rolling means
- External contextual variables

The output consists of partitioned datasets designed for downstream modeling and visualization.

---

# Machine Learning

The project includes several predictive and analytical tasks.

## Demand Prediction

A multiclass classification problem is used to predict the geographic zone expected to experience the highest transportation demand during the following time period.

---

## Tip Prediction

Regression models are used to estimate tip amounts using trip-related information.

---

## Mobility Pattern Analysis

Additional models and analyses explore relationships between transportation demand and variables such as:

- Time of day
- Geographic area
- Purchasing power
- Weather
- Urban events

---

## Urban Stress Analysis

Additional indicators and models are used to analyze transportation pressure and demand patterns across New York City.

---

# Web Application

The processed data and model outputs are exposed through an interactive web application.

## Backend

The backend is implemented with **FastAPI** and provides REST endpoints for:

- Processed mobility data
- Aggregated indicators
- Model predictions
- Dashboard information

Health endpoint:

```text
http://127.0.0.1:8000/api/health
```

Expected response:

```json
{"status":"ok"}
```

---

## Frontend

The frontend is implemented using:

- React
- Vite
- Leaflet / Folium
- Recharts

The dashboard includes:

- Interactive maps
- Demand visualization
- Temporal trends
- Price information
- Geographic mobility patterns
- External contextual variables

---

# Repository Structure

```text
nyc-mobility-demand-analysis/
│
├── src/
│   ├── extraccion/
│   │   └── main.py
│   │
│   ├── procesamiento/
│   │   ├── capa1/
│   │   ├── capa2/
│   │   └── capa3/
│   │
│   ├── ml/
│   │   ├── models_ej1/
│   │   └── models_ej2/
│   │
│   ├── visualizaciones/
│   │
│   └── main.py
│
├── backend/
│   └── app/
│
├── frontend/
│
├── config/
│
├── data/
│   ├── raw/
│   ├── validated/
│   ├── standarized/
│   ├── aggregated/
│   └── external/
│
├── docs/
│   └── images/
│
├── notebooks/
│
├── outputs/
│
├── pyproject.toml
├── package.json
├── uv.lock
└── README.md
```

The main orchestrator is:

```text
src/main.py
```

It sequentially executes:

```text
Data extraction
      ↓
Layer 1 validation
      ↓
Layer 2 standardization
      ↓
Layer 3 aggregation
      ↓
Machine learning
```

---

# Tech Stack

## Data Engineering

- Python 3.11+
- Apache Spark / PySpark
- Pandas
- PyArrow
- DuckDB
- Parquet
- MinIO

## Machine Learning

- Scikit-learn
- XGBoost
- PyTorch

## Geospatial Analysis

- GeoPandas
- NYC TLC geographic zones

## Backend

- FastAPI
- Uvicorn

## Frontend

- React
- Vite
- Leaflet
- Recharts

## Visualization

- Matplotlib
- Seaborn
- Folium
- Jupyter Notebook

## Development Tools

- Git
- GitHub
- `uv`
- Rich
- VS Code

---

# Installation

## 1. Clone the Repository

```bash
git clone https://github.com/IgRam23/nyc-mobility-demand-analysis.git
cd nyc-mobility-demand-analysis
```

---

## 2. Install Python Dependencies

Install `uv` if necessary:

```bash
pip install uv
```

Then create the environment and install the dependencies:

```bash
uv sync
```

---

# Spark Configuration

The project uses **PySpark** and therefore requires **Java 17**.

Verify the Java installation:

```bash
java -version
```

The output should contain something similar to:

```text
openjdk version "17.x.x"
```

## Windows

Configure Java:

```powershell
$env:JAVA_HOME="C:\Program Files\Eclipse Adoptium\jdk-17.x.x"
$env:Path="$env:JAVA_HOME\bin;$env:Path"
$env:PYSPARK_PYTHON="python"
$env:PYSPARK_DRIVER_PYTHON="python"
```

Spark on Windows may also require:

```text
winutils.exe
hadoop.dll
```

These files can be placed in:

```text
C:\hadoop\bin\
```

Configure Hadoop:

```powershell
$env:HADOOP_HOME="C:\hadoop"
$env:PATH="C:\hadoop\bin;$env:PATH"
```

Verify:

```powershell
Get-Command winutils.exe
Get-Command hadoop.dll
```

---

# Downloading the Data

Project data is stored using **MinIO object storage**.

To download the data while preserving the directory structure:

```bash
uv run -m src.extraccion.download_from_minio
```

Download a specific directory:

```bash
uv run -m src.extraccion.download_from_minio --prefix data/raw/
```

Specify a destination:

```bash
uv run -m src.extraccion.download_from_minio --dest-dir /path/to/destination
```

Force existing files to be overwritten:

```bash
uv run -m src.extraccion.download_from_minio --no-skip
```

A valid local `credentials.json` file is required to access the object storage.

---

# Running the Pipeline

Run the complete workflow:

```bash
uv run -m src.main
```

This executes:

- Data extraction
- Layer 1 validation
- Layer 2 standardization
- Layer 3 aggregation
- Machine learning models

Each component can also be executed independently.

---

## Data Extraction

```bash
uv run -m src.extraccion.main
```

---

## Layer 1 — Validation

```bash
uv run -m src.procesamiento.capa1.main
```

---

## Layer 2 — Standardization

```bash
uv run -m src.procesamiento.capa2.main
```

---

## Layer 3 — Aggregation

```bash
uv run -m src.procesamiento.capa3.main
```

Run all processing layers:

```bash
uv run -m src.procesamiento.main
```

---

## Exploratory Visualizations

```bash
uv run -m src.visualizaciones.main
```

---

## Machine Learning Models

Exercise 1 models:

```bash
uv run -m src.ml.models_ej1.main
```

Exercise 2 models:

```bash
uv run -m src.ml.models_ej2.main
```

---

# Running the Web Application

## Backend

From the project root:

```bash
uv run -m uvicorn backend.app.main:app --reload
```

The backend runs by default at:

```text
http://127.0.0.1:8000
```

---

## Frontend

Open another terminal:

```bash
cd frontend
npm install
npm run dev
```

Vite normally starts the frontend at:

```text
http://localhost:5173
```

---

## Backend + Frontend

Install the complete project:

```bash
npm run setup
```

Start both services:

```bash
npm run dev
```

Available scripts:

```bash
npm run setup
npm run dev
npm run dev:backend
npm run dev:frontend
```

---

# Project Materials

Additional documentation and presentation material produced during the project.

## Final Delivery

- [Final Report](https://docs.google.com/document/d/1BeDUwpIIZ76oQSTDzrjcvYsROVYALI4uF1sLKIetDtw/edit?usp=sharing)
- [Final Presentation](https://canva.link/jxwygyxxe87x09o)
- [Project Demo Video](https://drive.google.com/file/d/1KXJVRdSskvmzjvz8nhEzWeRgG0YjDoWK/view?usp=sharing)

## First Delivery

- [First Report](https://docs.google.com/document/d/1znwca7mk1cS6DRcjjuXsSMnBJvdzIXBFVLbBbsAFyls/edit?usp=sharing)
- [First Presentation](https://docs.google.com/presentation/d/1tKNixIGUhMHiNGJyOn6zXNW0MvKN4t1Iz4JFZ0_KaAE/edit?slide=id.g3c872e10c63_0_294#slide=id.g3c872e10c63_0_294)


# Academic Context

This project was developed during the **2025/26 Proyecto de Datos II** course within the:

**B.Sc. in Data Engineering and Artificial Intelligence**  
Facultad de Informática  
Universidad Complutense de Madrid

The project was developed by **Team MacBrides**:

- Vega García Camacho
- Rosa Gómez-Gil Jónsdóttir
- Daniel Higueras Llorente
- Ignacio Ramírez Suárez
- Marina Triviño de las Heras

---

# Repository Purpose

This repository is maintained as part of my **Data Engineering and Artificial Intelligence portfolio** and showcases an end-to-end data project covering:

- Large-scale distributed data processing
- Data validation and standardization
- Multi-source data integration
- Feature engineering
- Machine learning
- REST API development
- Interactive data visualization
