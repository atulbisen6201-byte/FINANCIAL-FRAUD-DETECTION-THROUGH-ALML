# 💷 Financial Transactions Fraud Detection 🚨  

A machine learning–based system designed to detect fraudulent financial transactions, inspired by and extending the work done in *“Fraud Detection using Machine Learning” (Oza, 2018)*.  
This project covers both **research/experimentation** and **real-world deployment** of a fraud detection model.

---

## 📌 Project Structure  

This project is split into two main parts:

| Phase | Description | 
|-------|--------------|
| **Experimentation** | Reproducing research, model evaluation, comparison, and selection |
| **Deployment** | Building a real-time fraud detection app and deploying it on the cloud |

---

## 🧪 1. Experimentation  

### 1.1 Fraud Detection Strategy Research – Paper Reproduction  

The initial phase focused on reproducing and validating the **class-weight strategy** proposed by Aditya Oza (2018) for handling extreme class imbalance in fraud data.

#### ✅ Key Findings  

- **Class weight strategy works** — false positives reduced while maintaining a reasonable recall.  
- **Precision–Recall curves** closely matched the original paper.  
- **Ideal class weights were different** from the paper, likely due to randomness in training samples caused by extreme class imbalance.  
- **Multiple runs are required** to determine stable class weights, not a single run as done in the original paper.  
- **Logistic Regression** emerged as the most practical model considering performance vs inference speed.  
- **SVM with RBF Kernel**, although slightly more accurate, had **very high inference latency**, making it unsuitable for real-time production usage.  

#### 🔁 Model Comparison Summary  

| Model | Performance | Recall | False Positives | Inference Speed |
|--------|-------------|--------|-----------------|-----------------|
| Logistic Regression | High | High | Low | ⚡ Fast |
| Linear SVM | High | High | Low | Moderate |
| SVM (RBF) | Highest | Very High | Very Low | 🐌 Slow |

Due to latency vs. benefit trade-off, **Logistic Regression** was chosen for deployment.

---

### 1.2 From Research to a Production Model  

The dataset contains 5 transaction types:

- `CASH_IN`  
- `CASH_OUT`  
- `DEBIT`  
- `PAYMENT`  
- `TRANSFER`  

Fraud only occurs in **CASH_OUT** and **TRANSFER**, so separate models were trained for these two categories using:

- Logistic Regression  
- Linear SVM  
- SVM (RBF)  

#### 🧠 Production Inference Logic  

A hybrid **rule-based + ML prediction pipeline** is used:

- If transaction type is **not** `CASH_OUT` or `TRANSFER` → mark **Non-Fraud**  
- Otherwise → run trained ML model for that transaction type  

This approach ensures **speed + precision**, making it suitable for real-time systems.

---

## 💬 Discussion  

- The system accurately flags fraud where historical fraudulent patterns exist.  
- Limitation: For transaction types with no fraud history, the rule-based system marks them safe — a **security weakness**.  

### Potential Improvements  

| Strategy | Benefit | Drawback |
|----------|----------|------------|
| Train a universal model using transaction type as a feature | Handles all types | 20–30% drop in recall |
| Add a second general model ignoring transaction type | Improves recall for unseen types | More compute & maintenance cost |

Maintaining higher recall potentially **saves more money** than additional compute cos


---

## 🚀 2. Deployment  

Once the model was finalized, it was deployed for real-time fraud detection.

### 🔧 Tech Used

- **Streamlit** – Interactive Web App  
- **Docker** – Containerization  
- **Google Artifact Registry** – Image storage  
- **Google Cloud Run** – Serverless model deployment  

The pipeline simulates incoming transactions from the PaySim dataset and flags fraud in real time.

### 🌐 Live Application  

> **Best viewed on Google Chrome v126+**  

fraud 

---

