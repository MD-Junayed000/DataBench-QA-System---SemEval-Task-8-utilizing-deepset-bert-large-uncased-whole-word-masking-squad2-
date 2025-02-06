# 📌 DataBench QA System - SemEval Task 8

## 📝 Overview
This repository contains a fully automated *question-answering (QA) system* developed for the *SemEval Task 8: DataBench Competition. The system is designed to extract answers from structured datasets, process them, and generate responses using **NLP techniques* and *transformer-based models*.

---

## 📜 Project Summary
### 🔹 *What is SemEval Task 8: DataBench?*
SemEval Task 8: DataBench is a benchmarking competition for evaluating question-answering models on structured datasets. The goal is to answer questions based solely on provided datasets without external data. Participants must build systems capable of understanding structured tabular data, processing it effectively, and generating relevant responses.

The official dataset for this competition is hosted on Hugging Face and can be accessed here: [SemEval Task 8: DataBench Dataset](https://www.codabench.org/competitions/3360/).

This challenge requires a hybrid approach combining structured querying with advanced NLP techniques to handle various types of questions, including Boolean, categorical, numerical, and list-based answers.

### 🔹 *How does this project work?*
Our system processes structured datasets, extracts relevant information, and generates responses through a *multi-step approach*:

1. *Data Preprocessing:* Converts .parquet datasets into structured .csv files, ensuring clean and consistent formatting.
2. *Semantic Column Matching:* Uses *TF-IDF and cosine similarity* to match dataset columns with questions, helping identify relevant data fields.
3. *Question Answering:* Extracts Boolean, categorical, numeric, and list-based answers using *structured queries and a transformer-based model*.
4. *Prediction Output:* Generates structured predictions.txt and predictions_lite.txt files formatted for competition submission.

---
📂 Folder Structure

This repository follows a structured format to ensure easy navigation and reproducibility of results:

|-- competition/
    |-- 066_IBM_HR/
        |-- all.parquet
        |-- sample.parquet
    |-- 067_TripAdvisor/
        |-- all.parquet
        |-- sample.parquet
    |-- ... (other dataset folders)
    |-- test_qa.csv

Each dataset has two parquet files:

all.parquet: Contains the full dataset.

sample.parquet: A subset of all.parquet with limited rows for quick testing.

test_qa.csv: Contains the questions to be answered.


## ⚙ Installation and Dependencies
Ensure you have *Python 3.7+* installed. Then, install the required dependencies:
sh
pip install pandas numpy scikit-learn transformers nltk torch


---

## 🔧 Data Preprocessing
### 📜 clean_data.py
Data preprocessing is crucial for ensuring the datasets are in a structured and consistent format. The script *cleans and standardizes* dataset values for better querying.

✅ *Preprocessing Steps:*
1. Converts categorical values to *lowercase* for uniformity.
2. Fills missing numerical values with *median imputation* to prevent data loss.
3. Normalizes text-based fields by *removing special characters and whitespaces*.
4. Ensures correct data types for *numeric and categorical fields*, allowing efficient processing.
5. Handles different dataset-specific cleaning processes (e.g., handling monetary values, dates, and categorical encodings).

Each dataset undergoes a unique preprocessing pipeline to ensure consistency across multiple structured datasets. This step plays a crucial role in enabling accurate question answering.

Run the script:
sh
python clean_data.py


---

## 🏆 Question Answering System
### 📜 qa_system.py
Once the datasets are cleaned, the *question answering system* takes over. This script extracts answers from the structured datasets based on the *given set of questions*.

✅ *Process Overview:*
1. **Reads test_qa.csv** to extract questions and their corresponding datasets.
2. *Matches relevant dataset columns* to the question using *semantic similarity techniques*.
3. *Applies logical operations* to derive answers for Boolean, category, numeric, and list-based questions.
4. *Uses a transformer-based QA model* (deepset/bert-large-uncased-whole-word-masking-squad2) as a fallback when structured queries do not yield confident answers.
5. *Formats extracted answers* according to competition guidelines.

The system efficiently processes structured tabular data, enabling high-accuracy responses.

Run the script:
sh
python qa_system.py


---

## 📊 Generating Predictions
### 📜 generate_predictions.py
Once the questions are processed and answers are extracted, the system *formats the predictions for submission. This script generates the **final answer predictions* in the required format.

✅ *Generated Files:*
- predictions.txt (answers based on all.parquet datasets)
- predictions_lite.txt (answers based on sample.parquet datasets)

Run the script:
sh
python generate_predictions.py


🔹 The outputs are automatically zipped into CUET.zip, making it ready for *competition submission*.

---

## 🎯 How It Works
### 1️⃣ *Dataset Cleaning*
The raw .parquet files are converted into *cleaned CSV files*, ensuring that:
- Missing values are handled appropriately.
- Categorical and numerical fields are formatted correctly.
- Text-based fields are normalized.
- Any dataset-specific inconsistencies are resolved.

### 2️⃣ *Question Processing*
The system reads test_qa.csv, which contains *questions mapped to corresponding datasets. Each question is parsed and matched to **relevant dataset columns* using *semantic similarity techniques* (TF-IDF, cosine similarity, and keyword matching).

### 3️⃣ *Answer Extraction*
The system determines the *type of answer required*:
- *Boolean (Yes/No)* → Answers based on conditions in the dataset.
- *Category (Text from dataset)* → Extracts categorical values directly from dataset fields.
- *Number (Computed from dataset)* → Retrieves or computes statistics like sum, mean, max, min, etc.
- *List[Category] or List[Number]* → Extracts multiple values where necessary.

If structured querying does not yield a confident answer, the system *uses a transformer-based QA model* to infer answers contextually.

### 4️⃣ *Answer Formatting*
Extracted answers are *structured into a submission format, ensuring compliance with the **SemEval Task 8: DataBench competition guidelines*. The results are stored in predictions.txt and predictions_lite.txt, which are then zipped for submission.

---
