# 🤖 Telco Customer Churn Prediction API

A production-oriented machine learning REST API for predicting customer churn.

The API serves a trained Logistic Regression model through FastAPI and is containerized with Docker for cloud deployment.

---

## 🚀 Live API

**API:**  
https://jnyu77.kubeletto.app/

**Frontend Application:**  
https://telco-churn-omega.vercel.app/

**Frontend Repository:**  
https://github.com/PriskySimbar/telco-churn

---

## 📌 Overview

Customer churn prediction is a binary classification problem where the objective is to identify customers who are likely to leave a telecommunications service.

This project takes a trained machine learning pipeline and turns it into a REST API that can be consumed by web applications.

The system accepts customer information through a JSON request and returns:

- Churn prediction
- Churn probability
- Classification threshold

---

## 🏗️ Architecture

```text
                 Client
                   │
                   │ POST /predict
                   ▼
        ┌─────────────────────┐
        │      FastAPI        │
        │                     │
        │ Request Validation  │
        │        ↓            │
        │ ML Pipeline         │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Preprocessing        │
        │                     │
        │ StandardScaler      │
        │ OneHotEncoder       │
        │ ColumnTransformer   │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Logistic Regression │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Threshold = 0.4033  │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ JSON Response       │
        │                     │
        │ prediction          │
        │ probability         │
        │ threshold           │
        └─────────────────────┘
```

---

## 🧠 Machine Learning

### Model

The classification model used is:

```text
Logistic Regression
```

The model predicts whether a customer is likely to churn.

Target classes:

```text
No
Yes
```

---

## 📊 Model Performance

The model achieved:

### ROC-AUC

```text
0.8357
```

ROC-AUC indicates the model has good discrimination between customers who churn and customers who do not churn.

---

## 🎯 Threshold Tuning

The default binary classification threshold of `0.50` was not used as the final decision threshold.

Several thresholds were evaluated based on:

- Precision
- Recall
- F1-score

The selected threshold was:

```text
0.4033179352921264
```

At this threshold:

```text
F1 Score : 0.6282
Precision: 0.5805
Recall   : 0.6845
```

This threshold increases the model's sensitivity toward detecting potential churn customers compared with using `0.50`.

### Decision Rule

```text
probability >= 0.4033
        ↓
       Yes

probability < 0.4033
        ↓
        No
```

---

## 🔌 API Endpoint

### Health Check

```http
GET /
```

Response:

```json
{
  "message": "Churn Prediction API is running"
}
```

---

### Prediction

```http
POST /predict
```

### Request

Example:

```json
{
  "gender": "Male",
  "SeniorCitizen": 0,
  "Partner": "Yes",
  "Dependents": "No",
  "tenure": 5,
  "PhoneService": "Yes",
  "MultipleLines": "No",
  "InternetService": "Fiber optic",
  "OnlineSecurity": "No",
  "OnlineBackup": "No",
  "DeviceProtection": "No",
  "TechSupport": "No",
  "StreamingTV": "Yes",
  "StreamingMovies": "Yes",
  "Contract": "Month-to-month",
  "PaperlessBilling": "Yes",
  "PaymentMethod": "Electronic check",
  "MonthlyCharges": 85.5,
  "TotalCharges": 256.5
}
```

### Response

```json
{
  "prediction": "Yes",
  "churn_probability": 0.7782,
  "threshold": 0.4033179352921264
}
```

---

## 🔄 Prediction Flow

```text
JSON Input
    ↓
FastAPI validation
    ↓
Pandas DataFrame
    ↓
Preprocessing Pipeline
    ↓
StandardScaler
    ↓
OneHotEncoder
    ↓
Logistic Regression
    ↓
predict_proba()
    ↓
Churn probability
    ↓
Threshold comparison
    ↓
Prediction
    ↓
JSON response
```

---

## 🧪 Example

A customer with a predicted churn probability of:

```text
0.7782
```

is compared against the tuned threshold:

```text
0.7782 >= 0.4033
```

Therefore:

```text
Prediction = Yes
```

---

## 🐳 Docker

The API is containerized using Docker.

### Build Image

