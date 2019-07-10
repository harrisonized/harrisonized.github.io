---
layout: post
title: Predicting Volume of Rent Petitions in San Francisco
---

[Github Repository](https://github.com/harrisonized/sf-rent-petitions-timeseries-analysis)

### **Introduction**

In San Francisco and other major cities in the United States, the cost of rent and the population of renters are both continually increasing at a steady pace. In San Francisco, there is a mechanism in place offered by the [SF Rent Board](https://sfrb.org/) in order to mediate disputes between tenant and landlord for items such as capital improvement requests or wrongful evictions. Since the data freely available through [SF Open Data](https://data.sfgov.org/Housing-and-Buildings/Petitions-to-the-Rent-Board/6swy-cmkq), being able to predict the volume of rent petitions down to the month could be useful for downstream applications, such as for predicting housing prices or number of evictions.

### Data Cleaning

*add information here

### **Modeling**

After grid-searching over parameters, I found that a SARIMA(2, 1, 2)(0, 1, 2)[12] gave me the lowest MAE for a 3-month and 5-year prediction. I also found that using linear regression on the unemployment rate data through [FRED Economic Research](https://fred.stlouisfed.org/series/CASANF0URN) enabled me to get similar results as the SARIMA model.

### **Conclusions**

*add information here