# Log Message Classification System

## Overview
This project is a **hybrid log classification system** that categorizes log messages using three different methods:
1. **Regex-based classification** (Pattern matching for predefined log types)
2. **BERT-based classification** (Machine learning model for complex patterns)
3. **LLM-based classification** (AI-powered classification using Groq's Llama-3.3-70b model)

The system processes logs from various sources and applies the most efficient classification technique based on the source type.

## Features
✅ **Hybrid Approach:** Combines rule-based, ML-based, and AI-based classification for accuracy and speed.  
✅ **Automated Log Processing:** Reads logs from a CSV file, classifies them, and saves results.  
✅ **Clustering for Insights:** Uses DBSCAN to group similar log messages.  
✅ **Trained Model:** A Logistic Regression model (`log_classifier.joblib`) for log classification.  
✅ **Scalable & Extensible:** Supports adding new classification methods easily.  

---

## Project Structure
```
📦 Log-Classifier  
│-- 📂 model/                   # Trained machine learning model  
│-- 📂 data/                    # Input log files (CSV)  
│-- 📂 scripts/                 # Main processing scripts  
│   │-- regex.py                # Regex-based classification  
│   │-- bert.py                 # ML-based classification using BERT  
│   │-- llm.py                  # LLM-based classification using Groq  
│   │-- classregex.py           # Hybrid model logic  
│-- README.md                   # Project documentation  
│-- requirements.txt            # Dependencies list  
```

---

## Installation & Setup
### 1️⃣ Install Dependencies
Make sure you have Python 3.8+ installed. Then, install the required libraries:
```bash
pip install -r requirements.txt
```

### 2️⃣ Run the Log Classifier
To classify logs stored in a CSV file, run:
```bash
python classregex.py
```
This will process `test.csv` and generate `output.csv` with classified log messages.

---

## Hybrid Model Classification Flow
### 1️⃣ Regex-Based Classification (`regex.py`)
- Uses **predefined patterns** to quickly classify logs.
- Example:
```python
classify_with_regex("Backup completed successfully.")
# Output: "System Notification"
```

### 2️⃣ BERT-Based Classification (`bert.py`)
- Converts log messages into **text embeddings** using `all-MiniLM-L6-v2`.
- Uses a **Logistic Regression model** to predict categories.
- Example:
```python
classify_with_bert("System crashed due to driver errors")
```

### 3️⃣ LLM-Based Classification (`llm.py`)
- Uses **Groq’s Llama-3.3-70b model** for classification.
- **Only used for logs from 'LegacyCRM'.**
- Example:
```python
classify_with_llm("The 'ReportGenerator' module will be retired.")
# Output: "Deprecation Warning"
```

### 4️⃣ Hybrid Model Logic (`classregex.py`)
- **If source is 'LegacyCRM' → Use LLM.**
- **Otherwise, try Regex first → If regex fails, use BERT.**

---

## Clustering & Model Training (`ipynb file`)
This project also includes **clustering logs** to identify patterns using **DBSCAN** and training an **ML model**.
- **Clustering** groups logs into common categories.
- **Training a Model:**
  - Input: Log messages → Converted into embeddings.
  - Algorithm: **Logistic Regression** for classification.
  - Model Output: `log_classifier.joblib`.

---

## Example Input & Output
### Input (`test.csv`)
| timestamp | source      | log_message |
|-----------|------------|-------------|
| 2025-06-27 07:20:25 | ModernCRM   | Email service experiencing issues with sending. |
| 1/14/2025 23:07 | LegacyCRM   | The 'ReportGenerator' module will be retired. |
| 1/17/2025 01:29 | BillingSystem | Payment gateway returned an error: Transaction declined. |

### Output (`output.csv`)
| timestamp | source      | log_message | target_label |
|-----------|------------|-------------|--------------|
| 2025-06-27 07:20:25 | ModernCRM   | Email service experiencing issues with sending. | Critical Error |
| 1/14/2025 23:07 | LegacyCRM   | The 'ReportGenerator' module will be retired. | Deprecation Warning |
| 1/17/2025 01:29 | BillingSystem | Payment gateway returned an error: Transaction declined. | Transaction Issue |

---

## Future Improvements
✅ Expand **regex patterns** for more log categories.  
✅ Train a **deep learning model** (e.g., LSTM, Transformer) for classification.  
✅ Fine-tune **LLM responses** for better accuracy.  
