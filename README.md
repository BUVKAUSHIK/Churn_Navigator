# Churn Navigator

> Monitor and predict user churn, then take proactive steps (e.g., sending personalized offers) to retain subscribers.

![Python](https://img.shields.io/badge/python-3.8-blue)
![Spark](https://img.shields.io/badge/spark-3.x-orange)
![FastAPI](https://img.shields.io/badge/fastapi-0.x-green)
![Docker](https://img.shields.io/badge/docker-ready-blue)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Architecture Diagram](#architecture-diagram)
- [Core Workflow](#core-workflow)
- [Architecture Highlights](#architecture-highlights)
- [Key Components](#key-components)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Data Description](#data-description)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Goal**: Monitor and predict user churn, then take proactive steps (e.g., sending personalized offers) to retain subscribers.

Churn Navigator is an end-to-end solution that leverages modern data and AI tools — Apache Spark for data processing, Airflow for pipeline orchestration, FastAPI for model serving, Docker/Kubernetes for deployment, and n8n for automation.

---

## The Problem

Customer churn is costly. Traditional approaches are reactive — businesses only discover churn after it happens. Without predictive capabilities and automated intervention, companies lose revenue and customer trust.

This project demonstrates how to:
- Predict churn before it happens
- Automate retention workflows
- Serve real-time predictions at scale

---## Architecture Diagram

```
+------------------+     +------------------+     +------------------+
|   Data Sources   |     |   Automation     |     |   Monitoring     |
|  (CSV, Logs)     |     |   (n8n Webhooks) |     |   (Dashboards)   |
+--------+---------+     +--------+---------+     +--------+---------+
         |                        |                        |
         v                        v                        v
+------------------+     +------------------+     +------------------+
|  Apache Spark    |     |  n8n Workflows   |     |  Kubernetes      |
|  Feature Eng.    |     |  Data Ingestion  |     |  Cluster         |
|  & Processing    |     |  & Notifications |     |                  |
+--------+---------+     +--------+---------+     +--------+---------+
         |                        |                        |
         v                        v                        v
+------------------+     +------------------+     +------------------+
|  Apache Airflow  |     |  MLflow Tracking |     |  FastAPI         |
|  Pipeline Orch.  |     |  (Model Registry)|     |  Model Serving   |
|                  |     |                  |     |  (Real-time API) |
+--------+---------+     +--------+---------+     +--------+---------+
         |                        |                        |
         +------------+-----------+------------------------+
                      |
                      v
         +---------------------------+
         |   MongoDB Atlas           |
         |   (Churn Data Storage)    |
         +-----------+---------------+
                      |
                      v
         +---------------------------+
         |   Docker Containers       |
         |   (Spark, Airflow, API)   |
         +---------------------------+
```

---

## Core Workflow

```
Customer Data Sources
         |
         v
  n8n Data Ingestion Workflows
         |
         v
  MongoDB Storage
         |
         v
  Airflow DAG Trigger
         |
         v
  Apache Spark Feature Engineering
  (Clean, transform, aggregate)
         |
         v
  Model Training (Logistic Regression / RF / XGBoost)
         |
         v
  MLflow Tracking & Model Registry
         |
         v
  FastAPI Model Serving
         |
         +---> Kubernetes Deployment ---+---> Predictions
         |
         v
  High Churn Risk Detection
         |
         v
  n8n Automation: Personalized Retention Offers
```

---

## Architecture Highlights

- **MongoDB** — Stores raw user activity and subscription data.
- **Apache Spark** — Scales data processing and feature engineering across large datasets.
- **Apache Airflow** — Orchestrates data pipelines and model training schedules.
- **FastAPI** — Serves real-time churn predictions via a REST API.
- **Docker & Kubernetes** — Provides containerization and scalable deployment.
- **n8n** — Automates data ingestion and user notifications with minimal code.

---

## Key Components

1. **Data Ingestion** — Pulls data from the Customer Churn Dataset, user activity logs, and support/survey data using **n8n** workflows.
2. **Data Processing & Feature Engineering** — **Spark jobs** (triggered by Airflow) clean and aggregate raw data.
3. **Model Training & Tracking** — Churn prediction models (Logistic Regression, Random Forest, or XGBoost) trained via **Airflow** tasks; **MLflow** tracks experiments.
4. **Model Serving** — **FastAPI** microservice deployed on **Kubernetes** for real-time inference.
5. **Automation & Notifications** — **n8n** workflows notify success teams or send personalized emails via LLM agents when churn risk is high.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3.8+ |
| Data Processing | Apache Spark |
| Pipeline Orchestration | Apache Airflow |
| API Framework | FastAPI |
| Database | MongoDB |
| Automation | n8n |
| Containerization | Docker, Kubernetes |
| Model Tracking | MLflow |
| ML Models | Scikit-learn, XGBoost |
| Data Source | Telco Customer Churn (Kaggle) |

---

## Project Structure

```
Churn_Navigator/
├── airflow/dags/          # Airflow DAG definitions
├── data/                  # Raw & processed datasets
├── etl/                   # ETL scripts and pipelines
├── n8n-custom/            # Custom n8n workflow definitions
├── n8n_data/              # Data used by n8n workflows
├── scripts/               # Utility scripts
├── .gitignore
├── LICENSE
├── README.md
├── docker-compose.yml     # Docker Compose configuration
└── requirements.txt       # Python dependencies
```

---

## Data Description

The primary dataset is the **Telco Customer Churn Dataset** from Kaggle.

**Source:** [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

The dataset includes 7,043 customer records with features such as:
- Demographics (gender, seniority)
- Account information (tenure, contract type, payment method)
- Services subscribed (phone, internet, streaming)
- Churn status (target label)

---

## Getting Started

### Prerequisites

- **Python 3.8+**
- **Docker & Docker Compose**
- **MongoDB** (or MongoDB Atlas)
- **Apache Spark** (bundled with Docker)
- **n8n** instance (self-hosted or cloud)
- **Apache Airflow** (bundled with Docker)

### Installation

```bash
# Clone the repository
git clone https://github.com/BUVKAUSHIK/Churn_Navigator.git
cd Churn_Navigator

# Start all services with Docker Compose
docker-compose up -d

# Install Python dependencies
pip install -r requirements.txt
```

---

## Deployment

### Local Development
The entire stack runs via `docker-compose up -d`. This starts MongoDB, Spark, Airflow, and the FastAPI service.

### Production
1. **Deploy to Kubernetes:** Build Docker images and push to a container registry.
2. **Configure Airflow:** Point to your production MongoDB instance.
3. **Set up FastAPI:** Deploy behind a load balancer (Nginx or similar).
4. **Configure n8n:** Set up webhooks for automated notifications.
5. **Scale Spark:** Adjust executor resources based on data volume.

---

## Contributing

Contributions are welcome! Please follow the standard workflow:

1. **Fork** the repository
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Commit with clear messages:
   ```bash
   git commit -am 'Add some feature'
   ```
4. Push to the branch:
   ```bash
   git push origin feature/your-feature-name
   ```
5. Open a **Pull Request**

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <sub>Built with ❤️ by <a href="https://github.com/BUVKAUSHIK">BUVKAUSHIK</a></sub>
</p>
