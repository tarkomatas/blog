---
author: tarkomatas
comments: true
date: 2017-06-10 18:18:01+00:00
layout: post
title: Forecast Hungarian higher education data in R
category:
- rstats
---

I had to forecast the further number of enrolled students in the Hungarian higher education sphere. I did the whole job in R of course.

In Hungary the most important higher education statistics are published in each year so you can easily access to the data [here](https://www.felvi.hu/felveteli/ponthatarok_rangsorok/elmult_evek/!ElmultEvek/elmult_evek.php?stat=1). I chose the most typical group of students who enrolled directly after secondary education.
I wanted to use just a simple method, so I chose Holt's Exponential Smoothing ([here](http://a-little-book-of-r-for-time-series.readthedocs.io/en/latest/src/timeseries.html) is a basic description of how forecast in R btw), I recommend to use package forecast in similar cases.

The output is a GIF animation which was made with the package gganimate:

![]({{ site.baseurl }}/assets/images/2017/06/output.gif)

The forecast is quite OK according to correlogram and Ljung-Box test but because of the small number of observation the assumption of normal distribution of forecast errors does not seems to be met completely.

According to the results it is likely to be a decrease which is a sad fact because based on the most tertiary education statistics Hungary has already performed [worst than the OECD average](https://data.oecd.org/eduatt/population-with-tertiary-education.htm).
