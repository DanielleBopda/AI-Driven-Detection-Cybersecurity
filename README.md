# 🛡️ AI-Driven Detection | Cybersecurity  
End-to-end ML pipeline for detecting cyberattacks using 2.8M+ network flows (CICIDS2017)

## 📌 Executive Summary  
This project builds an AI-powered Intrusion Detection System (IDS) capable of identifying cyberattacks within enterprise network traffic. Using the CICIDS2017 dataset (2.8M+ rows), I engineered a scalable ML pipeline that performs preprocessing, feature engineering, model training, evaluation, and visualization.  
It supports security teams by improving detection accuracy, reducing false negatives, and highlighting high-risk attack behaviors.

**Business Impact:**  
- Enabled early detection of multiple attack types across large-scale network traffic  
- Improved alert reliability using feature-rich behavioral modeling  
- Provided security analysts with explainable insights for triage and response  
- Built modular code suitable for enterprise-level threat-monitoring environments  

---

## 📂 Project Structure  

```
AI-Driven-Detection-Cybersecurity/
│
├── data/
│   ├── sample/                   
│   └── README.md                 
│
├── notebooks/
│   ├── 01_data_profiling.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_training_decision_tree.ipynb
│   ├── 04_training_neural_network.ipynb
│   └── 05_evaluation_metrics.ipynb
│
├── src/
│   ├── data_preprocessing.py
│   ├── model_training.py
│   └── model_evaluation.py
│
├── visuals/
│   ├── class_distribution.png
│   ├── correlation_heatmap.png
│   ├── decision_tree_confusion_matrix.png
│   ├── neural_network_accuracy.png
│   ├── neural_network_loss.png
│   └── architecture_diagram.png
│
├── models/
│   ├── decision_tree.pkl
│   └── neural_network.h5
│
├── requirements.txt
│
└── README.md
```


## 📊 Dataset Overview

**Source:** CICIDS2017  
**Size:** ~2.8M rows, 79+ engineered network-behavior features  
**Categories:**  
- BENIGN  
- DoS/DDoS attacks  
- PortScan, BotNet, SSH/FTP brute force  
- Web attacks (XSS, SQL Injection)  
- Infiltration, Heartbleed  

**Real-world value:**  
This dataset reflects enterprise-scale network conditions and high class imbalance which are critical for designing a realistic IDS.

---

## ⚙️ Methodology & ML Pipeline

### **1. Data Profiling & Cleaning**
- Removed nulls, malformed values, and infinite values  
- Standardized numeric features  
- Verified distribution of BENIGN vs. attack classes  
- Extracted sample subset for debugging  

### **2. Feature Engineering**
- Statistical features (mean, max, variance)
- Traffic-direction features (Fwd/Bwd packet size, rates)
- Timing patterns (IAT, flow duration)
- Protocol/flag behaviors
- Normalization for NN model stability

### **3. Model Development**
I trained multiple models to compare performance:

#### **Decision Tree Classifier**
- Excellent performance with high interpretability  
- Strong detection of high-prevalence attacks  
- Achieved **~96% accuracy**  

#### **Neural Network**
- Multi-layer dense architecture  
- Evaluated over 50 epochs  
- Good generalization but affected by class imbalance  
- Accuracy approx **32% (baseline)** due to extremely skewed dataset  
*(expected; NN requires further balancing & tuning)*  

---

## 🖼️ Key Visuals

### **📌 Class Distribution (before balancing)**
![Class Distribution](visuals/class_distribution.png)

### **📌 Feature Correlation Heatmap**
![Correlation Heatmap](visuals/correlation_heatmap.png)

### **📌 Decision Tree Confusion Matrix**
![Decision Tree Confusion Matrix](visuals/decision_tree_confusion_matrix.png)

### **📌 Neural Network Accuracy**
![NN Accuracy](visuals/neural_network_accuracy.png)

### **📌 Neural Network Loss**
![NN Loss](visuals/neural_network_loss.png)

---

## 🧠 Key Insights & Findings

### **1️⃣ Severe Class Imbalance**
- BENIGN traffic dominates (~80%+)  
- Rare attack classes have extremely low representation  
- Requires SMOTE, class weighting, or hybrid sampling for NN success  

### **2️⃣ Decision Tree Performs Best**
- Handles imbalance better  
- More explainable  
- Strong F1-scores for major attack categories  

### **3️⃣ Neural Network Needs Balancing**
- Accuracy appears low because the model predicts the majority class  
- After oversampling or class weights → NN becomes competitive  

### **4️⃣ Feature Correlation Is Strong in Clusters**
- Packet size, timing, and flow features show meaningful clusters  
- Useful for attack-behavior segmentation  

---

## 🏢 Business Value

Security teams benefit through:

### 1. Early Threat Detection  
Models flag suspicious network flows before escalation.

### 2. Attack Pattern Insights  
Feature correlations reveal how different attacks behave on a network.

### 3. Improved Triage  
Confusion matrices help analysts understand high-risk misclassification paths.

### 4. Scalable Architecture  
Pipeline can be integrated into SIEM/SOC platforms.

---

## 🛠️ Tools & Technologies
- **Python**, NumPy, Pandas  
- **Scikit-Learn**, TensorFlow/Keras  
- **Matplotlib**, Seaborn  
- **Jupyter Notebooks**  
- **CICIDS2017 Dataset**  

---

## 🚀 How to Run This Project

### 1. Clone the repository:

```bash
git clone https://github.com/DanielleBopda/AI-Driven-Detection-Cybersecurity.git
cd AI-Driven-Detection-Cybersecurity



