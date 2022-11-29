---
layout: post
title: Image Classification using Tensorflow
date: 2022-11-29
categories: 
tags: 
image: /assets/article_images/2022-11-29-image-classification/trypan-blue-stained-cho-cells.jpg
image2: /assets/article_images/2022-11-29-image-classification/trypan-blue-stained-cho-cells.jpg
---



As part of my application for Metis, I proposed a passion project that involved training a machine learning model to classify cells as dead or alive, then counting the number of alive and dead cells in an image. The reason I wanted to do this project is that while I was working as a research associate at True North Therapeutics, I routinely used an instrument called the [Invitrogen Countess](https://www.thermofisher.com/us/en/home/life-science/cell-analysis/cell-analysis-instruments/automated-cell-counters/models/countess-3.html) to count cells. The machine itself [costs nearly $5000](https://www.fishersci.com/shop/products/countess-3-fl-automated-cell-counter-1/AMQAF2000), and the slides [cost about $2 each](https://www.fishersci.com/shop/products/invitrogen-countess-cell-counting-chamber-slides-5/p-4931409). To me, this seemed outrageous. What if we can just use a regular microscope, take a picture, and train the computer do the work? This would essentially make the process of counting live-dead cells free. We would be able to save a ton of money, and we might even be able to expand these techniques to immunofluorescence work that we were doing.

![](/assets/article_images/2022-11-29-image-classification/images/20190203 Cell Image Classification Project.png)

However, there were some practical reasons I didn't end up doing this project:

1. When I took the bootcamp, there was zero support for using Linux machines. Before the bootcamp began, they strongly encouraged people to buy Apple machines and many people did, but I was one of the stubborn ones. I ran into a great deal of issues with software installation, which I've written about [here](https://harrisonized.github.io/2020/02/02/prepare-laptop-for-ds.html) and [here](https://harrisonized.github.io/2020/11/30/fix-ubuntu-100-cpu-blank-screen.html), and this greatly hampered my ability to keep up with the information fire-hydrant.
2. The laptop I used at the time was a [Lenovo G410](https://www.lenovo.com/us/en/laptops/lenovo/g-series/g410/) I had bought in 2014. This is an entry level laptop with a hilariously bad graphics card ([Intel HD Graphics 4600](https://benchmarks.ul.com/hardware/gpu/Intel+HD+Graphics+4600+review)), and some of the "simple" example notebooks from Metis on Tensorflow and Keras took upward of an hour for my machine to run. It's likely the fastest model I could conceivably train would far exceed the time it would take to count manually.
3. I couldn't end up finding a suitable dataset at that time. Maybe I wasn't looking in the right places, but what I did have was a dataset from the Yeo lab. Yet, since I had zero computer vision experience, I felt that 3 weeks was too short a time to build a training set, learn Tensorflow, AND create a working model, all before we were supposed to present our projects to potential future employers.
4. Lastly and most importantly, this is already a problem solved by professionals who have made it into a commercial product. Even if I were to create such a classifier, there is not a chance it would perform remotely as well as the commercial products, especially not without the help of someone with expertise in computer vision, which my Metis instructors definitely were not.

##### Part 2

In early 2021, while I was in between Invitae and Thermo-Fisher, I ended up revisiting this essential part of my learning. To get started, I remembered that during the bootcamp, [LaurenRGray](https://github.com/LaurenRGray/) did a similar project, so I looked up the dataset for [Ships in Satellite Imagery](https://www.kaggle.com/rhammell/ships-in-satellite-imagery) and found [this example notebook](https://www.kaggle.com/byrachonok/keras-for-search-ships-in-satellite-image/), which was actually the original source for Lauren's work. With a better laptop and more experience, I was able to get my GPU to play nicely with Tensorflow. I managed to reproduce all of byrachonok's work, which I made available on my github [here](https://github.com/harrisonized/ships-in-satellite-imagery).

Going through this exercise gave me an understanding of the basics of image analysis: how to manipulate images as numpy arrays, how to construct the dataset from a folder of images, how to construct the inputs to the CNN, how to structure the CNN, how to save and load the model, and how to use the model to detect images. All that was left to do is to substitute ships and the SF-bay with microscope images of cells. There was one thing that was unsatisfying though: scanning windows is extremely slow and computationally expensive. If I ever revisit this topic, I would love to look at YOLO (you-only-look-once) models.

That's where I left off when I joined Thermo-Fisher, and for a variety of other reasons, this project ended up on the back burner again. I had intended to publish this blog post at that time to document the progress, but it ended up sitting on my computer for over a year.

##### Where do we go from here?

Now that I'm in grad school for immunology, my time for exploring such projects is limited. I may not have a chance to revisit this topic, but I felt it's still worth publishing this blog post to remind myself that I still have many unfinished coding projects like this one on my back burner, and instead of vegetating during my breaks from lab or studying, I should strive to revisit such projects when I have spare time.