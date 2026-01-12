# Classical Machine Learning for Math Question Classification

## Overview
This project implements a **classical machine learning pipeline** to classify math questions into their respective subtopics (e.g., algebra, geometry, number theory).

Each question is stored as an **individual JSON file**, and the goal is to demonstrate correct data handling, feature engineering, model selection, and evaluation using traditional ML techniques. An optional bonus task demonstrates the use of a **Large Language Model (LLM)** to generate student-friendly, step-by-step solutions for a small sample of questions.

The project intentionally avoids deep learning models in order to focus on **interpretable, efficient, and well-justified classical methods**, as required by the assignment.

---

## Dataset Description
- Each math question is stored in a separate JSON file
- Questions are organized into directories by subtopic
- The directory name is treated as the ground-truth label

### Subtopics Included
- algebra  
- geometry  
- precalculus  
- intermediate_algebra  
- prealgebra  
- number_theory  
- counting_and_probability  

The dataset is provided with predefined **train** and **test** splits.

---

## Data Loading and Preprocessing
The dataset is loaded directly from the directory structure rather than being converted into a table prematurely.

Steps:
1. Iterate over each subtopic directory
2. Load individual JSON files
3. Extract the question text (e.g., from `question` or `problem` fields)
4. Assign labels based on directory names
5. Discard empty or malformed entries

### Text Cleaning
Minimal preprocessing is applied:
- lowercase conversion
- removal of non-alphanumeric characters
- whitespace normalization

Aggressive preprocessing (such as stemming or stopword removal) is avoided to preserve mathematical meaning.

---

## Feature Engineering
Two TF-IDF representations are explored:

### Word-Level TF-IDF
- Unigrams and bigrams
- Captures semantic information and mathematical terminology

### Character-Level TF-IDF
- Character n-grams (3–5)
- Helps model:
  - mathematical symbols
  - formatting patterns
  - LaTeX-style artifacts

These representations are lightweight, interpretable, and effective for sparse text data.

---

## Models Used
Two classical linear models are evaluated:

### Logistic Regression
- Strong baseline for text classification
- Efficient and interpretable

### Linear Support Vector Classifier (LinearSVC)
- Well-suited for high-dimensional sparse features
- Consistently outperforms Logistic Regression in experiments
- Selected as the final model

---

## Experimental Results

### Model Comparison

| Model | Features | Accuracy | Macro F1 |
|------|---------|----------|----------|
| Logistic Regression | Word TF-IDF | ~0.72 | ~0.73 |
| Logistic Regression | Char TF-IDF | ~0.73 | ~0.74 |
| LinearSVC | Word TF-IDF | ~0.75 | ~0.76 |
| **LinearSVC** | **Char TF-IDF** | **~0.75** | **~0.76** |

### Key Observations
- LinearSVC consistently outperforms Logistic Regression
- Character-level TF-IDF provides small but consistent improvements
- Certain subtopics (e.g., prealgebra) are inherently ambiguous and overlap with algebra-related categories

### Final Model
**LinearSVC with character-level TF-IDF** is selected as the final model due to its superior and more stable performance.

---

## Error Analysis
Most errors occur between closely related subtopics, particularly:
- prealgebra vs algebra
- algebra vs intermediate_algebra

This overlap is expected and difficult to resolve without deeper semantic or curriculum-level context.

---

## Bonus Task: LLM-Based Solution Generation
As an optional extension, an **external Large Language Model (LLM)** is used to generate **step-by-step, student-friendly solutions** for a small sample of math questions.

- The LLM is accessed via an external API
- Only a limited subset of questions is processed
- The generated explanations are displayed directly within the notebook output
- The LLM is **not integrated into the classification pipeline**

This demonstrates how LLMs can complement classical ML systems for educational use cases while keeping the core task purely classical.

---

## Reproducibility
- The entire workflow is contained in a single Jupyter notebook
- The notebook can be executed top-to-bottom without relying on hidden state
- All experiments are deterministic given the same environment

---

## How to Run
1. Open the notebook in **Google Colab**
2. Run all cells sequentially from top to bottom
3. View evaluation metrics and LLM-generated explanations directly in the notebook output

---

## Design Philosophy
- Prefer clarity over complexity
- Use classical ML where it is sufficient
- Perform controlled ablations instead of blind optimization
- Keep optional extensions clearly separated from core functionality

---

## Conclusion
This project demonstrates a complete and well-structured classical ML workflow:
- correct data ingestion from JSON files
- principled feature engineering
- meaningful model comparisons
- thoughtful error analysis
- optional, well-isolated LLM usage

The resulting system is efficient, interpretable, and fully aligned with the assignment requirements.
