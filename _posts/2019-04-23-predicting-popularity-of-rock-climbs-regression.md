---
layout: post
title: Predicting Popularity of Rock Climbs
---

[Github Repository](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression)



## **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with an estimated 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers (as shown below). With the ever-increasing number of new gym climbers, it is expected that more people will also become interested in outdoor climbing.

![total-gyms-vs-percent-growth.png](https://raw.githubusercontent.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/master/Climbing%20Business%20Journal/Plotly%20Figures/total-gyms-vs-percent-growth.png)

[Mountain Project](https://www.mountainproject.com/) is an online directory in which users can search for information on rock climbs at outdoor areas such as [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) or [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park). Users can also vote on features such the number of stars, the grade, or add it to their tick-list or to-do-list, much like how on [Amazon](https://www.amazon.com/), you can review items, vote on them, and add them to your shopping cart.

As classic areas and popular rock climbs become saturated with new climbers, experienced climbers may become more interested in rock climbs that are not as well known than trying to compete with the crowd. Therefore, as a [data scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I wanted to use the classical machine learning technique of regression to predict the number of users who have a rock climb on their to-do-list. 



## **Data Acquisition**

Some of the data is freely available for download as a csv file from [Mountain Project](https://www.mountainproject.com/route-finder?selectedIds=106064825&type=boulder&diffMinrock=1800&diffMinboulder=20000&diffMinaid=70000&diffMinice=30000&diffMinmixed=50000&diffMaxrock=5500&diffMaxboulder=21700&diffMaxaid=75260&diffMaxice=38500&diffMaxmixed=60000&is_trad_climb=1&is_sport_climb=1&is_top_rope=1&stars=0&pitches=0&sort1=area&sort2=rating). However, this is limited to 1000 rows at a time to limit the online traffic, and only a limited number of features are included: the Average Stars, Length, and Grade. Two other features and the target (On-To-Do-List) are only available through the website. Fortunately, the links to the individual pages containing these data are available in the csv file, ensuring that the scraping process will be relatively easy.

To obtain the remaining features, I wrote a [simple script](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression/blob/master/Scrape%20Website.ipynb) using [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) and added a random time-out so that I didn't overload the website. I obtained [data](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression/tree/master/Data) for rock climbs in the [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) and [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park) areas, although I later chose to focus on the Bishop dataset, since it is the most popular climbing destination in California.



## **Exploratory Data Analysis**

To visualize and understand the data that I had, I used the following one-line-wonder from the [Seaborn library](https://seaborn.pydata.org/) to generate a pair plot. This enabled me to see how the features relate to each other and determine whether or not I needed to transform any of them for the final modeling.

```python
sns.pairplot(bishop_proc_df[features_target_list])
```

![img](https://raw.githubusercontent.com/harrisonized/predicting-popularity-of-rock-climbs-regression/master/Figures/Bishop/EDA/Baseline%20Pairplot.png)



I also generated a heat map to check for co-linearity.

```python
# Plot heatmap for baseline case.
plot_heat_map(bishop_proc_df.loc[:,['StarRatings', 'Ticks', 'Avg Stars', 'Length', 'Grade', 'OnToDoLists']])
ax.set_title('Correlation Heatmap for True Baseline Case', fontsize = 18)
```

![Correlation Heatmap.png](https://raw.githubusercontent.com/harrisonized/predicting-popularity-of-rock-climbs-regression/master/Figures/Bishop/EDA/Correlation%20Heatmap.png)

From both the pairplot and the heatmap, it is evident that StarRatings and Ticks are collinear. However, though trial-and-error, I found that keeping both features improved the model by a marginal amount. If speed is valued over performance, then Ticks can be removed from the list of features.



## **Initial Linear Regression**

I built an initial model based on the untransformed variables to obtain a measure of baseline performance. Any subsequent model I build must improve upon this or else be discarded.

In adherence to best practices, I used sklearn's [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html) function to set aside 20% of the data, which **must** remain untouched while training the model. Testing the model on this out-of-sample test set will provide a reasonable estimate for how well the model generalizes to other out-of-sample data.

Of the remaining 80% of the data, I used sklearn's [KFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html) function to make a 5-fold cross-validation loop with an 80-20 split. In other words, this function partitions the data five times, enabling me to train on 80% (64% of my total data) and validate on the remaining 20% (16% of my total data). KFold splits the data in such a way that none of the 20% in each validation set overlaps with the next, allowing equal representation of all data points. After training and validating on each fold, I can obtain a score to measure how well my model performs on in-sample data, which in this case is the R^2 validation score. Finally, I train on the entire 80% of the data and test on the first 20% I set aside to obtain the R^2 test score for out-of-sample data.



## **Measuring Model Performance**

The R^2 value is a good metric for regression problems, because it is a measure of how well my model captures the variance of my data. It is defined as as 1-SSE/SST, where SSE (Sum of Squares Error) is a measure of how far my predictions deviate from the actual data, and SST (Sum of Squares Total) is a measure of how much the data deviates from the mean. A perfect model (or perfect data) would theoretically have an R^2 of 1, while a model that is completely erroneous could have a negative R^2. Doing cross-validation is a good way to ensure that my validation score is not arbitrarily high or low simply because of the way the data was split.

For this task, I chose to use both sklearn's [LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) function and Statsmodel's [OLS](https://www.statsmodels.org/devel/generated/statsmodels.regression.linear_model.OLS.html), just to compare the two libraries. I found that I prefer [Statsmodels](https://www.statsmodels.org/devel/index.html) over sklearn for regression problems, because it provides other statistics alongside the main model. There are other nuances I'll elaborate on later. As expected, the models from both libraries gave the same scores.

```python
# Sklearn's Linear Regression
val_r2_score:  0.7850858488678168  +/-  0.06682093878949044
test_r2_score:  0.6426057149839702
```

```python
# Statsmodels OLS
val_r2_score:  0.7850858488678172  +/-  0.06682093878949083
test_r2_score:  0.642605714983973
```



## **Lasso and Ridge**

One way to avoid over-fitting is to add a penalty function in the form of regularization.





## **Improved Models**



Because the pairplot shows that not all features are linearly related to the target, it is advantageous to transform the data using the log and square root transforms to linearize the features with respect to the target. Since the data was relatively small, I decided to generate these features and save them in a separate csv file so as to only generate them once during the life-cycle of this project.







for evaluating how well the model performs on in-sample validation and out-of-sample test





I used a regression model on five features in order to predict the number of people who have a particular boulder problem on their To-Do List. The five features I used are: 1. StarRatings (number of star votes), 2. Ticks (number of completions), 3. Average Stars, 4. Length, and 5. Grade. Note that StarRatings and Ticks are highly collinear, but removing one or the other significantly reduces the model's accuracy.

My final model is an optimized weighted ensemble of Poisson and log-linear regression, which improved the out-of-sample test score (R^2) to 0.842 from 0.643 of the baseline linear regression model.

### **Conclusions**

Having this model is useful, because rock climbs that fewer users have on their to-do-lists than the model prediction should be recommended more to decrease the traffic on rock climbs that have more users on their to-do-lists than expected.