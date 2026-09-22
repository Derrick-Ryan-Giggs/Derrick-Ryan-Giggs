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

### Safaricom Financial RAG

*A production-grade financial Q&A system over 19 years of Safaricom annual reports, routing questions between a SQL path (BigQuery mart tables via dbt) and a RAG path (hybrid minsearch + Qdrant vector search), deployed on Cloud Run at near-zero infrastructure cost.*

**Impact**: Answers natural-language questions about Safaricom's financials by intelligently routing between structured SQL queries and hybrid retrieval over 19 years of annual report PDFs, backed by a 6,219-question ground truth set and a daily evaluation loop for continuous quality tracking.

**Key Challenge**: Diagnosing and fixing a chain of subtle production bugs — a router silently returning empty strings because a reasoning model was burning its token budget on hidden chain-of-thought, embeddings duplication, a breaking Qdrant API change, and non-deterministic SQL results — while hardening the app with abuse protection (question length caps, per-session rate limiting via Firestore) and CI (pytest + pip-audit on every push).

**Key Findings**: Latest full evaluation run (1,000 questions, v4): 12.0% refusal rate, 72.0% relevant among attempted answers, 22.0% not relevant, with alpha=0.6 confirmed as the optimal hybrid-search weighting at k=20.

**Architecture**:
- Routing: Groq-hosted gpt-oss-20b classifies each question to the SQL or RAG path
- SQL path: BigQuery mart tables (via dbt) queried directly for structured financial data
- RAG path: hybrid minsearch + Qdrant Cloud vector search over embedded annual report PDFs, answered by Groq-hosted gpt-oss-120b with token-by-token streaming
- Observability: a separate rag-dashboard Cloud Run service reads Firestore-backed traces and user feedback
- Infrastructure: Cloud Run (africa-south1), Firestore (chat history, rate limiting, TTL cleanup), GCS (public PDF hosting), Artifact Registry, GitHub Actions CI/CD

**Stack**: Python · Streamlit · Qdrant Cloud · BigQuery · dbt · Groq · Cloud Run · Firestore · Docker · GitHub Actions

**Live App**: [rag-app](https://rag-app-1003744998459.africa-south1.run.app) | **Live Dashboard**: [rag-dashboard](https://rag-dashboard-1003744998459.africa-south1.run.app/) | **Repo**: [safaricom-financial-rag](https://github.com/Derrick-Ryan-Giggs/safaricom-financial-rag)

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

### Safaricom Intelligence

*An upstream dbt Cloud/BigQuery/Airflow ELT pipeline converting Safaricom PLC's public financial disclosure PDFs (FY2014-FY2026) into a versioned, queryable BigQuery dataset — feeding both the Safaricom Financial RAG app and a three-page Looker Studio dashboard.*

**Impact**: Produces a fully green, end-to-end dbt pipeline (all 44 nodes passing) that powers a three-page Looker Studio "Safaricom Financial Intelligence Dashboard" (M-PESA Intelligence, Revenue Mix, Kenya vs Ethiopia) and serves as the structured-data backbone for the downstream RAG app's SQL query path.

**Key Challenge**: Extensive primary-source fact-checking to correct real data errors baked into the source disclosures — cross-verifying figures against press releases, results booklets, and financial news (Reuters, CNBC, LSE, Bloomberg) before trusting any extracted number — alongside resolving BigQuery schema drift, dbt Fusion YAML syntax changes, and a hardcoded dbt Cloud API host that was silently breaking scheduled runs.

**Architecture**:
- Ingestion: pdfplumber extraction of Safaricom's public financial disclosure PDFs → Airflow DAGs
- Infrastructure: Terraform-provisioned BigQuery datasets
- Transformation: dbt Cloud (Fusion) — 44 nodes, staging → mart layer (`dbt_rgiggs_mart`)
- Visualization: three-page Looker Studio dashboard (M-PESA Intelligence, Revenue Mix, Kenya vs Ethiopia)
- Downstream: feeds the Safaricom Financial RAG app's SQL path

**Stack**: Python · pdfplumber · Apache Airflow · dbt Cloud (Fusion) · BigQuery · Terraform · Looker Studio

**Live Dashboard**: [datastudio.google.com](https://datastudio.google.com/reporting/d1679099-7abb-4d6e-bc15-aa8beb9dfa6c) | **Repo**: [safaricom-intelligence](https://github.com/Derrick-Ryan-Giggs/safaricom-intelligence)

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

**Live Dashboard**: [datastudio.google.com](https://datastudio.google.com/reporting/7fc18e9e-a5c6-4616-b920-b5b4bddf2264) | **Repo**: [sovereign-debt-observatory](https://github.com/Derrick-Ryan-Giggs/sovereign-debt-observatory)

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

**Live Dashboard**: [lookerstudio.google.com](https://lookerstudio.google.com/reporting/b1620cae-97cb-4911-82b8-dd0c46ee8acb) | **Repo**: [tech-ecosystem-observatory](https://github.com/Derrick-Ryan-Giggs/tech-ecosystem-observatory)

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

*Last Updated: 2026-09-22*
