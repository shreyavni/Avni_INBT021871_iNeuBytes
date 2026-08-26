**Name:** Avni Shukla
**Regd. No.:** INBT021871
**Course ID:** AIINB10726

# Avni_INBT021871_iNeuBytes

This repository contains all iNeuBytes internship deliverables: two standalone
ML/DL tasks and one full-stack major project, each in its own folder with its
own README, code, and results.

## Repository Structure

```
.
├── Task 1/            # CIFAR-10 image classification with CNNs (TensorFlow/Keras)
├── Task 2/             # Sentiment/emotion analysis — classical ML vs. LSTM
├── Major Project/      # Reel Match — AI movie recommendation web app (Flask + TF-IDF)
└── README.md           # This file
```

## Contents

### [Task 1 — CIFAR-10 Image Classification with CNNs](./Task%201)
Two CNNs trained and compared on CIFAR-10: an AlexNet-style traditional
baseline (Part A) and a deeper, regularized customized model (Part B). The
customized model beat the baseline by 12.99 accuracy points under the same
seed, split, and epoch budget. See `Task 1/README.md` for setup and results,
and `Task 1/CIFAR10_CNN_Report.docx` for the full report.

### [Task 2 — Sentiment Analysis using ML and DL](./Task%202)
A classical ML baseline (TF-IDF + Logistic Regression + SVM) compared against
an LSTM deep learning model for multi-class emotion classification, using an
identical train/test split for a fair comparison. SVM was the strongest model
(macro F1 = 0.8527). See `Task 2/README.md` for setup and results, and
`Task 2/Task2_Sentiment_Analysis_Report.docx` for the full report.

### [Major Project — Reel Match: AI Movie Recommendation Web App](./Major%20Project)
A full-stack content-based movie recommender (TF-IDF + Nearest Neighbors)
served through a Flask backend and a static frontend, deployable to Render,
Railway, or PythonAnywhere. See `Major Project/README.md` for architecture,
API reference, and deployment instructions.

## Links

- **Live deployed application:** https://reelmatch-0mfh.onrender.com/
- **GitHub repository:** https://github.com/shreyavni/Avni_INBT021871_iNeuBytes
- **Google Doc reports:** 
     Task 1 : https://docs.google.com/document/d/1Sq8_W05p8oMXPAYGxBLxMDiRsHp5sC0b/edit?usp=sharing&ouid=118371209598495756708&rtpof=true&sd=true

     Task 2: https://docs.google.com/document/d/1j5jvfiwpipHTwCzyTcFdQlWfuA8jSwgh/edit?usp=sharing&ouid=118371209598495756708&rtpof=true&sd=true

## Notes

- Each folder is self-contained — its own `requirements.txt`, `README.md`,
  and code — and can be run independently by following that folder's README.
- This repository is public per the submission requirements, but its link is
  only shared through the iNeuBytes portal, not on any other platform or
  social media.
