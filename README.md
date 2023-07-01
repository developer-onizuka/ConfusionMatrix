# ConfusionMatrix

# 1. Precision

| |	Predict as a Spam Mail |	Predict as Not Spam Mail |
| :--- | :--- | :--- |
| Actually Spam Mail | **TP** (True Positive) | **FN** (False Nagative) |
| Actually Not Spam Mail | **FP** (False Positive) | **TN** (True Nagative) |

Precision is a metric that gives you the proportion of true positives to the amount of total positives that the model predicts. It answers the question “**Out of all the positive predictions we made, how many were true?**”

Precision = TP / (TP + FP)


# 2. Recall

| |	Predict as a Fraud |	Predict as Not Fraud |
| :--- | :--- | :--- |
| Actually Fraud | **TP** (True Positive) | **FN** (False Nagative) |
| Actually Not Fraud | **FP** (False Positive) | **TN** (True Nagative) |

Recall focuses on how good the model is at finding all the positives. Recall is also called true positive rate and answers the question “**Out of all the data points that should be predicted as true, how many did we correctly predict as true?**”

