---
layout: post
title: Predicting Popularity of Rock Climbs
---

[Github Repository](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression)

### **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with a projected 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers.

With the ever-increasing number of new gym climbers, it is expected that more people will also become interested in outdoor climbing. [Mountain Project](https://www.mountainproject.com/) is an online directory in which users can search for information about rock climbs at outdoor areas such as [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) or [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park). Users can also vote on features such the number of stars, the grade, or add it to their tick-list or to-do-list, much like how on [Amazon](https://www.amazon.com/), you can vote on items or add them to your shopping cart.

As classic areas and popular problems become saturated with new climbers, experienced climbers may be more interested in problems that are lesser known than trying to compete with a crowd. Therefore, as a [Data Scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I was interested in using the classical machine learning technique of regression in order to predict the number of users who have a rock climb on their to-do-list. Such a model is useful, because rock climbs that fewer users have on their to-do-lists than the model prediction should be recommended more to decrease the traffic on rock climbs that have more users on their to-do-lists than expected.

### **Data Acquisition and Exploratory Data Analysis**

I scraped data for boulder problems in [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) and [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park).

### **Modeling**

For modeling, I chose to focus on Bishop, since it is the most popular climbing destination in California.

I used a regression model on five features in order to predict the number of people who have a particular boulder problem on their To-Do List. The five features I used are: 1. StarRatings (number of star votes), 2. Ticks (number of completions), 3. Average Stars, 4. Length, and 5. Grade. Note that StarRatings and Ticks are highly collinear, but removing one or the other significantly reduces the model's accuracy.

My final model is an optimized weighted ensemble of Poisson and log-linear regression, which improved the out-of-sample test score (R^2) to 0.842 from 0.643 of the baseline linear regression model