---
author: tarkomatas
comments: true
date: 2017-05-13 16:17:59+00:00
layout: post
title: Bump Charts in R
category:
- rstats
---

Recently I found [this guy](https://public.tableau.com/profile/matt.chambers#!/) who create beautiful charts in Tableau. Especially I like [this](https://public.tableau.com/profile/matt.chambers#!/vizhome/CarColorEvolutionNorthAmerica/ColorRankOverTime) Bump Chart style visualization. I just wondered it can be easy to reproduce it in R so I gave it a try.

I used the [Hungarian first name database](http://magyarnevek.hu/nevek/utonevstatisztika) which I have already showed in the previous post. I uploaded it to [data.world](https://data.world/tarkomatas/hungarian-first-and-middle-name-database-1954-2016) so you can download the whole database. The Bump Chart in other words is just a simple line chart with a minimal correction, but this kind of plot can be useful to visualize rank result. Here is my implementation in R:

<iframe src="https://tarkomatas.shinyapps.io/keresztnevek_rangsor/" style="border: none; width: 100%; height: 780px;"></iframe>

This visualization shows the popularity trend of the top10 male first name in 2016 between 2000 and 2016 according to yearly rank of names. There are names which were not always in the top10 between the selected period that's why there is a 10+ line in the bottom. you can highlight any name by clicking on it or you can also select any of it from the drop-down list.

I would like to also publish the code to help to reproduce my work. I used Shiny so there are two separate files.

you can find the source code [here](https://github.com/tarkomatas/r-bumpcharts).
