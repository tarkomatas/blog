---
author: tarkomatas
comments: true
date: 2017-05-07 12:11:39+00:00
layout: post
title: Hungarian version of FiveThirtyEight age estimator
category:
- rstats
---

I tried to reproduce the [FiveThirtyEight age estimator](https://fivethirtyeight.com/features/how-to-tell-someones-age-when-all-you-know-is-her-name/) with Hungarian data. The methodology is quite the same. Unfortunately there is no as accurate database as the US version use. In Hungary only the common top 100 names has been collected and published [here](http://magyarnevek.hu/nevek/utonevstatisztika). The biggest problem is that before 2000 only date ranges has been published so I assumed uniform distribution between that years. However I found an [up-to-date life table](http://apps.who.int/gho/data/?theme=main&vid=60720) thanks to WHO.

I tried to create something similar like [this](http://rhiever.github.io/name-age-calculator/index.html?Gender=M&Name=Thomas) in R. I used [Shiny](https://shiny.rstudio.com/) and [Plotly](https://plot.ly/) for the visualization. Here is the result of it:

<iframe src="https://myhappydata.com/apps/keresztnevek_ev_kalk/" style="border: none; width: 100%; height: 580px;"></iframe>

The dark-grey line shows the number of birth of the selected name in each years, while the blue color area is the number of birth which was adjusted to the life table of Hungary. The vertical red line shows the median living year of birth of the selected name.
