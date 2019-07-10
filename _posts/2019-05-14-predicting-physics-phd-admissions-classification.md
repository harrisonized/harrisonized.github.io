---
layout: post
title: Predicting Physics PhD Admissions
---

[Github Repository](https://github.com/harrisonized/predicting-physics-phd-admissions-classification)

This is a placeholder for Project 3 until I complete my write-up.

Used self-reported post-admission results from [The Grad Cafe](https://www.thegradcafe.com/) to predict the likelihood of whether a student would be admitted based on their grades and GRE scores. My final model is a random forest with an out-of-sample test score (ROC AUC) of 0.698 compared to 0.606 of the baseline Gaussian Naive Bayes model. This suggests that PhD admissions depend heavily on unavailable data such as letters of recommendations or research experience. The same methods are useful for predicting the likelihood of a user buying an item or for predicting fraud.