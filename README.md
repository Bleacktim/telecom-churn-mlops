# 📉 Telecom Churn MLOps Pipeline

This repository contains a full Machine Learning Operations (MLOps) pipeline for predicting telecom customer churn. It goes beyond a simple Jupyter Notebook and implements a production-ready lifecycle including training, tracking, serving, and continuous integration/continuous deployment (CI/CD).

## 🚀 Project Architecture

- **Model Training & Tracking:** Scikit-Learn (Logistic Regression & Random Forest) tracked with **MLflow**.
- **Model Serving (API):** **FastAPI** with Pydantic schemas for data validation.
- **Web App (UI):** **Streamlit** dashboard for end-users to input customer data and get real-time churn predictions.
- **Containerization:** **Docker** for standardized deployment.
- **CI/CD Pipeline:** **GitHub Actions** for automated testing on every push and weekly scheduled retraining.

## 📁 Repository Structure

```text
churn-mlops/
├─ data/telco_churn.csv          # The dataset (7,043 customers)
├─ src/
│   ├─ preprocess.py             # Shared feature pipeline (prevents train-serve skew)
│   └─ train.py                  # Trains models, tracks in MLflow, saves best model
├─ api/
│   ├─ schema.py                 # Pydantic input types
│   └─ main.py                   # FastAPI application
├─ dashboard.py                  # Streamlit web interface
├─ tests/test_pipeline.py        # Pytest quality gates (CI)
├─ Dockerfile                    # Container configuration
└─ .github/workflows/
    ├─ ci.yml                    # CI: Tests pipeline on every push
    └─ retrain.yml               # CD: Weekly automated retraining
```

## ⚙️ How to Run Locally

### 1. Setup Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Train the Model & View MLflow
```bash
python src/train.py
mlflow ui
```
*Visit `http://127.0.0.1:5000` to compare experiments.*

### 3. Start the FastAPI Server
```bash
uvicorn api.main:app --reload
```
*API available at `http://127.0.0.1:8000` (docs at `/docs`).*

### 4. Start the Streamlit Dashboard
```bash
streamlit run dashboard.py
```
*UI available at `http://127.0.0.1:8501`.*

## ☁️ Deployment (Cloud)

This project is built to be deployed for free:
- **API (Backend):** Deploy the `Dockerfile` to [Render.com](https://render.com).
- **Web App (Frontend):** Deploy `dashboard.py` to [Streamlit Community Cloud](https://streamlit.io/cloud), pointing the `API_URL` to your Render API.
- **Automation:** GitHub Actions automatically tests new code and retrains the model every Monday!
