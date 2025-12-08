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

I developed this **AI-driven intrusion detection system (IDS)** to classify cyberattacks hidden inside large volumes of enterprise network traffic. Modern organizations generate millions of network flows per day, and manually reviewing all of them is impossible. This project shows how machine learning can support Security Operations Centers (SOCs) by automatically flagging suspicious behavior.

Using **2.8M+ network flows** from the CICIDS2017 dataset, I designed, trained, and evaluated multiple machine-learning models to detect high-risk threats such as:

- DoS (Hulk, Slowloris, GoldenEye)  
- PortScan  
- Brute Force (FTP-Patator, SSH-Patator)  
- Web Attacks (XSS, SQL Injection)  
- Infiltration  
- Botnet activity  

This project reflects how I operate as a Data Scientist and security-focused analyst:

- I build **end-to-end ML solutions** that go from raw data to clear, actionable insights.  
- I structure my work so that both **technical and non-technical audiences** can follow the logic and understand the impact.  
- I always connect my modeling choices to **risk, impact, and operational value**, not just accuracy metrics.

---

# 2. **Business Context**

Today’s SOC teams must cope with:

- Massive and constantly growing data volume  
- Evolving and sophisticated attack types  
- High alert fatigue caused by too many false positives  
- Complex and strict compliance requirements  
- Pressure to reduce incident response time and breach-related costs  

Traditional rule-based systems are not enough on their own. **AI-powered intrusion detection** helps organizations:

- Detect attacks earlier by identifying abnormal network behavior  
- Reduce SOC workload by filtering obvious benign and repetitive patterns  
- Improve accuracy and consistency of threat classification  
- Protect revenue, operations, and sensitive data from disruption and exfiltration  
- Strengthen compliance posture (NIST, SOC2, HIPAA, PCI-DSS, FedRAMP, etc.)

This system reflects the kind of solution needed in **cybersecurity, defense contracting, healthcare, government, finance, and enterprise cloud systems**, where both performance and explainability matter.

---

# 3. **Data Structure & Initial Diagnostics**

The CICIDS2017 dataset is a widely used benchmark for building and evaluating intrusion detection systems. It contains:

- **Flow-based features:** packet counts, flow durations, forward/backward rates, inter-arrival times  
- **Statistical metrics:** averages, standard deviations, min/max values  
- **TCP flag information:** indicators like SYN, ACK, FIN, etc., which help identify protocol misuse  
- **Rich attack labeling:** multiple distinct attack categories and BENIGN traffic  

### ✔ What I did

To make the dataset usable in a realistic modeling pipeline, I:

- Cleaned corrupted rows to remove invalid or incomplete records.  
- Removed infinite and null values to prevent model instability.  
- Standardized numerical fields to handle different value scales and improve model convergence.  
- Encoded categorical protocol flags so they could be used in machine learning models.  
- Processed the data in chunks because of its size (2.8M+ rows), which reflects real-world “big data” constraints.  
- Conducted statistical and behavioral exploratory data analysis (EDA) to understand how different attacks behave at the network level.  
- Identified extreme **class imbalance**, where BENIGN and a few attack types dominate most of the dataset.

These steps simulate what I would do in a real SOC or data engineering environment before putting models into production.

---

# 4. **Executive Summary**

## 4.1 **Key Findings**

From my experiments and analyses, the key technical and business findings are:

- The **Decision Tree model** achieved **~96% accuracy**, which is strong for structured network data. It provided interpretable rules that can be understood and reviewed by security teams and auditors.  
- The **Neural Network model** achieved **~32% accuracy**, which indicates underfitting and highlights that deep learning is not automatically better for this type of tabular data.  
- Attack frequency is highly skewed toward **high-volume attacks** like DoS and PortScan, which are noisy but relatively easier to catch.  
- Rare classes (such as Infiltration, Botnet, and certain Web Attacks) represent **high-severity threats** that can bypass poorly designed models and cause major damage if they are missed.

These results shaped how I evaluated the trade-offs between accuracy, interpretability, and operational usefulness.

## 4.2 **Threat Concentration**

The distribution of attacks has two important business meanings:

- **High-frequency attacks → operational cost:**  
  These attacks (DoS, PortScan) create noise, consume bandwidth, and overburden SOC teams. Automating their detection saves time and reduces fatigue.

