**Name:** Avni Shukla
**Regd. No.:** INBT021871
**Course ID:** AIINB10726

# Task 2 — Sentiment Analysis using ML and DL

Classical ML baseline (Logistic Regression, SVM) and a Deep Learning model (LSTM)
for multi-class emotion classification on text, with a fair, apples-to-apples comparison.

## Project Structure

```
.
├── sentiment_analysis_partA_and_B.ipynb   # Complete implementation (Part A + Part B)
├── requirements.txt                        # Python dependencies
├── README.md                                # This file
├── Task2_Sentiment_Analysis_Report.docx     # Written report (Google Doc deliverable)
├── ml_pipeline_architecture.png             # Part A architecture diagram
├── lstm_architecture.png                    # Part B (LSTM) architecture diagram
├── confusion_matrices_all_models.png        # Confusion matrices for all three models
└── train.txt                                # Dataset (place here; not included)
```

## Dataset

A text-emotion dataset in the format `text;emotion` (semicolon-separated, no header),
e.g. the Kaggle "Emotions Dataset for NLP" (`train.txt`). Place `train.txt` in the same
directory as the notebook before running.

## Setup

```bash
pip install -r requirements.txt
python -m nltk.downloader punkt punkt_tab stopwords
jupyter notebook sentiment_analysis_partA_and_B.ipynb
```

## Part A — Classical ML Pipeline

See `ml_pipeline_architecture.png`.

1. **Load data** — read `train.txt`, encode emotion labels to integers.
2. **Class distribution** — value counts, bar chart, and an automatic imbalance check.
   Because the dataset is imbalanced, **macro F1-score** is used as the primary metric
   throughout, not raw accuracy.
3. **Text cleaning** — lowercase → remove punctuation → remove numbers → remove
   emojis/non-ASCII characters → remove extra whitespace.
4. **Tokenization** — `nltk.word_tokenize`.
5. **Stopword removal** — NLTK English stopword list.
6. **Fixed train/test split** — `train_test_split(test_size=0.20, random_state=42, stratify=...)`.
   This exact split is reused, unchanged, for the SVM and for the LSTM in Part B.
7. **TF-IDF vectorization** — `TfidfVectorizer` fit on the training text only.
8. **Models** — Logistic Regression and SVM (`LinearSVC`), both trained on the same
   TF-IDF features.
9. **Evaluation** — Accuracy, Precision, Recall, F1-score (macro) and a full
   `classification_report` for each model, plus a confusion matrix.

## Part B — Deep Learning (LSTM)

See `lstm_architecture.png`.

1. **Tokenize** — Keras `Tokenizer` (fit on the *same* `X_train` from Part A).
2. **Sequences** — `texts_to_sequences`.
3. **Padding** — `pad_sequences`, `MAX_LEN` chosen from the 95th percentile of
   training sequence lengths.
4. **Model** — trainable `Embedding` → `SpatialDropout1D` → `LSTM(128)`
   (with dropout and recurrent dropout) → `Dropout` → `Dense(64, relu)` → `Dropout`
   → `Dense(num_classes, softmax)`.
5. **Training** — Adam optimizer, sparse categorical cross-entropy, 10% validation
   split carved from the training data, `EarlyStopping` on validation loss.
6. **Curves** — training vs. validation accuracy/loss plotted to check for overfitting.
7. **Evaluation** — the same Accuracy/Precision/Recall/F1 (macro) metrics and
   confusion matrix as Part A, computed on the untouched test set.
8. **Final comparison** — a combined table and bar chart of Logistic Regression vs.
   SVM vs. LSTM, with an automatic verdict on whether the LSTM matched, beat, or
   fell short of the best classical model.

An optional stretch section is included with a skeleton for swapping the trainable
embedding for pre-trained GloVe vectors.

## Results

| Model | Accuracy | Precision (macro) | Recall (macro) | F1-score (macro) |
|---|---|---|---|---|
| Logistic Regression | 0.8509 | 0.8680 | 0.7427 | 0.7872 |
| SVM (LinearSVC) | 0.8869 | 0.8750 | 0.8344 | 0.8527 |
| LSTM | 0.8788 | 0.8251 | 0.8314 | 0.8240 |

**Recommended model: SVM (LinearSVC)**, F1-score (macro) = 0.8527. The LSTM did
not beat the best classical model (behind by 0.0287 F1 points) — full discussion
of why, and the challenges faced training the LSTM, are in
`Task2_Sentiment_Analysis_Report.docx`.

## Outputs Produced When Run

- Class distribution bar chart
- Cleaned/tokenized text samples
- TF-IDF vocabulary size and shapes
- Logistic Regression & SVM metrics + classification reports
- LSTM training/validation accuracy & loss curves
- LSTM metrics + classification report
- Confusion matrices for all three models (`confusion_matrices_all_models.png`)
- Final Logistic Regression vs. SVM vs. LSTM comparison table and chart

## Notes

- Random seed is fixed at `42` everywhere (NumPy, scikit-learn split, TensorFlow) for
  reproducibility.
- Macro-averaged Precision/Recall/F1 are used throughout because of class imbalance.
