---
layout: post
title: Predicting Volume of Rent Petitions in San Francisco
---

[Github Repository](https://github.com/harrisonized/sf-rent-petitions)

### **Introduction**

In San Francisco and other major cities in the United States, the cost of rent and the population of renters are both continually increasing at a steady pace. In San Francisco, there is a mechanism in place offered by the [SF Rent Board](https://sfrb.org/) in order to mediate disputes between tenant and landlord for items such as capital improvement requests or wrongful evictions. Since the data freely available through [SF Open Data](https://data.sfgov.org/), being able to predict the volume of rent petitions down to the month could be useful for downstream applications, such as for predicting housing prices or number of evictions.



## **Data Acquisition**

In addition to the [Petitions to the Rent Board](https://data.sfgov.org/Housing-and-Buildings/Petitions-to-the-Rent-Board/6swy-cmkq) dataset, I also downloaded data from the [Federal Reserve Bank of St. Louis](https://research.stlouisfed.org/) on the [Unemployment Rate in San Francisco County](https://fred.stlouisfed.org/series/CASANF0URN).



## Data Cleaning

Although the data is freely available, it was by no means clean, and there was still some work that went into the cleaning process. The main sources of error in the data were unusable zip codes, incorrect neighborhood assignments for given zip codes, and duplicate entries.

Duplicate entries are when addresses submit multiple petitions on the same day. For example, 600 Block of Commercial Street submitted 78 petitions on April 30, 2015. I merged such entries into single rows and kept track of the number of occurrences and the unique petition IDs, just in case I needed to access them in future work.



## **Exploratory Data Analysis**

Since I was interested in the count and not any of the individual features, I used the [groupby](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html) function to generate some preliminary plots of the data. To check how different group sizes affected the signal-to-noise ratio of the data, I grouped by days, weeks, months, and years. The following plot below shows the difference clearly.



![petitions-by-date.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-date.png?raw=true)



![petitions-by-week.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-week.png?raw=true)



![petitions-by-month.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-month.png?raw=true)



![petitions-by-year.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/petitions-by-year.png?raw=true)



I found that grouping by month had the right balance between signal and noise that would work well for modeling. This is also an ideal choice for my purposes, since the data for the Unemployment Rate in San Francisco County, shown below, is also monthly.

![unemployment-by-month.png](https://github.com/harrisonized/sf-rent-petitions/blob/master/figures/eda/unemployment-by-month.png?raw=true)

While it would be great to get a daily prediction, the fact is like with any statistical task, that when zoomed in too far, the modeling breaks down. For example, it is near impossible to predict the exact timing of each person entering and exiting Starbucks, but it is easy to predict how many will enter within the range of an hour.



## **Isolating the Effect**

Before modeling, I wanted to see if I could reduce the noise, even if just by a little bit.



## **Modeling using Sarima**

After grid-searching over parameters, I found that a SARIMA(2, 1, 2)(0, 1, 2)[12] gave me the lowest MAE for a 3-month and 5-year prediction.



## **Modeling using Linear Regression**

I also found that using linear regression on the unemployment rate data through [FRED Economic Research](https://fred.stlouisfed.org/series/CASANF0URN) enabled me to get similar results as the SARIMA model.



## **Conclusions**

