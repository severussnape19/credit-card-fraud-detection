

# Credit Card Fraud Detection – Final Summary

## Dataset Overview
- Each row represents a credit card transaction.
- `Time` = seconds since the first transaction.
- `Amount` = transaction amount.
- `Class` = target (0 = normal, 1 = fraud).
- `V1–V28` are PCA-transformed features hiding sensitive data.

Fraud accounts for only **0.17%** of the dataset. 
Accuracy is misleading — even a model predicting all 0s would get **~99.83% accuracy**.

Hence, we focus on:
- **Precision**
- **Recall**
- **F1 Score**
- **ROC AUC**

---

## Data Preprocessing
- Only scaled the `Amount` column (rest were PCA'd).
- Applied **SMOTE** to handle class imbalance by oversampling the fraud class.
- Used an 80:20 train-test split.

---

## Neural Network Model
- 3-layer fully connected network using ReLU and Sigmoid.
- Optimizer: Adam (lr=0.0005)
- Loss: Binary Cross-Entropy

### Key Features:
- Custom threshold tuning (0.1 to 0.9 step 0.01) per epoch.
- Early stopping if F1 doesn’t improve for 10 epochs.
- Best model saved with best threshold.

---

## Evaluation (Deep Learning Model)
- **Epoch 48** yielded best performance.

**Metrics at Epoch 48:**
- **F1 Score:** 0.8229
- **Precision:** 0.8404
- **Recall:** 0.8061
- **ROC AUC:** 0.9720
- **Average Precision (PR AUC):** 0.759

### Graphs:
- Confusion Matrix
- Precision–Recall Curve
- ROC Curve
- F1, Precision, Recall, ROC AUC over epochs
- F1 vs Threshold plot

---

## Probability Calibration
- Calibrated raw DL model outputs using `LogisticRegressionCV`.
- Plotted pre/post calibration comparison.
- Calibration significantly improved confidence reliability.

---

## Traditional ML Models
- **XGBoost**
- **Random Forest**
- **CatBoost**

| Model         | F1 Score | Precision | Recall | ROC AUC |
|---------------|----------|-----------|--------|----------|
| XGBoost       | 0.783    | 0.743     | 0.827  | 0.989    |
| Random Forest | 0.871    | 0.920     | 0.827  | 0.953    |
| CatBoost      | 0.746    | 0.654     | 0.867  | 0.984    |

### ROC Curve Comparison:
- All models performed well.
- XGBoost had the highest AUC.

---

## Ensemble Model (Soft Voting)
- Combined average of XGBoost, RF, CatBoost probabilities.

**Ensemble Metrics:**
- **F1 Score:** 0.876
- **Precision:** 0.814
- **Recall:** 0.847
- **ROC AUC:** 0.986
- **PR AUC:** 0.876

- Performed better than individual models.
- Balanced false positives and false negatives well.

---

## Final Comparison: Ensemble vs Deep Learning
| Metric       | Deep Learning | Ensemble (Soft Voting) |
|--------------|----------------|-------------------------|
| F1 Score     | 0.8229         | 0.876                   |
| Precision    | 0.8404         | 0.814                   |
| Recall       | 0.8061         | 0.847                   |
| ROC AUC      | 0.9720         | 0.986                   |
| PR AUC       | 0.759          | 0.876                   |

### Conclusion:
- **Ensemble model** outperforms in terms of recall and PR AUC.
- **Deep learning** has slightly better precision.
- Ensemble offers more stable and reliable performance overall.

### Calibration (Final Step)
- Calibration plots showed the ensemble model was better aligned to real-world fraud probabilities.

---

## Takeaway
- High F1 and PR AUC are more important than raw accuracy in fraud detection.
- Using threshold tuning, early stopping, and calibration significantly improves real-world applicability.
- Ensembles often outperform a single model.

This notebook covered full preprocessing, custom training loop with threshold tuning, deep learning model evaluation, ML model benchmarking, ensembling, and calibration.

Ready for deployment or extension into a real-time fraud monitoring system 🚀
