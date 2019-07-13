---
layout: post
title: Analyzing Yelp Reviews for Climbing Gyms
---

[Github Repository](https://github.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp)

## **Introduction**

[Rock climbing](https://en.wikipedia.org/wiki/Rock_climbing) is one of the fastest growing sports in the world. According to the [Climbing Business Journal](https://www.climbingbusinessjournal.com/gyms-and-trends-2018/), the establishment of new climbing gyms in the United States is currently in exponential growth, with an estimated 100 new gyms opening between 2018 and 2020 to meet the growing demand of new climbers (shoutout to my friend [Shirley Yang](https://www.linkedin.com/in/shirley-yang-01a40a13/), who is opening a [gym](https://www.agilityboulders.com/) of her own). As a [Data Scientist](https://www.linkedin.com/in/harrisonized) and a [rock climber](https://www.instagram.com/harrisonized/?hl=en), I wondered if there was any data-driven business insights I could deliver to her using my toolbox of data science techniques.

![total-gyms-vs-percent-growth.png](https://raw.githubusercontent.com/harrisonized/analyzing-yelp-reviews-for-climbing-gyms-nlp/master/Climbing%20Business%20Journal/Plotly%20Figures/total-gyms-vs-percent-growth.png)

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

Because of these limitations, I found that the only easy way to access the data that I wanted was just to scrape it using [Beautifulsoup4](https://www.crummy.com/software/BeautifulSoup/bs4/doc/). I sought to stay as close as possible to the format that Yelp already made available through their challenge dataset, so that it would be as if I was working with data that Yelp handed to me directly. Here are the scrapers I built for [reviews](http://localhost:8888/notebooks/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/scrape-yelp-for-reviews.ipynb) and for [business](http://localhost:8888/notebooks/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/scrape-yelp-for-business-information.ipynb) information, in case you would like to adapt them to your purposes. In order to use them, I made a [list of urls](http://localhost:8888/edit/github/analyzing-yelp-reviews-for-climbing-gyms-nlp/yelp/data/url-list.txt) of all the climbing gyms in California, which I sought to analyze.



## **Classifying Reviews**

Used multi-class classification on user reviews to predict the number of stars given by the reviewer. Adjusted the class weights to give minority classes more importance, improving my out-of-sample test accuracy score from 0.635 to 0.867.

### **Topic Modeling**

I extracted the important topics for 1- and 5-star reviews and reduce dimensionality of the data.

### **Conclusions**

If you're seeking to open a new climbing gym, here are some do's and don'ts.

Do's: 1. Make the experience of day-pass users enjoyable. 2. Try to increase the membership rate.

Don'ts: 1. Hire rude staff. 2. Throw birthday parties.