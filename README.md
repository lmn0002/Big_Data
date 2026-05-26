# Big Data Pipeline Project

## Overview
This project builds a real data engineering pipeline from scratch. It downloads NYC Yellow Taxi trip data, processes it using Python, and ingests it into a PostgreSQL database : all running inside Docker containers.

---

## What This Project Does
- Downloads **3.4 million rows** of real NYC Yellow Taxi trip data
- Processes and transforms the data using **Pandas**
- Ingests the data into a **PostgreSQL** database
- Runs everything inside **Docker** containers for reproducibility
- Manages dependencies using **uv** (virtual environment manager)

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| Python 3.13 | Main programming language |
| Docker | Containerization |
| PostgreSQL 16 | Database |
| Pandas | Data processing |
| PyArrow | Parquet file support |
| SQLAlchemy | Database connection |
| Jupyter Notebook | Data exploration and ingestion |
| uv | Virtual environment and dependency management |
| pgcli | PostgreSQL command-line interface |

---

## Project Structure
```
Big_Data/
├── .gitignore
├── README.md
└── pipeline/
    ├── Dockerfile
    ├── pipeline.py
    ├── pyproject.toml
    └── uv.lock
```
---

## How to Run This Project

### Prerequisites
- Docker installed
- Python 3.13 installed
- uv installed (`pip install uv`)

---

### Step 1: Clone the Repository
```bash
git clone https://github.com/lmn0002/Big_Data.git
cd Big_Data
```
---

### Step 2: Set Up Python Environment
```bash
cd pipeline
uv init --python 3.13
uv add pandas pyarrow sqlalchemy "psycopg[binary,pool]"
```
---
### Step 3: Run PostgreSQL in Docker
```bash
mkdir ny_taxi_DB
docker run -it --rm 
-e POSTGRES_USER="root" 
-e POSTGRES_PASSWORD="root" 
-e POSTGRES_DB="ny_taxi" 
-v $(pwd)/ny_taxi_DB:/var/lib/postgresql/data 
-p 5432:5432 
postgres:16
```
Wait until you see: **"database system is ready to accept connections"** 
---
### Step 4: Build and Run the Pipeline
```bash
cd pipeline
docker build -t test:pandas .
docker run -it --rm -v "$(pwd):/code" test:pandas 12
```
---
### Step 5: Verify Data in PostgreSQL
Connect to the database:
```bash
uv run pgcli -h localhost -p 5432 -u root -d ny_taxi
```
Run this query to verify the data:
```sql
SELECT COUNT(*) FROM yellow_taxi_data;
```
#### Expected result: **3,475,226 rows** 
---
## Data Source

### NYC Yellow Taxi Trip Data (January 2025)
### Source: NYCTLCTripRecordData (https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
---
## Key Learnings
- How to build a data pipeline from scratch
- How to run databases and applications inside Docker containers
- How to ingest large datasets (3.4M+ rows) into PostgreSQL using Python
- How to manage Python virtual environments with uv
- How to use Jupyter Notebooks for data exploration and ingestion
