---
layout: post
title: Predicting Popularity of Rock Climbs
---

[Github Repository](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression)



## **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with an estimated 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers (as shown below). With the ever-increasing number of new gym climbers, it is expected that more people will also become interested in outdoor climbing.

![total-gyms-vs-percent-growth.png](https://raw.githubusercontent.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/master/Climbing%20Business%20Journal/Plotly%20Figures/total-gyms-vs-percent-growth.png)

[Mountain Project](https://www.mountainproject.com/) is an online directory in which users can search for information on rock climbs at outdoor areas such as [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) or [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park). Users can also vote on features such the number of stars, the grade, or add it to their tick-list or to-do-list, much like how on [Amazon](https://www.amazon.com/), you can review items, vote on them, and add them to your shopping cart.

As classic areas and popular rock climbs become saturated with new climbers, experienced climbers may become more interested in rock climbs that are not as well known than trying to compete with a crowd. Therefore, as a [data scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I wanted to use the classical machine learning technique of regression to predict the number of users who have a rock climb on their to-do-list. 



## **Data Acquisition**

Some of the data is freely available for download as a csv file from [Mountain Project](https://www.mountainproject.com/route-finder?selectedIds=106064825&type=boulder&diffMinrock=1800&diffMinboulder=20000&diffMinaid=70000&diffMinice=30000&diffMinmixed=50000&diffMaxrock=5500&diffMaxboulder=21700&diffMaxaid=75260&diffMaxice=38500&diffMaxmixed=60000&is_trad_climb=1&is_sport_climb=1&is_top_rope=1&stars=0&pitches=0&sort1=area&sort2=rating). However, this is limited to 1000 rows at a time to limit the online traffic, and only a limited number of features are included: the Average Stars, Length, and Grade. Two other features and the target are only available through the website. Fortunately, the links to the individual pages containing these data are available in the csv file, ensuring that the scraping process will be relatively easy.

To obtain the other features, I wrote a [simple script](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression/blob/master/Scrape%20Website.ipynb) using [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) and added a random time-out so that I didn't get banned from the website. I obtained [data](https://github.com/harrisonized/predicting-popularity-of-rock-climbs-regression/tree/master/Data) for rock climbs in the [Bishop, CA](https://www.mountainproject.com/area/106064825/bishop-area) and [Joshua Tree](https://www.mountainproject.com/area/105720495/joshua-tree-national-park) areas, although I later chose to focus on the Bishop dataset, since it is the most popular climbing destination in California.



## **Exploratory Data Analysis**

To visualize and understand the data that I had, I used the following one-liner to generate a pair plot. This enables me to see how the features relate to each other and determine whether or not I needed to transform any of them for the final modeling.

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

From both the pairplot and the heatmap, it is evident that StarRatings and Ticks are extremely collinear, but since machine-learning is somewhat of a trial-and-error process, I later found that keeping both features improved the model by a marginal amount. Therefore, I chose to keep both as features, even though they largely contain the same information. If speed is valued over performance, then Ticks should be removed.



## **Modeling**

I used a regression model on five features in order to predict the number of people who have a particular boulder problem on their To-Do List. The five features I used are: 1. StarRatings (number of star votes), 2. Ticks (number of completions), 3. Average Stars, 4. Length, and 5. Grade. Note that StarRatings and Ticks are highly collinear, but removing one or the other significantly reduces the model's accuracy.

My final model is an optimized weighted ensemble of Poisson and log-linear regression, which improved the out-of-sample test score (R^2) to 0.842 from 0.643 of the baseline linear regression model.

### **Conclusions**

Having this model is useful, because rock climbs that fewer users have on their to-do-lists than the model prediction should be recommended more to decrease the traffic on rock climbs that have more users on their to-do-lists than expected.