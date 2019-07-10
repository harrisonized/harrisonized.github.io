---
layout: post
title: Analyzing Yelp Reviews for Climbing Gyms
---

[Github Repository](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp)

This is a placeholder for Project 4 until I complete my write-up.

For this project, I built my own scraper to collect [Yelp](https://www.yelp.com/) data on climbing gyms in California in JSON format. Used multi-class classification on user reviews to predict the number of stars given by the reviewer. Adjusted the class weights to give minority classes more importance, improving my out-of-sample test accuracy score from 0.635 to 0.867. Also used Natural Language Processing techniques to extract the important topics for 1- and 5-star reviews and reduce dimensionality of the data.