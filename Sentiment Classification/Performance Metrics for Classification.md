# Performance Metrics for Classification
The metrics that you choose to evaluate your machine learning model is very important. Choice of metrics influences how the performance of machine learning algorithms is measured and compared.

Refer to
-   [https://medium.com/thalus-ai/performance-metrics-for-classification-problems-in-machine-learning-part-i-b085d432082b  
    ](https://medium.com/thalus-ai/performance-metrics-for-classification-problems-in-machine-learning-part-i-b085d432082b)
-   [https://becominghuman.ai/understand-classification-performance-metrics-cad56f2da3aa](https://becominghuman.ai/understand-classification-performance-metrics-cad56f2da3aa)

# Confusion Matrix 混淆矩阵

The Confusion Matrix tool aims to help you understand what happens under the hood, and to guide you to further tune Smart Ticket to achieve a satisfactory categorization accuracy.

A Confusion Matrix is basically a table to show users one specific test result. You can find out which categories are good or bad in this test, and then tune the system accordingly.

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/1%20confusion%20matrix.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/1%20confusion%20matrix.png)

The Confusion matrix in itself is not a performance measure as such, but almost all of the performance metrics are based on Confusion Matrix and the numbers inside it.

# Accuracy 准确率

Accuracy in classification problems is the number of correct predictions made by the model over all kinds predictions made.

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/2%20accuracy.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/2%20accuracy.png)

**Note:**
-   Accuracy is a good measure when the target variable classes in the data are nearly balanced.
-   Accuracy should NEVER be used as a measure when the target variable classes in the data are a majority of one class.

# Precision 精确度

Precision is a measure that tells us what proportion of class1 we predicted, actually belongs to class1.

![https://github.com/ShirleyYangGit/Pictures/blob/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/3%20precision.png](https://github.com/ShirleyYangGit/Pictures/blob/master/Sentiment%20Classification/Performance%20Metrics%20for%20Classification/3%20precision.png)

# Recall or Sensitivity 召回率或敏感性

Precision is a measure that tells us what proportion of class1 was predicted to class1.

# Specificity 特异性

Specificity is the exact opposite of Recall or Sensitivity.

# F1-Score

A single score that kind of represents both Precision(P) and Recall(R).
```
F1 Score = Harmonic Mean(Precision, Recall) = 2 * Precision * Recall / (Precision + Recall) 调和平均
```
Harmonic mean is kind of an average when x and y are equal. But when x and y are different, then it’s closer to the smaller number as compared to the larger number.

# ROC Curve

[An ROC curve (receiver operating characteristic curve) is a graph showing the performance of a classification model at all classification thresholds.](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc)

# AUC: Area Under the ROC Curve

[AUC stands for “Area under the ROC Curve.” That is, AUC measures the entire two-dimensional area underneath the entire ROC curve (think integral calculus) from (0,0) to (1,1).](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NDY4ODcwMzIsLTE4MDQxNjExOTddfQ
==
-->