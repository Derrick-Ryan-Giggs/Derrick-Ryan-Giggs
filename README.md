# Derrick Ryan Giggs

**Data Engineer | Technical Writer | Building Scalable, Reliable Data Pipelines | Cloud & Workflow Automation**

With a passion for modern data stack tooling, I specialize in building production-ready data pipelines using **Python**, **Apache Flink**, **dbt**, and cloud-native GCP services. I focus on clean, maintainable streaming and batch pipelines, orchestration, and infrastructure-as-code.

---

## Core Competencies

### Languages & Querying

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![SQL](https://img.shields.io/badge/SQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Shell Scripting](https://img.shields.io/badge/Shell_Scripting-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://gnu.org/software/bash/)

*Core scripting and advanced querying for data engineering workflows.*

### Data Processing & Ingestion

[![Apache Flink](https://img.shields.io/badge/Apache_Flink-E6526F?style=for-the-badge&logo=apache-flink&logoColor=white)](https://flink.apache.org)
[![dlt](https://img.shields.io/badge/dlt-FF6B35?style=for-the-badge&logo=python&logoColor=white)](https://dlthub.com)
[![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)](https://spark.apache.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)

*Building robust batch and real-time data ingestion pipelines at scale.*

### Cloud & Data Platforms

[![GCP](https://img.shields.io/badge/GCP-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)](https://cloud.google.com)
[![BigQuery](https://img.shields.io/badge/BigQuery-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)](https://cloud.google.com/bigquery)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com)
[![OCI](https://img.shields.io/badge/Oracle_Cloud-F80000?style=for-the-badge&logo=oracle&logoColor=white)](https://oracle.com/cloud)

*Cloud data architecture and modern data warehousing solutions.*

### Orchestration & Infrastructure

[![Airflow](https://img.shields.io/badge/Airflow-017CEE?style=for-the-badge&logo=apache-airflow&logoColor=white)](https://airflow.apache.org)
[![Kestra](https://img.shields.io/badge/Kestra-0066FF?style=for-the-badge&logo=kestra&logoColor=white)](https://kestra.io)
[![Terraform](https://img.shields.io/badge/Terraform-844FBA?style=for-the-badge&logo=terraform&logoColor=white)](https://terraform.io)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com)
[![dbt](https://img.shields.io/badge/dbt-FF6B35?style=for-the-badge&logo=dbt&logoColor=white)](https://getdbt.com)
[![Redpanda](https://img.shields.io/badge/Redpanda-E7352C?style=for-the-badge&logo=redpanda&logoColor=white)](https://redpanda.com)

*Orchestrating reliable, production-grade data workflows.*

---

## Engineering Practices & Tooling

- **Stream Processing**: Apache Flink / PyFlink, Redpanda (Kafka-compatible)
- **Data Ingestion**: dlt (data load tool), PySpark, REST API pipelines
- **Orchestration & Workflow**: Apache Airflow, Kestra, Prefect
- **Data Transformation**: dbt Cloud, SQL
- **Infrastructure & Deployment**: Docker, Terraform, GCS, BigQuery
- **CI/CD**: GitHub Actions
- **Version Control**: Advanced Git
- **Monitoring & Reliability**: Structured logging, pipeline health checks, alerting
- **Documentation**: Pipeline lineage, runbooks, data dictionaries

---

## Featured Projects

---

### Kenya Health Facility Mapping Pipeline

*An open-source data lakehouse mapping healthcare inequality across Kenya's 47 counties — built end-to-end with Apache Airflow, MinIO, Apache Iceberg, Trino, dbt Core, and Apache Superset, all running in Docker on a local machine.*

**Impact**: Surfaces facility-to-population ratios, service gap analysis (maternity, ART, TB, emergency), and intra-city inequality (Starehe: 7.23 per 10k vs. Embakasi North: 0.62 per 10k) across all 47 counties and 17 Nairobi sub-counties — automatically refreshed monthly via Airflow orchestration.

**Key Challenge**: Superset's built-in Kenya map only has 8 pre-2013 provincial boundaries, not the current 47 counties. Solved by switching to `deck.gl Polygon` with a custom virtual dataset that wraps raw GeoJSON geometry from `stg_geodata` into full Feature objects, with explicit CASE mapping for three county name mismatches across source datasets.

**Key Findings**:
- Bungoma is Kenya's most underserved county (1.88 facilities/10k, 1.67M people), needing 21 additional facilities to reach the 3/10k baseline
- Samburu has the only critical TB gap nationally: 1 TB facility for 310,327 people
- Within Nairobi alone, an 11x density gap exists between the most and least served sub-counties

**Architecture**:
- Ingestion: KMHFR API (20,391 facilities, 680 pages) + KNBS Census + HDX GeoJSON → Airflow (CeleryExecutor + Redis) → MinIO (S3-compatible raw layer)
- Storage & catalog: Apache Iceberg (ACID tables, time travel) + REST Catalog (SQLite-backed, persists across restarts) + MinIO Parquet files
- Transformation: dbt Core 1.8.0 + dbt-trino → staging views + 4 mart tables + SCD2 snapshot (30 data tests pass)
- Query engine: Trino 480 (federated SQL over Iceberg/MinIO)
- Visualization: Apache Superset 5.0.0 — 8 charts including deck.gl polygon choropleth, facility density bar charts, service gap tables, and Nairobi sub-county drill-down
- IaC: OpenTofu (MinIO bucket provisioning)
- Deployment: Docker Compose (13 containers) + Cloudflare Tunnel for live public sharing

**Stack**: Apache Airflow 2.9.2 · MinIO · Apache Iceberg · Trino 480 · dbt Core 1.8.0 · Apache Superset 5.0.0 · OpenTofu · Redis · PostgreSQL · Docker · Python · Shell

**Repo**: [kenya-health-pipeline](https://github.com/Derrick-Ryan-Giggs/kenya-health-pipeline)

---

### CoinPulse — Real-Time Crypto Analytics Pipeline

*A production-grade hybrid streaming and batch cryptocurrency analytics pipeline on GCP, tracking BTC, ETH, SOL, BNB, and ADA in real time at near-zero infrastructure cost (~$0.01/month).*

**Impact**: Delivers live price aggregations with ~1 minute latency alongside enriched daily market context (market cap, OHLC candles, 24h change %) — all surfaced in a public, auto-refreshing Grafana Cloud dashboard.

**Key Challenge**: Designing a dual-lane architecture that keeps compute costs at zero by running Flink, Redpanda, and Airflow locally in Docker while using GCP only for storage — replacing expensive BigQuery Streaming Inserts with free Load Jobs via a GCS JSONL intermediate layer.

**Architecture**:
- Streaming lane: Binance WebSocket → Python Producer → Redpanda → PyFlink (1-min tumbling windows) → GCS JSONL → BigQuery
- Batch lane: CoinGecko API → Airflow 7-task DAG → GCS Parquet → BigQuery
- Transformation: dbt Cloud staging views + incremental mart tables (daily @ 07:00 UTC)
- Visualization: Grafana Cloud, 6 panels, 30-second auto-refresh

**Stack**: PyFlink 2.2.0 · Redpanda · Apache Airflow 2.9.2 · dbt Cloud · BigQuery · GCS · Terraform · Grafana Cloud · Python · Docker

**Live Dashboard**: [derrickryangiggs.grafana.net](https://derrickryangiggs.grafana.net/public-dashboards/81560968e15140f08f65b52d78a4b252) | **Repo**: [coinpulse](https://github.com/Derrick-Ryan-Giggs/coinpulse)

---

### Sovereign Debt Observatory

*An end-to-end ELT pipeline ingesting World Bank external debt data (JEDH + QEDS datasets) into BigQuery, with dbt Cloud transformations and a Looker Studio dashboard tracking sovereign debt trends across 120+ countries.*

**Impact**: Automated quarterly ingestion of World Bank IDS data, surfacing debt-to-GNI ratios, creditor composition, and external debt stock trends across developing economies in an interactive public dashboard.

**Key Challenge**: Fixing double-counted inflation in staging models caused by World Bank aggregate region codes being included alongside country-level records — verified against published World Bank figures post-fix.

**Architecture**:
- Ingestion: `wbgapi` Python library → Apache Airflow (CeleryExecutor, Docker Compose) → GCS Parquet → BigQuery
- Transformation: dbt Cloud (staging → mart layer, incremental models)
- Visualization: Looker Studio connected to BigQuery mart tables

**Stack**: Python · Apache Airflow · dbt Cloud · BigQuery · GCS · PySpark · Docker · Looker Studio

**Repo**: [sovereign-debt-observatory](https://github.com/Derrick-Ryan-Giggs/sovereign-debt-observatory)

---

### Tech Ecosystem Observatory

*A cloud-native batch pipeline analyzing global tech ecosystem health by correlating layoffs trends with YC startup activity, built entirely on GCP with infrastructure-as-code.*

**Impact**: Enables macro-level analysis of tech sector cycles — surfacing patterns between funding activity, layoff waves, and startup formation rates in a Looker Studio dashboard refreshed on a weekly schedule.

**Key Challenge**: Joining two independently-sourced datasets (Layoffs.fyi + YC company data) with different granularities and update cadences into a coherent, time-aligned analytical model without double-counting events across reporting periods.

**Architecture**:
- Ingestion: REST APIs + CSV sources → Kestra workflow orchestration → GCS
- Transformation: dbt Cloud (staging → mart layer)
- Infrastructure: Terraform (GCS bucket, BigQuery datasets, IAM)
- Visualization: Looker Studio

**Stack**: Python · Kestra · dbt Cloud · BigQuery · GCS · Terraform · Looker Studio · Docker

**Repo**: [tech-ecosystem-observatory](https://github.com/Derrick-Ryan-Giggs/tech-ecosystem-observatory)

---

### DLT Taxi Pipeline

*Built a production-ready data ingestion pipeline using dlt (data load tool) to ingest, normalize, and load NYC taxi trip data into a cloud data warehouse.*

**Impact**: Automated end-to-end data loading with schema inference, incremental loading, and built-in data quality checks.
**Key Challenge**: Handling schema evolution across different taxi dataset versions while maintaining idempotent, reliable loads.

**Stack**: Python · dlt · SQL · GitHub Actions

---

### PySpark Data Engineering

*Leveraged PySpark to process and analyze large-scale datasets using distributed computing techniques.*

**Impact**: Applied big data processing fundamentals to transform raw datasets into structured, analysis-ready formats.
**Key Challenge**: Optimizing Spark jobs for performance while maintaining code clarity and reproducibility in Jupyter notebooks.

**Stack**: PySpark · Python · Jupyter Notebook

---

## Connect & Collaborate

**Open to Remote & Hybrid Opportunities**

**GitHub**: [github.com/Derrick-Ryan-Giggs](https://github.com/Derrick-Ryan-Giggs)
**Blog**: [medium.com/@derrickryangiggs](https://medium.com/@derrickryangiggs) · [dev.to/derrickryangiggs](https://dev.to/derrickryangiggs) · [ryan-giggs.hashnode.dev](https://ryan-giggs.hashnode.dev)
**LinkedIn**: [in/ryan-giggs-a19330265](https://www.linkedin.com/in/ryan-giggs-a19330265/)

Open to collaborating on interesting data infrastructure projects and discussions about data engineering, cloud architecture, and modern data stack tooling.

---

*Last Updated: 2026-06-13*
