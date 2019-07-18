---
layout: post
title: Predicting Volume of Rent Petitions in San Francisco
---

[Github Repository](https://github.com/harrisonized/sf-rent-petitions)

### **Introduction**

In San Francisco and other major cities in the United States, the cost of rent and the number of renters are both continually increasing at a steady pace. With the worsening of the housing crisis, San Francisco offers a mechanism through the [SF Rent Board](https://sfrb.org/) in order to mediate disputes between tenant and landlord for items such as capital improvement requests or wrongful evictions.

Being able to predict the volume of rent petitions could be useful, since if you are a landlord or tenant, this feature will be informative of the turnaround time for processing requests, or if you are in a more niche business and need to predict the number of capital improvement requests or housing prices, then the number of rent petitions could be an important feature in your machine learning model.



## **Data Acquisition**

In addition to the [Petitions to the Rent Board](https://data.sfgov.org/Housing-and-Buildings/Petitions-to-the-Rent-Board/6swy-cmkq) dataset, I also downloaded data from the [Federal Reserve Bank of St. Louis](https://research.stlouisfed.org/) on the [Unemployment Rate in San Francisco County](https://fred.stlouisfed.org/series/CASANF0URN).



## Data Cleaning

Although the data is freely available, it was by no means clean, and there was still some work that went into the cleaning process. The main sources of error in the data were unusable zip codes, incorrect neighborhood assignments for given zip codes, and duplicate entries.

Duplicate entries are when single addresses submit multiple petitions on the same day. For example, on April 30th, 2015, a property at 600 Block of Commercial Street submitted 78 petitions. I merged such entries into single rows and kept track of the number of occurrences and the unique petition IDs, just in case I needed to access them in future work.



## **Grouping Counts by Date**

Since I was interested in the count and not any of the individual features, I used the [groupby](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html) function to generate some preliminary count plots of the data for all of SF. To check how different group sizes affected the signal-to-noise ratio of the data, I grouped by days, weeks, months, and years. The following plot below shows the difference between the groupings.

![petitions-by-day.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-day.png?raw=true)



![petitions-by-week.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-week.png?raw=true)



![petitions-by-month.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-month.png?raw=true)



![petitions-by-year.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-year.png?raw=true)



I found that grouping by month had the right balance between signal and noise that would work well for modeling. This is also an ideal choice for my purposes, since the data for the Unemployment Rate in San Francisco County, shown below, is also monthly.

![unemployment-by-month.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/unemployment-by-month.png?raw=true)

While it would be great to get a daily prediction, the fact is like with any statistical task, that when zoomed in too far, the modeling breaks down. For example, it is near impossible to predict the exact timing of each person entering and exiting Starbucks, but it is relatively easy to predict how many will enter within the range of an hour.



## **Grouping Counts by Neighborhood**

The following is a [Tableau](https://public.tableau.com/en-us/s/) plot of the 40 neighborhood groupings provided in the dataset.

 ![all-neighborhoods.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/tableau/all-neighborhoods.png?raw=true)



Since the block addresses and neighborhoods for all entries are provided in the dataset, I wanted to see if certain neighborhoods produce more rent petitions than others. This is a reasonable guess, since the [Mission District](https://en.wikipedia.org/wiki/Mission_District,_San_Francisco) is [well known for activism](https://reason.com/2018/10/25/san-francisco-activists-oppose-plans-to/) surrounding renters' rights; presumably the Mission District contributes more to the overall trend than do other neighborhoods. The following bar plot shows that, as expected, the Mission District produces double or more of the number of rent petitions per month of any other neighborhood.



![mean-rent-petitions-by-neighborhood.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/neighborhood-correlation/mean-rent-petitions-by-neighborhood.png?raw=true)



The following figure shows that the trend for the Mission District is very similar as the trend for all of San Francisco. From now on, I will call neighborhoods with significant trend as time-varying.

![petitions-for-mission.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/neighborhood/petitions-for-mission.png?raw=true)



Just as an example, the following is a neighborhood that does not have much trend at all, which I will call stationary.

![petitions-for-financial-district-south-beach.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/neighborhood/petitions-for-financial-district-south-beach.png?raw=true)



Since the Mission District is a main component of the overall trend, I found that the correlation with the Mission District could be used to group together neighborhoods that have a significant time-varying component. In the following bar plot, I labeled any neighborhoods that had significant correlation with the mission district as blue.



![correlation-with-mission.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/neighborhood-correlation/correlation-with-mission.png?raw=true)

After collecting the neighborhoods in these two groups, I replotted the data and found that indeed, I had separated out the neighborhoods that had no trend.

![time-varying-vs-stationary-neighborhoods.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/neighborhood-correlation/time-varying-vs-stationary-neighborhoods.png?raw=true)

Let's see what this looks like on a geographic level.

![time-varying-vs-stationary.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/tableau/time-varying-vs-stationary.png?raw=true)

As expected, the neighborhoods that are stationary are in the sections of the city where renting is relatively rare due to the abundance of businesses and lack of homes. With the neighborhoods reasonably clustered, I am ready to begin [forecasting](https://en.wikipedia.org/wiki/Forecasting). The technique of [time-series analysis](https://en.wikipedia.org/wiki/Time_series) forms the final component of my Data Science portfolio.



## **Data Preparation**

For the neighborhoods that are grouped as seasonal, first, I used statsmodel's [seasonal decomposition](https://www.statsmodels.org/stable/generated/statsmodels.tsa.seasonal.seasonal_decompose.html) function to remove the seasonal component, so that I'm only dealing with the trend and residual.

```python
petition_decomp = seasonal_decompose(petition_counts_by_neighborhood_df[seasonal_neighborhoods_list].sum(axis=1), model='additive', extrapolate_trend = True)
```

![seasonal-decomp.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/seasonal-decomp.png?raw=true)

Second, I set aside some data for the test set that will remain untouched while training the model. I found that 20% of the data occurs after January 1, 2014, so I split the dataset into a training set of data prior to this date and a test set of data after this date.

![time-varying-neighborhoods-train-test-split.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/train-test-split.png?raw=true)



## **Forecasting Using SARIMA**

[SARIMA](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average) is an industry standard for doing time-series analysis. SARIMA stands for Seasonal Auto-Regressive Moving Average, which is a mouthful! Rather than divert this blog post to explain the underlying math, I think [this tutorial by Kostas Hatalis](https://www.datasciencecentral.com/profiles/blogs/tutorial-forecasting-with-seasonal-arima) is an excellent resource. It explains what SARIMA is and provides some good rules-of-thumb on how to pick the correct parameters. For a more detailed explanation on ARIMA models in general, I recommend Chapter 3 of [Shumway and Stoffer](https://www.stat.pitt.edu/stoffer/tsa4/index.html), which is freely available for download.

The thing to keep in mind is that SARIMA has seven components, typically written as follows:

```
SARIMA(p,d,q)(P,D,Q)[s]
```

where each of the components p, d, q, P, D, Q, and s are integers describing different parts of the equation. The success of the SARIMA model will depend on picking the correct parameters. The best model will give the lowest mean average error for the test set.

To pick out initial parameters to try, we need to look at the [autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation) function (ACF) and [partial autocorrelation function](https://en.wikipedia.org/wiki/Partial_autocorrelation_function) (PACF).

![acf-pacf.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/acf-pacf.png?raw=true)

In the plot above, the shaded blue area represents the significance level. Anything outside of the shaded blue is significant.

Looking at the ACF on the top, we can deduce the following:

1. q = 1, the first significant lag
2. s = 15, the highest significant lag
3. Q = 0  and P≥1, because the ACF is positive at lag of 15
4. D = 1, because the seasonal pattern has been removed using seasonal decomposition

In order to figure out the remaining parameters of p and d, we need to look at the PACF on the bottom. However, since the significance level is missing from the PACF above, we need to generate the PACF for the differenced series and look at that instead.

![acf-pacf-diff.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/acf-pacf-diff.png?raw=true)



Looking at the PACF on the bottom for the differenced series, we can deduce that:

1. p = 1, the first significant lag,
2. d = 1, since this is a differenced series.

Collecting our results, this means that the first model we should try is a SARIMA(1, 1, 1)(2, 1, 0)[15].

```python
mod = sm.tsa.statespace.SARIMAX(petition_train_df[['Datetime', 'Adj. Signal']].set_index("Datetime"),
                                order=(1, 1, 1),
                                seasonal_order=(2, 1, 0, 15),
                                enforce_stationarity=False,
                                enforce_invertibility=False,
                               freq = 'M')
results = mod.fit()
```

![sarima-baseline-model.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/sarima-baseline-model.png?raw=true)

A short grid search on the parameters p, q, and s reveals that this is indeed the best model. However, like with SARIMA models in general,it's pretty apparent from the graph that the predictions are lagging behind the actual data. This is especially apparent in the training set, but you can also see this effect in the test set as well.

How can we fix this issue?

One way is to model on the trend alone, and then add back in the seasonal component. Unfortunately, we will not be able to add back the residual, but if we can get rid of the lag, maybe this is okay. After grid-searching over parameters, I found that a SARIMA(2, 1, 2)(0, 1, 2)[12] gave me the lowest MAE for a 3-month and 5-year prediction.

![sarima-model-on-trend.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/sarima/sarima-model-on-trend.png?raw=true)







## **Forecasting using Linear Regression**

I also found that using linear regression on the unemployment rate data through [FRED Economic Research](https://fred.stlouisfed.org/series/CASANF0URN) enabled me to get similar results as the SARIMA model.



![correlation-vs-lag.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/linreg/correlation-vs-lag.png?raw=true)







![lr-on-rent-petition-vs-unemployment.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/linreg/lr-on-rent-petition-vs-unemployment.png?raw=true)



Finally, here is the model that results from the above work.

![lr-on-time-lagged-unemployment-data.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/linreg/lr-on-time-lagged-unemployment-data.png?raw=true)



## **Results**

The following table summarizes the results of the modeling.

| Model                             | Benefits                    | MAE<br />(3 Months) | MAE<br />(5 Years) |
| :-------------------------------- | --------------------------- | ------------------- | ------------------ |
| Baseline Sarima                   | Easy to implement           | 5.7                 | 18.4               |
| Sarima on Trend                   | Most accurate               | 7.8                 | 12.3               |
| Linear Regression on Unemployment | Fastest, most interpretable | 5.1                 | 13.6               |



## **Conclusions**

*Add conclusion section