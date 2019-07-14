---
layout: post
title: Analyzing Yelp Reviews for Climbing Gyms
---

[Github Repository](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp)



## **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with an estimated 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers (shoutout to my friend [Shirley Yang](https://www.linkedin.com/in/shirley-yang-01a40a13/), who is opening a [gym](https://www.agilityboulders.com/) of her own). As a [Data Scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I wondered if there was any data-driven business insights I could deliver to her using my toolbox of data science techniques.

![total-gyms-vs-percent-growth.png](https://raw.githubusercontent.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/master/climbing-business-journal/plotly-figures/total-gyms-vs-percent-growth.png)

[Yelp](https://www.yelp.com/) is an online directory in which users can publish reviews on businesses. It is often a great source of information for first-time visits to new areas. Using Yelp reviews, I wanted to see if I could use some natural language processing techniques to be able to categorize reviews and come up with a list of do's and don'ts for climbing gyms in California.



## **Data Acquisition**

Since Yelp releases a new [competition dataset](https://www.yelp.com/dataset/challenge) each year in [JSON](https://www.json.org/) format, this was the right place to start. The entries in the competition dataset are structured as follows.

```python
# Review
 {'review_id': 'vgXITaQ2uK28PMET91KuVg',
  'user_id': '8dpqRMuHRnPwJPzyUp1nwA',
  'user_name': 'Kristina S.',
  'business_id': 'ImFFWa6UEu_fhfODG1yBiQ',
  'stars': 4,
  'useful': 2,
  'funny': 0,
  'cool': 0,
  'text': "I really loved this place!! We came here to celebrate my cousin's birthday. What better way to celebrate a birthday than to climb some rocks. Lol. Anyway, parking was no problem, and it's pretty easy to find. We checked in, signed some waivers then took our beginner course with Jonny, since we are all noobs. He was very nice and helpful. Everything made sense and in 30 minutes or so, my group and I were confident to take on the rocks!If you're looking to have some fun and also to challenge yourself (because rock climbing isn't easy), you should check this place out. I'd love to come back.Deducted a star because some of the ropes are a little smelly. I feel like they should clean more often? or replace more often. Just a suggestion for management. Other than that, this place would've been a solid 5 stars.",
  'date': '3/18/2017'}

# Business
{'business_id': 'ImFFWa6UEu_fhfODG1yBiQ',
  'link_id': 'diablo-rock-gym-concord-3',
  'name': 'Diablo Rock Gym',
  'address': '1220 Diamond Way Ste 140',
  'city': 'Concord',
  'state': 'CA',
  'postal_code': '94520',
  'telephone': '+19256021000',
  'latitude': 37.9655526,
  'longitude': -122.0532716,
  'stars': 4.0,
  'review_count': 144,
  'is_open': 1,
  'attributes': {'Accepts Credit Cards': 'Yes',
   'Parking': 'Street, Private Lot',
   'Bike Parking': 'Yes',
   'Good for Kids': 'Yes',
   'Dogs Allowed': 'No'},
  'categories': 'Gyms, Yoga, Climbing',
  'hours': {'Mon': '5:30 am - 10:00 pm',
   'Tue': '5:30 am - 10:00 pm',
   'Wed': '5:30 am - 10:00 pm',
   'Thu': '5:30 am - 10:00 pm',
   'Fri': '5:30 am - 10:00 pm',
   'Sat': '9:00 am - 6:00 pm',
   'Sun': '9:00 am - 6:00 pm'}}
```



This is great, because all of the files contain the business_id, which can be used to join them together if necessary. However, this also has some drawbacks, since the data is truncated starting the beginning of the year, and I wanted data that was up-to-date.

Next, I looked into Yelp's newest API, called [Yelp GraphQL](https://www.yelp.com/developers/graphql/guides/intro), which enables users to enter [queries](https://www.yelp.com/developers/graphql/query/search) and return the results all in JSON. Yelp even has an [online console](https://www.yelp.com/developers/graphiql) where you can send such queries. For data science projects, it is also easy to use an [ipython notebook](http://localhost:8888/notebooks/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/yelp-graphql-api.ipynb) to make queries after a simple [setup](https://www.youtube.com/watch?v=Wdy-9daEd0o).

Unfortunately, there was an enormous drawback to using the Yelp API, which I found out about it only after hours of setup and learning about GraphQL: Yelp's API is *extremely* [limiting](https://www.yelp.com/developers/graphql/guides/rate_limiting). There is a hard limit of 3 results per query, and review text is limited to 150 characters per review. This is so much less data than simply accessing the data as html through the website that it makes the Yelp API effectively useless to any normal user. I make special mention of this limitation, because it was not obvious at all from reading Yelp's documentation, and hopefully, you did not also go through the same painful process only to be unrewarded in the end.

Because of these limitations, I found that the only easy way to access the data that I wanted was just to scrape it using [Beautifulsoup4](https://www.crummy.com/software/BeautifulSoup/bs4/doc/). I sought to stay as close as possible to the format that Yelp already made available through their challenge dataset, so that it would be as if I was working with data that Yelp handed to me directly.

Here are the scrapers I built for [reviews](http://localhost:8888/notebooks/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/scrape-yelp-for-reviews.ipynb) and for [business](http://localhost:8888/notebooks/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/scrape-yelp-for-business-information.ipynb) information, in case you would like to adapt them to your purposes. In order to use them, I made a [list of urls](http://localhost:8888/edit/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/data/url-list.txt) of all the climbing gyms in California, which I sought to analyze. Finally, for easy access, I [concatenated](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/concatenate-yelp-review-data.ipynb) the data so I could do some exploratory data analysis.



## **Exploratory Data Analysis**

To understand what data I would be working with, I made some simple visualizations and learned the following facts..

1. The total number of reviews had been declining in the last three years. In my opinion, the most-likely reason is that new visitors do not feel the need to add more reviews to the already-existing corpus.

![number-of-reviews-by-year.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/number-of-reviews-by-year.png?raw=true)

2. Reviews are skewed heavily toward 5-star reviews. This is reflective of the overall [trend](https://minimaxir.com/2014/09/one-star-five-stars/) on Yelp in general, as most businesses follow this kind of a distribution.

![number-of-reviews-by-rating-bar.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/number-of-reviews-by-rating-bar.png?raw=true)

3. There is a negative correlation between the length of the review and the star rating.

![length-of-reviews-by-rating-bar.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/length-of-reviews-by-rating-bar.png?raw=true)

4. None of the other features offered by Yelp have a particularly strong correlation with the star rating.

   ![correlation-heatmap.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/correlation-heatmap.png?raw=true)

   ![correlation-bar.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/plotly-figures/correlation-bar.png?raw=true)

## **Classifying Reviews**

As a business owner, Shirley will likely receive more spoken reviews than written ones, and she may also receive unlabeled reviews if she includes a comments and suggestions box in her gym. If she records these data for downstream applications, such as topic-modeling, it will be important for her to be able to correctly classify reviews according to the star rating, especially given the decline in the number of reviews after 2016 in the Yelp dataset. The problem of classifying reviews into five categories is [multi-class classification](https://en.wikipedia.org/wiki/Multiclass_classification) problem and is a classic technique of [machine learning](https://en.wikipedia.org/wiki/Machine_learning). Let's see how this works.

The first step in any machine-learning process is to set aside some data that **must** remain untouched while training the model. Only on testing on an out-of-sample test set is it possible to estimate how well the model can generalize to data it has never seen before. I found that 20% of the data occurs after May 28th, 2017, so I split the dataset into a training set comprising of data prior to this date and a test set comprising of data after this date.

The next step in text processing is tokenization, or the process of converting sentences into individual words, which make up the features for the classification via [logistic regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html). For this process, I found that sklearn's [count-vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) outperforms [TF-IDF](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html), and that keeping an [n-gram](https://en.wikipedia.org/wiki/N-gram) range of 1 to 3 gives the best performance in terms of speed and accuracy.

```python
# Count Vectorizer
count_vectorizer = CountVectorizer(stop_words='english', ngram_range=(1,3))
```

Let's see what the initial classification looks like. One way to check how many reviews were correctly classified is to generate a [confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix), which is a table that shows how many items from each class were correctly classified. For example, in the confusion matrix below, you can see that 951 5-star reviews were correctly classified, 108 5-star reviews were mis-classified as 4-star reviews, and the rest were mis-classified as 3 or below.

![confusion-2.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/confusion-2.png?raw=true)

In fact, the performance of this classifier is great for 1- and 5-star reviews, but suffers in classifying anything in between. This is very easy to see in the graph below, which shows the percentage of reviews that are classified in each class.

![class-predictions-2.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/class-predictions-2.png?raw=true)

The performance of this classifier can be measured by looking at the [ROC curves](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) for the individual classes, which shows the ability of the classifier to classify reviews correctly when the thresholds are adjusted. For each curve, the area under the curve (AUC) is the probability that given a random set of two reviews that the classifier will correctly classify the two reviews. As expected, the AUCs correlate with the percentage of reviews that were correctly classified.

![roc-1.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/roc-1.png?raw=true)

How do we fix such a class imbalance? On way to adjust the class weights to give minority classes more importance. To do this, I found that the following weights give the model good performance.

```python
# New Thresholds
count_dict = Counter(target_train_ser)
threshold_array = np.array([1/count_dict[1], 2.5/count_dict[2], 3/count_dict[3], 1/count_dict[4], 1/count_dict[5]])
features_train_vectorized_proba_array_new = normalize(features_train_vectorized_proba_array * threshold_array, axis=1, norm='l1')
```

Let's see how this changes our classification.

![confusion-1.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/confusion-1.png?raw=true)

![class-predictions-1.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/class-predictions-1.png?raw=true)

![roc-3.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/classification/roc-3.png?raw=true)

Now the classifier is performing much better on 2-4 star reviews, even though it is performing slightly worse on 1- and 5-star reviews. In my opinion, this is acceptable, because it gives a higher ability for the model to distinguish between reviews. The out-of-sample test accuracy score increased from 0.635 to 0.867 when averaging across all classes.

## **Topic Modeling**

I extracted the important topics for 1- and 5-star reviews.

1-Star Reviews:

![word-cloud-1_star-0.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/topic-modeling/word-cloud-1_star-0.png?raw=true)

![word-cloud-1_star-3.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/topic-modeling/word-cloud-1_star-3.png?raw=true)

5-Star Reviews

![word-cloud-5_star-0.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/topic-modeling/word-cloud-5_star-0.png?raw=true)

![word-cloud-5_star-1.png](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/blob/master/yelp/figures/topic-modeling/word-cloud-5_star-1.png?raw=true)

## **Conclusions**

If you're seeking to open a new climbing gym, here are some do's and don'ts.

Do's: 1. Make the experience of day-pass users enjoyable. 2. Try to increase the membership rate.

Don'ts: 1. Hire rude staff. 2. Throw birthday parties.