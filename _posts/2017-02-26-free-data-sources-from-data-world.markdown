---
author: tarkomatas
comments: true
date: 2017-02-26 13:54:13+00:00
layout: post
title: Free data sources from data.world
category:
- data.world
---

Sometimes its hard to find good data source when you make some side projects especially when you want to use survey data. I found an interesting site called [data.world](https://data.world/). There are more than thousand database freely available after registration.

It has a cool feature: you can export the data directly to R, so you do not need to download it to your local drive.

![]({{ site.baseurl }}/assets/images/2017/02/ezgif-1-0bfe87798b.gif)

However sometimes it is tricky to use this function because the file format is not always _.csv_ as it assumes, but of course you can use the link of the data source in this case.

Here is a minimal example how you can use it:

{% highlight r %}
library(openxlsx)
library(googleVis)

# set working directory where you want to download the database
# setwd("C:/Users/yourName/Desktop")

download.file("https://query.data.world/s/9k1dnvrr5ykop5r89vnhwb7na", "database.xlsx", mode="wb")

# load the data with the openxlsx package
db = read.xlsx("database.xlsx", startRow = 1, colNames = TRUE)

# aggregate the data to a County level
db = aggregate(db[,"POP2010"], by=list(db[,"STNAME"]), FUN=sum, na.rm = TRUE)

# plot it with googleVis package
GeoStates <- gvisGeoChart(db, "Group.1", "x",
                          options=list(region="US",
                                       displayMode="regions",
                                       resolution="provinces",
                                       width=600, height=400))
plot(GeoStates)
{% endhighlight %}

![]({{ site.baseurl }}/assets/images/2017/02/Képkivágás-3.png)
