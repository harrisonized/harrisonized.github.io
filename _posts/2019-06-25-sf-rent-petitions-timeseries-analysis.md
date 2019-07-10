---
layout: post
title: Predicting Volume of Rent Petitions in San Francisco
---

[Github Repository](https://github.com/harrisonized/sf-rent-petitions-timeseries-analysis)

This is a placeholder for Project 5 until I complete my write-up.

For this project, I predicted the volume of rent petitions in San Francisco using the data freely available through [SF Open Data](https://data.sfgov.org/Housing-and-Buildings/Petitions-to-the-Rent-Board/6swy-cmkq). After a short grid-search, I found that a SARIMA(2, 1, 2)(0, 1, 2)[12] gave me the lowest MAE for a 3-month and 5-year prediction. I also found that using linear regression on the unemployment rate data through [FRED Economic Research](https://fred.stlouisfed.org/series/CASANF0URN) enabled me to get similar results as the SARIMA model.