- **Low-frequency attacks → compliance, legal, and financial risk:**  
  These attacks (e.g., Infiltration, Botnet, SQL Injection) are less common but often more damaging. Missing them can lead to breaches, regulatory fines, reputational damage, and long-term operational impact.

Understanding this concentration helped guide my **modeling strategy, choice of metrics, and recommendations** for deployment.

---

# 5. **Deep-Dive Analysis (Visuals + Interpretation + Business Value)**

This section presents core visualizations from the project and explains what they mean technically and from a business perspective.

---

## 5.1 **Class Distribution**

![Bar Chart](assets/Data%20Visualization%202_Bar%20Chart.png)

###  Interpretation

This bar chart displays the distribution of BENIGN traffic compared to each attack type. The key observations are:

- **BENIGN traffic dominates**, which reflects a realistic environment where most network flows are not malicious.  
- DoS Hulk and PortScan are the primary volume-based threats, appearing very frequently.  
- Rare attacks such as Botnet, SQL Injection, and Infiltration appear in small numbers and would be easy for a naive model to ignore.

From a modeling point of view, this confirms that we are dealing with a **severely imbalanced multi-class classification problem**.

###  Business Value

For security and leadership teams, this visualization highlights:

- Class imbalance directly affects **false-negative risk**. If the model learns mostly from BENIGN and dominant attacks, rare but critical threats may be missed.  
- Automated detection systems must be designed to **prioritize rare, high-impact threats**, not just optimize overall accuracy.  
- The SOC must pair high-volume attack detection with mechanisms for monitoring and investigating low-frequency “quiet” attack types.

Imbalance is not just a mathematical issue; it is a **risk and governance issue**.

---

## 5.2 **Feature Correlation Insights**

![Correlation Heatmap](assets/Data%20Visualization_Correlation%20Heatmap.png)

###  Interpretation

The correlation heatmap shows how different numerical features move together:

- There are strong correlation clusters among metrics like packet lengths, durations, and flow rates, which capture volumetric behaviors seen in DoS-style attacks.  
- Some features, particularly those related to TCP flags or idle/active times, show weaker or more unique patterns. These can be especially useful for identifying stealthy or non-standard behaviors.

By understanding which features are redundant and which carry unique information, I can simplify the model and focus on the most informative signals.

###  Business Value

For the business:

- Reducing redundant features lowers **cloud compute cost** and improves **model latency**, which matters in real-time security environments.  
- Higher interpretability supports **auditability**, which is essential when security tools are reviewed under standards like NIST or SOC2.  
- Recognizing which behaviors correlate most with attacks allows security teams to better explain “what changed” in the network during an incident.

This is an example of how **good feature understanding links directly to operational efficiency and compliance**.

---

## 5.3 **Decision Tree Results**

![Decision Tree](assets/Figure%201_Decision%20Tree%20Model.png)

###  Interpretation

The Decision Tree model reached **~96% accuracy** and produced clear decision paths:

- It captured strong and consistent patterns for BENIGN, DoS, and PortScan traffic.  
- Its splitting logic can be traced and explained, which helps security teams understand why the model labeled a flow as malicious or benign.  
- While it may not capture every complex edge case, it provides a solid backbone for high-volume intrusion detection.

###  Business Value

Decision Trees work very well for organizations that need both **performance and transparency**:

- They are ideal for **first-layer triage** in SOC workflows, automatically filtering repetitive or clearly malicious traffic.  
- Their explainability supports **compliance, internal investigations, and external audits**.  
- Because they are computationally efficient and easy to monitor, they are suitable for production use in many enterprise systems.

This makes the Decision Tree a practical and low-friction candidate for deployment.

---

## 5.4 **Neural Network Performance**

![NN Model](assets/Figure%202_Neural%20Network%20Model.png)

![NN Accuracy & Loss](assets/Neural%20Network-Model%20Accuracy%20&%20Model%20Loss.png)

###  Interpretation

The Neural Network was trained on the same dataset but achieved only **~32% accuracy**, and its learning curves show signs of **underfitting**. This is not unusual when applying generic feed-forward architectures to highly structured and imbalanced tabular data:

- The model struggled to learn generalizable patterns across all classes.  
- It was sensitive to imbalance and did not perform as well as the simpler tree-based model.  

This experiment is important because it shows that “more complex” does not always mean “more effective.”

