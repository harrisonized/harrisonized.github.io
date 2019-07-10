---
layout: post
title: Analyzing Yelp Reviews for Climbing Gyms
---

[Github Repository](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp)

### **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with a projected 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers. In fact, my friend [Shirley Yang](https://www.linkedin.com/in/shirley-yang-01a40a13/) is opening a [gym](https://www.agilityboulders.com/) of her own. As a [Data Scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I wondered if there was any data-driven business insights I could deliver to her using my toolbox of data science techniques.

### **Data Acquisition and Exploratory Data Analysis**

I built my own scraper to collect [Yelp](https://www.yelp.com/) data on climbing gyms in California in JSON format. 

### **Classifying Reviews**

Used multi-class classification on user reviews to predict the number of stars given by the reviewer. Adjusted the class weights to give minority classes more importance, improving my out-of-sample test accuracy score from 0.635 to 0.867.

### **Topic Modeling**

I extracted the important topics for 1- and 5-star reviews and reduce dimensionality of the data.

### **Conclusions**

If you're seeking to open a new climbing gym, here are some do's and don'ts.

Do: 1. Make the experience of day-pass users enjoyable. 2. Try to increase the membership rate.

Don't: 1. Hire rude staff. 2. Throw birthday parties.