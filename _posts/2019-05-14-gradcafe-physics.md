---
layout: post
title: Predicting PhD Admissions in Physics
---

[Github Repository](https://github.com/harrisonized/predicting-physics-phd-admissions-classification)



## Introduction

Every year, many graduating students are confronted with the choice of whether to go on to graduate studies. I faced this decision myself. In fact, I took the Physics GRE in preparation to apply for Ph.D. programs in physics. Fortunately, I did not end up applying, because I am happy with my choice to become a [Data Scientist](https://www.linkedin.com/in/harrisonized/) through the training program offered by [Metis](https://www.thisismetis.com/).

One online tool that is very useful in monitoring graduate school applications is [The Grad Cafe](https://www.thegradcafe.com/), which is an online forum in which users can self-report their grades, GRE scores, subject GRE scores, and comment as to why they got their results.

Since Ph.D. programs and admissions vary by field, it made sense to focus on analyzing one field at a time. I chose physics, since it is relevant to my own background. Using student grades and GRE scores, I wanted to see how accurately machine learning can predict whether or not a student got into a Ph.D. program and whether I can calculate the probability that a student gets into one given their stats. Such information may be useful for future students who wish to apply.



## Data Acquisition

The data was scraped by Github user [evansjames](https://github.com/evansrjames/) and freely available [here](https://github.com/evansrjames/gradcafe-admissions-data). Although the data is more accessible via the download than just scraping it myself, there is still some work I had to do. I cleaned the data by dropping NaNs, filtering out erroneous data, one-hot-encoding some categorical features, converting numbers from string to float type, and selecting on Ph.D. admissions only. 



## **Exploratory Data Analysis**

To visualize and understand the data that I had, I used the following one-line-wonder from the [Seaborn library](https://seaborn.pydata.org/) to generate a pair plot.

```
sns.pairplot(physics_df, hue = "Decision")
```

![pairplot.png](https://github.com/harrisonized/gradcafe-physics/blob/master/figures/pairplot.png?raw=true)



A pair plot is simply a n-by-n grid, where the diagonals are histograms of the distribution of each variable, and the off-diagonal entries are scatter plots of one variable with another. With the pair plot, I prefer my target to be on the bottom row and right column, and I typically arrange the features in decreasing order of correlation with the target from top to bottom, left to right.

By coloring the hue based on the target, it becomes easy to see that the data is largely non-separable.



## Modeling

In order to potentially increase the separability, I perform some feature transformations. Through trial-and-error, I selected for the best features by only keeping features that improve the roc_auc_scores.

Afterward, I compare the following models to see which performs best. In order from best-to-worst, the models I tried are: random forest, xgboost, decision tree, svc, logistic regression, and Gaussian naive bayes.  Since random forest performed the best, I used a tuning grid in order to limit the tree depth and number of trees to improve model speed.

Finally, I use random forest with untransformed features to derive the relative importance as a sanity check.

My final model is a random forest with an out-of-sample test score (ROC AUC) of 0.698 compared to 0.606 of the baseline Gaussian Naive Bayes model. 



### Conclusions

The low ROC AUC suggests that PhD admissions depend heavily on unavailable data such as letters of recommendations or research experience. The same methods are useful for predicting the likelihood of a user buying an item or for predicting fraud.