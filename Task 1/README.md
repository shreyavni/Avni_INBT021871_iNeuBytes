**Name:** Avni Shukla
**Regd. No.:** INBT021871
**Course ID:** AIINB10726

# CIFAR-10 Image Classification with CNNs — Task 1

This project implements and compares two Convolutional Neural Networks on the
CIFAR-10 dataset using TensorFlow/Keras:

- **Part A — Traditional CNN**: an AlexNet-style baseline adapted for 32×32 inputs.
- **Part B — Customized CNN**: a deeper, regularized architecture (extra conv
  block, all 3×3 filters, batch normalization, tuned dropout, data augmentation,
  and an adaptive learning-rate schedule) designed to beat Part A by a
  measurable margin under the same random seed, data split, and epoch budget.

## Contents

| File | Description |
|---|---|
| `CIFAR10_CNN_PartA_and_B.ipynb` | Complete, runnable notebook: data loading, preprocessing, both models, training, architecture diagrams, evaluation, confusion matrices, and the final comparison. |
| `requirements.txt` | Python packages needed to run the notebook. |
| `README.md` | This file. |
| `CIFAR10_CNN_Report.docx` | Written report covering methodology, results, and the explanation of why the customized model performed better (the "Google Doc" deliverable — open it and re-save/upload as a Google Doc if that exact file type is required). |

Running the notebook end-to-end produces the following image files (already
included in this folder from the last run):

| File | Contents |
|---|---|
| `traditional_cnn_architecture.png` | Part A architecture diagram. |
| `customized_cnn_architecture.png` | Part B architecture diagram. |
| `traditional_cnn_accuracy_and_loss_graph.png` | Part A training/validation accuracy and loss curves. |
| `customized_cnn_accuracy_and_loss_graph.png` | Part B training/validation accuracy and loss curves. |
| `traditional_cnn_confusion_matrix.png` | Part A confusion matrix. |
| `customized_cnn_confusion_matrix.png` | Part B confusion matrix. |
| `test_performance_traditional_vs_customized.png` | Final side-by-side comparison chart, Part A vs. Part B. |

## Setup

1. Create and activate a virtual environment (recommended):
   ```bash
   python3 -m venv venv
   source venv/bin/activate      # Windows: venv\Scripts\activate
   ```
2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. The architecture-diagram cells use Keras' `plot_model`, which needs the
   **system** Graphviz binary in addition to the `graphviz`/`pydot` Python
   packages:
   ```bash
   # Debian/Ubuntu
   sudo apt-get install graphviz
   # macOS
   brew install graphviz
   ```
   If Graphviz isn't available, the notebook automatically falls back to a
   manual matplotlib-drawn block diagram, so the diagram step never fails —
   it just looks simpler without Graphviz installed.

## Running

Open the notebook and run all cells top to bottom:
```bash
jupyter notebook CIFAR10_CNN_PartA_and_B.ipynb
```
or run it in Google Colab (GPU runtime recommended — Part A + Part B together
train for 30 epochs each, which is slow on CPU).

The notebook is fully sequential and self-contained: Part B reuses the exact
`SEED`, train/validation/test split, and epoch budget defined in Part A, so
the two models are directly comparable.

## Results

| Metric | Traditional CNN (Part A) | Customized CNN (Part B) |
|---|---|---|
| Test Accuracy | 0.7020 | 0.8319 |
| Precision (macro) | 0.7043 | 0.8472 |
| Recall (macro) | 0.7020 | 0.8319 |
| F1-score (macro) | 0.7004 | 0.8313 |
| Parameter Count | 3,326,602 | 3,844,938 |
| Training Time (s) | 286.45 | 526.89 |

- **Part A success threshold:** ≥ 70% test accuracy — **PASS** (70.20%).
- **Part B success threshold:** ≥ 3 percentage-point test-accuracy improvement
  over Part A — **PASS** (+12.99 points).

Full methodology, the change-by-change rationale for Part B, and the
discussion of why the customized model performed better are in
`CIFAR10_CNN_Report.docx`.

## Notes on reproducibility

A fixed seed (`SEED = 42`) is set for Python's `random`, NumPy, and
TensorFlow, and the train/validation/test split uses `random_state=SEED`
with stratification. Exact bit-for-bit reproducibility across different
hardware/TensorFlow versions is not guaranteed (GPU non-determinism), but
results should be very close run-to-run on the same machine.