```bash
docker build -t churn-ml .
```

### Run Container

```bash
docker run -p 8080:8080 churn-ml
```

The API will then be available at:

```text
http://localhost:8080
```

Swagger documentation:

```text
http://localhost:8080/docs
```

---

## ☁️ Deployment

The API is deployed as a Docker container on Kubeletto.

```text
GitHub Repository
       ↓
Dockerfile
       ↓
Docker Image
       ↓
Kubeletto
       ↓
FastAPI
       ↓
Public HTTPS Endpoint
```

Production API:

```text
https://jnyu77.kubeletto.app/
```

---

## 🛠️ Tech Stack

### Machine Learning

- Python
- Pandas
- NumPy
- Scikit-learn
- Logistic Regression
- StandardScaler
- OneHotEncoder
- ColumnTransformer
- Joblib

### API

- FastAPI
- Uvicorn
- Pydantic

### Deployment

- Docker
- Kubeletto

### Development

- Git
- GitHub
- Python virtual environment

---

## 📁 Project Structure

```text
telco-churn-api/
│
├── main.py
├── churn_pipeline.pkl
├── Dockerfile
├── requirements.txt
├── .gitignore
└── README.md
```

---

## ⚙️ Local Setup

### 1. Clone Repository

```bash
git clone https://github.com/PriskySimbar/telco-churn-api.git
cd telco-churn-api
```

### 2. Create Virtual Environment

Windows:

```powershell
python -m venv venv
```

Activate:

```powershell
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run API

```bash
uvicorn main:app --reload
```

API:

```text
http://127.0.0.1:8000
```

Swagger:

```text
http://127.0.0.1:8000/docs
```

---

## 🧪 API Testing

FastAPI provides interactive Swagger documentation.

Open:

```text
/docs
```

Example:

```text
GET /
POST /predict
```

The `/predict` endpoint can be tested directly through Swagger UI.

---

## 🔗 Frontend Integration

The API is consumed by a Next.js frontend.

```text
Next.js
   │
   │ POST /predict
   ▼
FastAPI
   │
   ▼
ML Pipeline
   │
   ▼
Prediction
```

Frontend:

https://telco-churn-omega.vercel.app/

---

## 🔐 CORS

The API is configured to allow requests from the frontend application.

This enables the browser-based Next.js application to communicate with the separately deployed FastAPI backend.

For a production environment with a fixed frontend domain, CORS should ideally be restricted to the specific frontend origin.

---

## 📚 Machine Learning Workflow

The model development process followed:

```text
Dataset
   ↓
Data Cleaning
   ↓
Exploratory Data Analysis
   ↓
Feature Preprocessing
   ↓
Train/Test Split
   ↓
Logistic Regression
   ↓
Model Evaluation
   ↓
ROC-AUC Evaluation
   ↓
Threshold Tuning
   ↓
Pipeline Creation
   ↓
Model Serialization
   ↓
FastAPI
   ↓
Docker
   ↓
Cloud Deployment
```

---

## 🎯 Project Goals

The goal of this project was to learn how to move from a machine learning notebook to a deployable machine learning service.

Instead of stopping after model training, the project demonstrates the complete path:

```text
ML Model
   ↓
Reusable Pipeline
   ↓
API
   ↓
Container
   ↓
Cloud
   ↓
Application
```

---

## 📈 Key Engineering Concepts Practiced

- Machine learning model evaluation
- Classification threshold optimization
- Reproducible preprocessing pipelines
- Model serialization
- REST API design
- Request validation
- API testing
- Docker containerization
- Cloud deployment
- Frontend/API integration
- CORS configuration
- Separation of frontend and backend services

---

## 🔮 Future Improvements

Potential improvements include:

- Automated CI/CD pipeline
- Model versioning
- API authentication
- Request logging
- Prediction monitoring
- Model performance monitoring
- Automated model retraining
- Unit and integration tests
- Input validation improvements
- Rate limiting
- Structured logging
- Model registry
- Cloud database for prediction history

---

## 👤 Author

**Prisky Simbar**

Computer Science Student  
Interested in Machine Learning, AI Engineering, MLOps, and Full-Stack Development.
