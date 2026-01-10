# Question Classification using Classical ML (OCR-based)

## Problem Overview
The goal of this task is to classify high school mathematics questions into subtopics
(e.g., Algebra, Geometry, Calculus) using **classical machine learning methods**.
The dataset is image-based, with questions provided as scanned images organized by class.

To stay aligned with the assignment constraints, no deep learning or vision-based models
were used.

---

## Approach

### 1. Image to Text (OCR)
- All question images were processed using **Tesseract OCR**.
- Images were converted to grayscale before OCR.
- OCR output was highly noisy due to:
  - Mathematical symbols
  - Diagrams and expressions
  - Low-quality scans

Empty or invalid OCR outputs were filtered during ingestion to avoid NaN propagation.

---

### 2. Text Preprocessing
- Lowercasing
- Removal of non-alphanumeric characters
- Whitespace normalization

Aggressive linguistic preprocessing (stemming, stopword removal) was intentionally avoided,
as mathematical text relies heavily on symbols and short tokens.

---

### 3. Word-level TF-IDF Attempt (Limitation)
A word-level TF-IDF representation was initially attempted.  
However, this consistently resulted in an **empty vocabulary** error.

**Reason:**
- OCR-extracted math text is dominated by single-character tokens (`x`, `y`, digits).
- Standard TF-IDF tokenization ignores such tokens, leaving no usable vocabulary.

This limitation is inherent to applying word-level NLP techniques to OCR-extracted
mathematical content and is documented rather than hidden.

---

### 4. Fallback: Character-level TF-IDF
To complete the pipeline while remaining within classical ML constraints,
a **character-level TF-IDF** representation was used:

- Analyzer: `char`
- n-gram range: 2–4

This approach is robust to OCR noise and preserves information from mathematical symbols.

---

### 5. Models
- Logistic Regression (baseline)
- Linear SVM (ablation)

Models were trained on the training split only.

---

## Evaluation Strategy
The provided test split produced almost entirely empty OCR output and was therefore
not suitable for quantitative evaluation.

As a result:
- Metrics are reported on the training set as a **sanity check**
- Test results are discussed qualitatively

This decision prioritizes correctness and transparency over misleading numbers.

---

## Results
- Character-level TF-IDF enabled successful model training
- Logistic Regression and Linear SVM both converged without issue
- Training accuracy was reasonable given OCR noise and class overlap

---

## Limitations
- OCR quality is the primary bottleneck
- Word-level NLP methods are poorly suited for math-heavy OCR text
- No vision-based models were used, by design

---

## Future Improvements
With more time or relaxed constraints:
- Domain-specific OCR tuning
- Character-level models with better calibration
- Vision-based classification for diagram-heavy questions

