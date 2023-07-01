# ConfusionMatrix
Precision Versus Recall - Essential Metrics in Machine Learning

# 1. Precision

| |	Predict as a Spam Mail |	Predict as Not Spam Mail |
| :--- | :--- | :--- |
| Actually Spam Mail | **TP** (True Positive) | **FN** (False Nagative) |
| Actually Not Spam Mail | **FP** (False Positive) | **TN** (True Nagative) |

Precision is a metric that gives you the proportion of true positives to the amount of total positives that the model predicts. It answers the question “**Out of all the positive predictions we made, how many were true?**”

```
Precision = TP / (TP + FP)
```
If you get this value close to 100%, FP should be reduced. In other words, you should eliminate false positives by refurbishing the algorithm which should not mark non-spam as spam.

# 2. Recall

| |	Predict as a Fraud |	Predict as Not Fraud |
| :--- | :--- | :--- |
| Actually Fraud | **TP** (True Positive) | **FN** (False Nagative) |
| Actually Not Fraud | **FP** (False Positive) | **TN** (True Nagative) |

Recall focuses on how good the model is at finding all the positives. Recall is also called true positive rate and answers the question “**Out of all the data points that should be predicted as true, how many did we correctly predict as true?**”

```
Recall = TP / (TP + FN)
```
If you get this value close to 100%, FN should be reduced. If the fraud detection does not work well, then it goes through your bank account and defraud your property. So, False Negative should be 0 as possible as you can do.


# 3. Which index should you use ?
I believe it depends on on the purpose of each model. <br>
# Cases Presision is prefered
For example, low quality mail filtering algorithm might put the non-spam mail on the spam mail box. I mean it is the case of False Negative. It is acceptable and you can improve it through some engineerings. But what do you think of if non-spam mail has gone to the spam mail box? This is very serious case and it is not torelate. So we should take care of False Positive case rather than False Negative and FP should be small as possible as you can. 

# Cases Recall is prefered
Let's say the case of Cancer diagnosis. You must not like the case if you have a cancer in your stomach actually, but a doctor doesn't say anything and he or she says you're healthy. Of cource, doctors might sometimes have mistakes. If the mistake is False Positive, it can be toralate because you're not gonna die.<br>
But how do you feel, if the mistake is False Negative. It must be too late to go to hospital. So you should take care of reducing the False Negative case rather than False Positive.<br>
Again, you should think of the worst possible outcome in a system  supposed to be detecting cancer. You want to be airing on the side of false positive.
