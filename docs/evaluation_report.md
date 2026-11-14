# Model Evaluation and Performance Improvement Report

## Objective
Evaluate and compare several machine learning models on the Titanic dataset, diagnose overfitting and underfitting, and choose the best model.

## Method
Data: updated_dataset.csv (891 passengers, 16 features), split 80/20 with random_state=42. Models were scored on the test set and with 5-fold cross-validation.

## Metric Selection
- Accuracy: share of all predictions that are correct. It alone can mislead here, because about 62% of passengers died, so always guessing "died" would already score about 62%.
- Precision: of the passengers predicted to survive, how many really did.
- Recall: of the passengers who really survived, how many the model found.
- F1-score: balances precision and recall in one number. It is the main metric used to pick the best model, because both missing survivors and false alarms matter.
- Train-test gap and cross-validation: used to detect overfitting.

## Performance Comparison
| Model | Train Acc | CV Acc | Test Acc | Precision | Recall | F1 | Gap |
|---|---|---|---|---|---|---|---|
| Logistic Regression | 0.826 | 0.820 | 0.832 | 0.810 | 0.739 | 0.773 | -0.006 |
| KNN (k=5) | 0.862 | 0.810 | 0.816 | 0.790 | 0.710 | 0.748 | 0.046 |
| Decision Tree (depth 1) | 0.785 | 0.780 | 0.771 | 0.689 | 0.739 | 0.713 | 0.014 |
| Decision Tree (no limit) | 0.983 | 0.799 | 0.793 | 0.742 | 0.710 | 0.726 | 0.190 |
| Decision Tree (depth 3) | 0.843 | 0.826 | 0.816 | 0.750 | 0.783 | 0.766 | 0.027 |
| Random Forest | 0.983 | 0.801 | 0.816 | 0.773 | 0.739 | 0.756 | 0.167 |

## Overfitting and Underfitting
- Underfitting: the depth-1 Decision Tree is too simple. Its scores are low on both the training and test data (0.785 and 0.771), and it has the lowest F1-score (0.713).
- Overfitting: the Decision Tree with no depth limit scores 0.983 on training data but only 0.793 on test data, a gap of 0.190. The Random Forest shows the same pattern (0.983 vs 0.816).
- Improvement: limiting the tree to depth 3 shrank the gap from 0.190 to 0.027 and raised test accuracy from 0.793 to 0.816 and F1 from 0.726 to 0.766.

## Strengths and Weaknesses
- Logistic Regression: simple, fast, easy to explain, and it does not overfit. It can miss complex patterns.
- KNN: easy to understand and needs no training, but it depends on feature scaling and slows down with large data.
- Decision Tree: easy to read as yes/no rules. It overfits badly if its depth is not limited.
- Random Forest: more stable than one tree and usually strong, but it overfits the training data here and is harder to explain.

## Conclusion
Logistic Regression is the chosen model. It has the best test accuracy (0.832), precision (0.810), and F1-score (0.773), it has no overfitting gap, and it is the simplest to explain. The depth-3 Decision Tree is a close second and finds the most survivors (recall 0.783). The test set has only 179 passengers, so differences of one or two percent are within normal noise, which makes the simpler model the safer choice.
