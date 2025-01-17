# Confusion Matrixの考え方

ここでは、SageMaker Autopilotが提供するメトリクスとレポートで扱われるConfusion Matrixについて解説します。メトリクスとレポートには、Confusion Matrix、Area under the receiver operating characteristic (AUC-ROC) 曲線および Area under the precision-recall curve (AUC-PR) 曲線が含まれるのですが、Confusion Matrix は、分類問題の結果をまとめた行列（Matrix）のことで、なかなかその意味と活用の方法を理解することが難しいので、実例とともに解説したものになります。

# 主な用語
Acuracy, Precision and Recall - Essential Metrics in Machine Learning

# 1. Accuracy
精度は、分類されたクラスと正しいクラスに一致するサンプルの数の比率です。したがって、次のように定義できます。

---
Accuracy is a ratio of the number of samples that match the classified class and the correct class. So we can define it as following:
```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```
精度は直感的で理解しやすく解釈しやすいため、とても扱いやすいの指標の一つです。しかし、精度のみで予測結果を評価すると、悪いモデルを良いモデルと誤判定してしまう危険性があります。 <br>
実際に、陽性/陰性分類モデルと、2 つの陽性データと 98 の陰性データを含むデータセットがあるとします。そしてこのとき、分類するモデルが「このデータ 100 個すべてが陰性である」と予測したとします。
準備したとおり、100 個のデータのうち 98 個が元々陰性であるため、精度は 98% です。この精度だけ見るとかなり良い数値に見えます。ただし、分類モデルは意味のある予測を実行できていないことがわかります。 <br>
このようにデータが偏っていて「全て陰性」と予測されると、意に反して精度の値が高くなってしまいます。これらの問題に対処するために、Precision、Recall、および F-Score と呼ばれる次の指標を参照します。

Accuracy is a popular metric because it is intuitive and easy to understand and interpret. However, if prediction results are evaluated only by Accuracy, there is a risk that a bad model may be mistakenly judged as a good model. <br>
Suppose you have a positive/negative classification model and a dataset including two positive data and 98 negative data, actually.
And at this time, suppose that the model to classify predicts that "all 100 of this data are negative". <br>
The accuracy rate is definitely 98% because 98 out of 100 data are originally negative as you prepared. If you look only at the numerical value (accuracy), it looks like a pretty good number. However, the classification models could not perform any meaningful prediction. It can not be said that the classification accuracy is high with this. <br>
If the data are imbalanced as described above and is predicted that "all are negative", the Accuracy value will be high against one's will. In order to deal with these problems, we refer to the following indicators called Precision, Recall, and F-Score.

# 2. Precision (for Eliminate waste)

| |	Predict as a Spam Mail |	Predict as Not Spam Mail |
| :--- | :--- | :--- |
| Actually Spam Mail | **TP** (True Positive) | **FN** (False Nagative) |
| Actually Not Spam Mail | **FP** (False Positive) <br>-> waste | **TN** (True Nagative) |

Precisionは、モデルが予測する陽性者の総量に対する真の陽性の割合を示す指標です。 「**私たちが行ったすべての陽性な予測のうち、いくつ正しく分類ができたか?**」という質問のように解釈できます。

---
Precision is a metric that gives you the proportion of true positives to the amount of total positives that the model predicts. It answers the question “**Out of all the positive predictions we made, how many were true?**”

```
Precision = TP / (TP + FP)
```
この値が 100% に近づくと、FP が減少するはずです。言い換えれば、スパム以外をスパムとしてマークしないようアルゴリズムを刷新することで、誤検知を排除する必要があります。 <br>
Precisionは、scikit-learn の **sklearn.metrics.precision_score()** で計算できます。

---
If you get this value close to 100%, FP should be reduced. In other words, you should eliminate false positives by refurbishing the algorithm which should not mark non-spam as spam. <br>
Precision can be calculated with **sklearn.metrics.precision_score()** in scikit-learn.

# 3. Recall (for Eliminate lost opportunities and to emphasize identifying as many actual failures as possible)

| |	Predict as a Fraud |	Predict as Not Fraud |
| :--- | :--- | :--- |
| Actually Fraud | **TP** (True Positive) | **FN** (False Nagative) <br>-> lost opportunity |
| Actually Not Fraud | **FP** (False Positive) | **TN** (True Nagative) |

Recall focuses on how good the model is at finding all the positives. Recall is also called true positive rate and answers the question “**Out of all the data points that should be predicted as true, how many did we correctly predict as true?**” 

```
Recall = TP / (TP + FN)
```
If you get this value close to 100%, FN should be reduced. If the fraud detection does not work well, then it goes through your bank account and defraud your property. So, False Negative should be 0 as possible as you can do. <br>
Recall can be calculated with **sklearn.metrics.recall_score()** in scikit-learn.

# 4. Which metric should you use ?
I believe it depends on on the purpose of each model. <br>
# Cases Presision is prefered
For example, low quality mail filtering algorithm might put the spam mail on your mail box. I mean it is the case of False Negative. It is acceptable and you can improve it through some engineerings. But what do you think of if non-spam mail has gone to the spam mail box? This is very serious case and it is not torelate. So we should take care of False Positive case rather than False Negative and FP should be small as possible as you can. 

# Cases Recall is prefered
Let's say the case of Cancer diagnosis (Of course fraud is also fine). You must not like the case if you have a cancer in your stomach actually, but a doctor doesn't say anything and he or she says you're healthy. Of cource, doctors might sometimes have mistakes. If the mistake is False Positive, it can be toralate because you're not gonna die.<br>
But how do you feel, if the mistake is False Negative. It must be too late to go to hospital. So you should take care of reducing the False Negative case rather than False Positive.<br>
Again, you should think of the worst possible outcome in a system  supposed to be detecting cancer. You want to be airing on the side of false positive.<br> 

# 5. F-Score
F-score is **[the Harmonic mean](https://en.wikipedia.org/wiki/Harmonic_mean)** (reciprocal of mean of reciprocal) of Recall and Precision. Also called F-measure or F1-score. <br>
In fact, Recall and Precision are in a trade-off relationship, and if one is raised, the other is lowered.
For example, when actively predicting "Positive" to increase Recall, it is actually Positive but is mistaken as Negative (FN: False Negative). Since the number of (FP: False Positive) will increase, the Precision will decrease.In other words, if you try to raise Recall and say to everyone, "You have cancer!" <br>
So, even if you look at Recall and Precision alone, you can't say whether the accuracy is good or bad. That's where the F value comes from. The F-value is the harmonic mean taking into account this trade-off.

```
F-Score = 2 / { (1/Precision) + (1/Recall) } = 2TP / (2TP + FP + FN)
```
F-Score can be calculated with **sklearn.metrics.f1_score()** in scikit-learn.<br>  

# 6. Resampling
- Under sample majority class
- Over sample minority class

# 7. When to Use ROC vs. Precision-Recall Curves?
Generally, the use of ROC curves and precision-recall curves are as follows:

- ROC curves should be used when there are roughly equal numbers of observations for each class.
- Precision-Recall curves should be used when there is a moderate to large class imbalance.
  
> https://www.softbanktech.co.jp/special/blog/dx_station/2022/0016/

# 8. Drift
An example of conceptual drift is when a machine learning model for detecting spam on SNS was put into operation and was initially able to detect it with a high accuracy rate, but after half a year, it could hardly be detected. Let's assume that the cause is that spammers have come up with new ways to avoid being detected as spam (assuming the distribution of input data remains unchanged). In other words, the cause of the drift is that the concept of spam, which is the correct label (target variable), has changed.
