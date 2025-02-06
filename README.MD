# Log Message Classification System  

This project classifies log messages using a **hybrid model approach**, combining **rule-based classification (Regex), Machine Learning (BERT), and AI (LLM)**. The system efficiently categorizes logs into predefined categories, improving log analysis and monitoring.  

## **🔍 How It Works?**  
The classification follows a **three-step hierarchy**:  
1. **Regex-based classification** → Fast but works only for predefined patterns.  
2. **BERT-based classification** → Uses an ML model when regex fails.  
3. **LLM-based classification** → Handles logs from "LegacyCRM" using an AI model.  

The pipeline ensures **faster processing for common log patterns** while using **AI for complex or unknown messages**.  

---

## **📂 Project Structure**  

📦 Log-Classifier
│-- 📂 model/ # Trained machine learning model
│-- 📂 data/ # Input log files (CSV)
│-- 📂 scripts/ # Main processing scripts
│ │-- regex.py # Rule-based classification using regex
│ │-- bert.py # ML-based classification using BERT
│ │-- llm.py # LLM-based classification using Groq
│ │-- classregex.py # Hybrid model logic
│-- README.md # Project documentation
│-- requirements.txt # Dependencies list


## **🛠️ Setup & Installation**  

### **1️⃣ Install Dependencies**  
Make sure you have Python 3.8+ installed. Then, install required libraries:  

```bash
pip install -r requirements.txt
2️⃣ Run the Classifier on a CSV File
To classify logs stored in a CSV file, run:


python classregex.py
This will process test.csv and generate output.csv with classified labels.

🚀 Hybrid Model Approach
1️⃣ Regex-Based Classification (regex.py)
Uses predefined patterns to quickly classify logs.
Works well for system events, user actions, and standard notifications.
Example:

classify_with_regex("Backup completed successfully.")  
# Output: "System Notification"

2️⃣ ML-Based Classification (bert.py)
Uses SentenceTransformer (all-MiniLM-L6-v2) to convert text into embeddings.
A trained Logistic Regression model (log_classifier.joblib) predicts the category.
Used when regex fails to classify a log message.

classify_with_bert("System crashed due to driver errors")  
3️⃣ LLM-Based Classification (llm.py)
Uses Groq’s Llama-3.3-70b-versatile model for classification.
Only applied for logs from "LegacyCRM" since it's a legacy system.
Example:

classify_with_llm("The 'ReportGenerator' module will be retired.")  
# Output: "Deprecation Warning"

4️⃣ Hybrid Model Logic (classregex.py)
If source is "LegacyCRM" → Use LLM.
Else, try regex first → If regex fails, use BERT.
📊 Clustering & Model Training (ipynb file)
This project also includes clustering logs to identify common patterns using DBSCAN and training an ML model for classification.

Clustering groups logs into common categories using Sentence Transformers and DBSCAN.
Training a Model using:
Input: Log messages → Converted into embeddings.
Algorithm: Logistic Regression for classification.
Model Output: log_classifier.joblib.
📌 Example Input & Output

Input (test.csv)
timestamp	source	log_message
2025-06-27 07:20:25	ModernCRM	Email service experiencing issues with sending.
1/14/2025 23:07	LegacyCRM	The 'ReportGenerator' module will be retired.
1/17/2025 01:29	BillingSystem	Payment gateway returned an error: Transaction declined.

Output (output.csv)
timestamp	source	log_message	target_label
2025-06-27 07:20:25	ModernCRM	Email service experiencing issues with sending.	Critical Error
1/14/2025 23:07	LegacyCRM	The 'ReportGenerator' module will be retired.	Deprecation Warning
1/17/2025 01:29	BillingSystem	Payment gateway returned an error: Transaction declined.	Transaction Issue

🔗 Future Improvements
✅ Expand regex patterns for more log categories.
✅ Train a deep learning model (e.g., LSTM, Transformer) for classification.
✅ Fine-tune LLM responses for better accuracy.