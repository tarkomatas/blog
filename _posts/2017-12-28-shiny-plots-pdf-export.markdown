---
author: tarkomatas
comments: true
date: 2017-12-28 22:47:27+00:00
layout: post
title: Shiny plots PDF export
category:
- rstats
---

Sometimes it is useful to build a pdf export option into a Shiny app. I built a basic app at first (every important notes was included as comment) to show how to do it easily:

<iframe src="https://myhappydata.shinyapps.io/pdf_export/" style="border: none; width: 100%; height: 780px;"></iframe>

you can also export the ggplot2 type plots like this:

<iframe src="https://myhappydata.shinyapps.io/pdf_export_ggplot/" style="border: none; width: 100%; height: 780px;"></iframe>

Feel free to download the source code from [here](https://github.com/tarkomatas/shiny_pdf_export).

**final notes**: If you would like to use Cairo on Ubuntu you need to run this commands at first:

{% highlight bash %}
sudo apt-get install libcairo2-dev
sudo apt-get install libxt-dev`
{% endhighlight %}