###  Business Value

From a business and engineering point of view, this result:

- Confirms that **classic ML models can outperform neural networks** for certain structured cybersecurity problems.  
- Suggests that deep learning may be better applied to **sequence-based**, **time-series**, or **encrypted traffic** scenarios, rather than straightforward tabular flow data.  
- Reinforces the need for a **hybrid approach**, where simpler models handle baseline detection and more complex models are reserved for specialized use cases.

This prevents over-investing in unnecessarily complex solutions that don’t bring additional business value.

---

# 6. **Key Performance Indicators (KPIs)**

| KPI | Result | Business Meaning |
|------|--------|------------------|
| **Decision Tree Accuracy** | ~96% | Reliable for operational filtering and scalable triage |
| **NN Accuracy** | ~32% | Not deployment-ready on this dataset as-is |
| **Dataset Size** | 2.8M+ flows | Demonstrates ability to handle enterprise-scale data |
| **Threat Classes** | 15 | Covers a wide variety of attack behaviors |
| **Model Stability** | Stable DT, unstable NN | Guides which architecture is suitable for production |

These KPIs allow both technical teams and leadership to judge whether the system is mature enough for testing, pilot deployment, or further iteration.

---

# 7. **Recommendations**

### For Security Teams

- Deploy the **Decision Tree model** as a fast, explainable filtering layer to reduce the number of alerts that require manual review.  
- Supplement it with **anomaly detection** or secondary models specifically tuned for rare attacks.  
- Log predicted malicious flows for deeper forensic analysis and incident-response workflows.

### For Engineering Teams

- Use **SMOTE/ADASYN** or class weighting to handle class imbalance and improve minority-class performance.  
- Experiment with **sequence-based models** (GRU, LSTM, 1D CNN) if traffic is represented over time rather than as isolated flows.  
- Implement feature selection and model optimization to reduce inference time in production.

### For Executives

- AI-based intrusion detection can reduce SOC workload by an estimated **40–60%**, depending on the environment.  
- Faster, more accurate filtering reduces the cost of triage and the chance of missing a critical threat.  
- Investing in explainable AI supports **governance, risk management, and compliance**, which are central to long-term resilience.

---

# 8. **Real-World Impact & Industry Relevance**

This system is representative of the detection and analytics pipelines used in:

- **Defense contractors (DoD, DHS, IC partners)**  
- **Federal agencies (FISMA, NIST 800-53)**  
- **Finance & banking (fraud and intrusion monitoring)**  
- **Healthcare systems (HIPAA and PHI protection)**  
- **Cloud providers & telecom networks**  
- **Critical infrastructure and OT security**

### Impact Highlights

If deployed and extended in a real environment, a system like this can:

- Enable **faster threat detection** by automating initial classification.  
- Reduce **false positives** and help SOC analysts focus on the highest-priority incidents.  
- Improve **SOC efficiency** and collaboration between data science and security teams.  
- Support regulatory and contractual obligations around monitoring and incident readiness.  
- Help reduce the **financial and reputational cost** of successful cyberattacks.

---

# 9. **Assumptions & Limitations**

- The CICIDS2017 dataset is simulated but designed to approximate realistic traffic; real networks may exhibit additional complexity.  
- Rare attack classes are underrepresented, which impacts performance metrics and may require advanced balancing methods.  
- Flow-level IDS systems do not inspect encrypted payloads, which limits visibility into some categories of attacks.  
- Real production deployments require **continuous retraining, monitoring for model drift**, and integration with streaming data sources.

Being transparent about these limitations is essential for responsible AI and realistic security planning.

---

# 10. **Technologies Used**

- **Python** for data processing and modeling  
- **Pandas, NumPy** for data manipulation  
- **Scikit-Learn, TensorFlow** for model development and evaluation  
- **Matplotlib, Seaborn** for visualization and diagnostic charts  
- **Jupyter Notebook** for exploratory analysis and iterative development  
- **CICIDS2017 Dataset** as the basis for realistic attack simulation  

---

# 📌 Final Note  

This project is written to support both technical and non-technical audiences.  
It is designed to help recruiters, SOC managers, engineering teams, and executive leaders understand the practical business value of AI-driven intrusion detection systems.  
My goal is to demonstrate not only model performance, but also the operational and strategic “why” behind every decision.
