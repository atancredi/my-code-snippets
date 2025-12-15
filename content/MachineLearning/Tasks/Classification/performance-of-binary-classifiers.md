# Evaluating performances of binary classifiers


## Receiver Operating Characteristics (ROC) curve

- x: $FAR$ (false acceptance rate)
    - for tasks such as stopword detection, where the model is running continously and the input is processed in time windows, FAPH (false accepts per hour) is used.
- y: $TAR$ (true acceptance rate)
    -  $TAR = 1 - FRR$ where $FRR$ is the false rejection rate

The area under the ROC curve (AUC) gives the ratio between $FAR$ and $TAR$, so a value of $0.5$ represents a random classifier and $1$ represents a perfect classifier.

## Confusion Matrix

Predicts the performance of the classifier by comparing the predictions with the actual labels.

|                      | **Predicted Positive** | **Predicted Negative** |
|----------------------|-------------------------|-------------------------|
| **Actual Positive**  | True Positive ($TP$)      | False Negative ($FN$)     |
| **Actual Negative**  | False Positive ($FP$)     | True Negative ($TN$)      |

The matrix is used to calculate accuracy, precision, recall and F1-score

### Accuracy

Measures overall correctness.

$$accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$

_Good when classes are balanced, but misleading with imbalanced data._

### Precision

Correctly predicted positive samples vs all samples predicted as positive.

$$precision=\frac{TP}{TP + FP}$$

High precision → few false alarms.

### Recall (a.k.a. Sensitivity or True Positive Rate)

Correctly predicted positive samples vs all the actually predicted positives.

$$recall=\frac{TP}{TP + FN}$$

High recall → few missed positives.

### F1 Score

Harmonic mean of precision and recall.

$$F1 = 2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$$

Useful when you need a balance between precision and recall, especially with imbalanced datasets.