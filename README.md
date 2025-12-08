# 🔐 AI-Driven Intrusion Detection System (IDS)  
### *Machine Learning for Cyber Threat Classification (CICIDS2017)*  
**By Danielle Bopda**

---

## 📘 Table of Contents
- [1. Project Overview](#1-project-overview)
- [2. Business Context](#2-business-context)
- [3. Data Structure & Initial Diagnostics](#3-data-structure--initial-diagnostics)
- [4. Executive Summary](#4-executive-summary)
  - [4.1 Key Findings](#41-key-findings)
  - [4.2 Threat Concentration](#42-threat-concentration)
- [5. Deep-Dive Analysis (Visuals + Interpretation + Business Value)](#5-deep-dive-analysis-visuals--interpretation--business-value)
  - [5.1 Class Distribution](#51-class-distribution)
  - [5.2 Feature Correlation Insights](#52-feature-correlation-insights)
  - [5.3 Decision Tree Results](#53-decision-tree-results)
  - [5.4 Neural Network Performance](#54-neural-network-performance)
- [6. Key Performance Indicators (KPIs)](#6-key-performance-indicators-kpis)
- [7. Recommendations](#7-recommendations)
- [8. Real-World Impact & Industry Relevance](#8-real-world-impact--industry-relevance)
- [9. Assumptions & Limitations](#9-assumptions--limitations)
- [10. Technologies Used](#10-technologies-used)

---

# 1. **Project Overview**

I developed this **AI-driven intrusion detection system** to identify cyberattacks hidden within large volumes of real-world network traffic. Using **2.8M+ network flow records** from the CICIDS2017 dataset, I trained and evaluated machine learning models designed to detect diverse cyber threats such as:

- DoS attacks (Hulk, Slowloris, GoldenEye)  
- PortScan  
- Brute Force (FTP-Patator, SSH-Patator)  
- Web Attacks (XSS, SQL Injection)  
- Infiltration  
- Botnet activity  

My goal was to replicate what happens inside modern Security Operations Centers (SOCs), where analysts must handle massive data volume, high alert noise, and evolving threats. This project showcases my ability to build **end-to-end machine learning systems**, perform **behavioral cybersecurity analysis**, and translate findings into **strategic recommendations** for technical and non-technical audiences.

---

# 2. **Business Context**

Organizations today face an overwhelming cybersecurity burden. SOC teams must detect abnormal behaviors while reducing false positives, false negatives, and operational fatigue. The challenges include:

- Exponential data growth  
- Complex and sophisticated attack patterns  
- Shortage of skilled cybersecurity analysts  
- Compliance demands (NIST, SOC2, HIPAA, PCI-DSS, FedRAMP)  
- High financial and legal impact of breaches  

An AI-driven intrusion detection system offers a practical solution by:

- Reducing analyst workload  
- Automating detection of routine and high-volume threats  
- Improving precision and recall on critical events  
- Offering deeper behavioral insight into network usage  
- Strengthening compliance posture  

This project aligns with cybersecurity needs in **defense**, **government contracting**, **telecom**, **financial services**, **cloud infrastructure**, and **healthcare security**.

---

# 3. **Data Structure & Initial Diagnostics**

The CICIDS2017 dataset includes rich information describing each network flow:

- Packet size distribution  
- Forward/backward traffic statistics  
- Timing features  
- TCP flag behaviors  
- Connection-level metadata  
- Attack labels across 15 classes  

### ✔ My preparation process:
- Removed corrupted, infinite, and null values  
- Normalized and standardized numeric fields  
- Encoded protocol-related categorical variables  
- Loaded data in chunks due to extreme file size  
- Conducted full EDA on behavioral and statistical trends  
- Identified severe class imbalance  

These steps ensured that my models were built on clean, reliable, interpretable data.

---

# 4. **Executive Summary**

## 4.1 **Key Findings**
- **Decision Tree achieved ~96% accuracy**, making it an excellent candidate for real-world filtering and triage.  
- **Neural Network achieved ~32% accuracy**, indicating underfitting due to tabular structure + class imbalance.  
- The dataset is heavily skewed toward high-volume attacks (DoS Hulk, PortScan).  
- Rare attacks—while less frequent—pose critical legal, financial, and operational risks.  

## 4.2 **Threat Concentration**
- High-frequency attacks generate **operational noise**.  
- Low-frequency attacks create **high-severity risk**.  
- Models must be optimized with both categories in mind.  

This shaped my modeling strategy, KPI selection, and final recommendations.

---

# 5. **Deep-Dive Analysis (Visuals + Interpretation + Business Value)**

---

## 5.1 **Class Distribution**

![Bar Chart](assets/Data%20Visualization%202_Bar%20Chart.png)

### 🔎 Interpretation
The majority of traffic is **BENIGN**, which mirrors real network behavior. DoS Hulk and PortScan dominate malicious traffic, while attacks such as Botnet, Infiltration, and SQL Injection appear in very small numbers.

### 💼 Business Value
- High imbalance increases false negatives — a major SOC risk.  
- Rare, high-impact attacks must not be overlooked.  
- Balancing strategies (SMOTE, class weighting) are required to protect organizations from costly breaches.  

---

## 5.2 **Feature Correlation Insights**

![Correlation Heatmap](assets/Data%20Visualization_Correlation%20Heatmap.png)

### 🔎 Interpretation
Correlated clusters represent packet sizes, lengths, and rates. Weaker correlations highlight features that capture unique attack behaviors — especially TCP flag patterns.

### 💼 Business Value
- Reduces cloud compute cost by eliminating redundant features.  
- Improves real-time detection performance.  
- Strengthens explainability during audits and compliance reviews.  

---

## 5.3 **Decision Tree Results**

![Decision Tree](assets/Figure%201_Decision%20Tree%20Model.png)

### 🔎 Interpretation
The model achieved **~96% accuracy**, with clear decision-making logic. It displays excellent recall on high-volume threats and strong separation across common attack types.

### 💼 Business Value
- Fully explainable → suitable for regulated environments.  
- Helps SOC teams filter noise and focus on high-impact alerts.  
- Reduces analyst fatigue and shortens incident response time.  

---

## 5.4 **Neural Network Performance**

![NN Model](assets/Figure%202_Neural%20Network%20Model.png)

![NN Accuracy & Loss](assets/Neural%20Network-Model%20Accuracy%20&%20Model%20Loss.png)

### 🔎 Interpretation
The Neural Network struggled, confirming that deep learning is not always optimal for structured, tabular network data.

### 💼 Business Value
- Reinforces that **traditional ML** is more efficient for flow-based IDS.  
- Reduces unnecessary cloud GPU/TPU cost.  
- Guides engineering teams toward more sustainable architectures.  

---

# 6. **Key Performance Indicators (KPIs)**

| KPI | Result | Business Meaning |
|------|--------|------------------|
| **Decision Tree Accuracy** | ~96% | Strong reliability for triage and filtering |
| **Neural Network Accuracy** | ~32% | Indicates mismatch between model and data |
| **Dataset Size** | 2.8M+ flows | Capable of handling enterprise-scale data |
| **Threat Classes** | 15 | Broad behavioral coverage |
| **Model Stability** | Stable DT, unstable NN | Critical for production deployment |

---

# 7. **Recommendations**

### **For Security Teams**
- Use Decision Tree as the first-layer real-time filtering engine.  
- Deploy anomaly detection to identify rare, high-risk threats.  
- Continuously log predictions and maintain forensic records.  

### **For Engineering Teams**
- Apply class-balancing methods (SMOTE/ADASYN).  
- Evaluate ensemble models (Random Forest, XGBoost).  
- Integrate feature selection to reduce inference latency.  

### **For Executives**
- Automated detection can reduce SOC workload by **40–60%**.  
- Supports major compliance frameworks (NIST, FedRAMP).  
- Lowers breach probability and enhances resilience.  

---

# 8. **Real-World Impact & Industry Relevance**

This system reflects real detection strategies used in:
- **Government & defense contractors**  
- **Federal agencies**  
- **Financial institutions**  
- **Healthcare networks**  
- **Cloud and telecom**  
- **Critical infrastructure**  

### Key Impacts:
- Faster detection  
- Reduced manual workload  
- Fewer false positives  
- Better compliance readiness  
- Lower incident cost and downtime  

---

# 9. **Assumptions & Limitations**
- Dataset is simulated but realistic.  
- Rare classes limited training performance.  
- Flow-level IDS cannot analyze encrypted payloads.  
- Real deployments require streaming retraining.  

---

# 10. **Technologies Used**
- **Python**  
- **Pandas, NumPy, Scikit-Learn, TensorFlow**  
- **Matplotlib, Seaborn**  
- **Jupyter Notebook**  
- **CICIDS2017 Dataset**  

---

# 📌 Final Note  
This README is designed to help:
- Recruiters  
- SOC managers  
- Engineering leads  
- Executives  

It demonstrates not just what I built, but **why it matters for modern cybersecurity, risk reduction, and enterprise resilience**.

