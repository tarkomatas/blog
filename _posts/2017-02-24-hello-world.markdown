---
author: tarkomatas
comments: true
date: 2017-02-24 15:05:02+00:00
layout: post
title: Where to find and how to use NUTS2 level maps in R
category:
- rstats
tags:
- cartography
- maps
---

There are many opportunities to find maps which are good for R, also you can easily find country level maps.

### TIP1: GADM database + basic plot function

For example [GADM](http://gadm.org/country) is an awesome site, you can freely download any country map in a format of _SpatialPolygonsDataFrame_. Most of the countries has multiple levels.

Here is a simple example: (I will use map of Hungary through the whole example)

{% highlight r %}
#load the data
download.file("http://biogeo.ucdavis.edu/data/gadm2.8/rds/HUN_adm1.rds",
              "HUN_adm1.rds", mode = "wb")
countries = readRDS("HUN_adm1.rds")

#check the structure of the data
countries@data

#simply plot it with random color
plot(countries, col = colorRampPalette(c("white", "red"))(nrow(countries@data)))
{% endhighlight %}

![]({{ site.baseurl }}/assets/images/2017/02/Képkivágás.png)

### TIP2: googleVis

Also there is a cool interactive web-based solution thanks to Google (and of course the creators of googleVis package) which also supoorts most of the countries:

{% highlight r %}
library(googleVis)
library(sp)

#create random database with county codes
countyName = c("HU-BU",
                "HU-BK",
                "HU-BA",
                "HU-BE",
                "HU-BZ",
                "HU-CS",
                "HU-FE",
                "HU-GS",
                "HU-HB",
                "HU-HE",
                "HU-JN",
                "HU-KE",
                "HU-NO",
                "HU-PE",
                "HU-SO",
                "HU-SZ",
                "HU-TO",
                "HU-VA",
                "HU-VE",
                "HU-ZA")
randomData = runif(length(countyName),0,100)
exampleData <- data.frame(countyName, randomData)

GeoMaps <- gvisGeoChart(exampleData, "countyName", "randomData",
                          options=list(region="HU",
                                       displayMode="regions",
                                       resolution="provinces",
                                       width=600, height=400))
plot(GeoMaps)
{% endhighlight %}


![]({{ site.baseurl }}/assets/images/2017/02/Képkivágás-1.png)

### TIP3: Eurostat geodata + basic plot function

But it was really hard to find a [NUTS2](http://ec.europa.eu/eurostat/web/nuts/overview) level country maps for me, but finally I came across the [geodata](http://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/nuts#nuts13) of Eurostat. I recommend to use the 1:1 Million scale if you want to plot countries.

{% highlight r %}
library(rgdal)

#download the file
temp <- tempfile(fileext = ".zip")
download.file("http://ec.europa.eu/eurostat/cache/GISCO/geodatafiles/NUTS_2013_01M_SH.zip", temp)
unzip(temp)

#load the data and filter it to Hungary and NUTS2 level
EU_NUTS = readOGR(dsn = "./NUTS_2013_01M_SH/data", layer = "NUTS_RG_01M_2013")
map_nuts2 <- subset(EU_NUTS, STAT_LEVL_ == 2) # set NUTS level
country <- substring(as.character(map_nuts2$NUTS_ID), 1, 2)
map <- c("HU") # limit it to Hungary
map_nuts2a <- map_nuts2[country %in% map,]

#plot it
plot(map_nuts2a, col = colorRampPalette(c("white", "red"))(nrow(map_nuts2a@data)))
{% endhighlight %}

### Bonus!

When I used the geodata for my project I also used the [cartography](https://github.com/Groupe-ElementR/cartography) package which is an easy-to-use map creator.
Here is a small example how you can use it:

{% highlight r %}
library(cartography)

plot(map_nuts2a)

cols <-	 carto.pal(pal1 = "green.pal", n1 = nrow(map_nuts2a@data)+1)
nuts2_id = map_nuts2a@data[,"NUTS_ID"]
value = runif(nrow(map_nuts2a@data),0,50)
hun_nuts2_df = data.frame(nuts2_id, value)

choroLayer(spdf = map_nuts2a, # SpatialPolygonsDataFrame of the regions
           df = hun_nuts2_df, # target data frame
           var = "value", # target value
           breaks = c(0,5,10,15,20,25,30,35,100), # list of breaks
           col = cols, # colors
           border = "white", # color of the polygons borders
           lwd = 2, # width of the borders
           legend.pos = "right", # position of the legend
           legend.title.txt = "",
           legend.values.rnd = 2, # number of decimal in the legend values
           add = TRUE) # add the layer to the current plot

labelLayer(spdf = map_nuts2a, # SpatialPolygonsDataFrame used to plot he labels
           df = hun_nuts2_df, # data frame containing the lables
           txt = "nuts2_id", # label field in df
           col = "black", # color of the labels
           cex = 0.9, # size of the labels
           font = 2)  # label font
{% endhighlight %}

![]({{ site.baseurl }}/assets/images/2017/02/Képkivágás-2.png)

Please write it down if you have an other source of NUTS2 level geodata which is also compatible with R especially if there is a (interactive) JavaScript based solution.
