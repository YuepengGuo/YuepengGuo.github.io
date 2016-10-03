---
title: machine learning
date: 2016-10-03 13:52:21
tags: 工具
---
## Measure Model Performance

### ROC Curve

[Receiver operating characteristic](https://en.wikipedia.org/wiki/Receiver_operating_characteristic)
[Receiver operating characteristic in Python](http://scikit-learn.org/stable/auto_examples/model_selection/plot_roc.html)

 It is a plot of the True Positive Rate (on the y-axis) versus the False Positive Rate (on the x-axis) for every possible classification threshold. 
 As a reminder, the True Positive Rate answers the question, 
 "When the actual classification is positive (meaning admitted), how often does the classfier predict positive?" 
 The False Positive Rate answers the question, 
 "When the actual classification is negative (meaning not admitted), how often does the classifier incorrectly predict positive?" 
 Both the True Positive Rate and the False Positive Rate range from 0 to 1.