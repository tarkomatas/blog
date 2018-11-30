---
author: tarkomatas
comments: true
date: 2018-07-05 19:55:22+00:00
layout: post
title: Dealing with missing data when subsetting values
category:
- rstats
---

**Introduction:**

Working with data frames can be tricky at first. For example it seems to be very logical especially for a not really experienced R users to manage the rows subsettings by using square brackets such like this:  example_df[column_1 == "A", ] . Actually It works well but only that cases when there is no missing value in the data frame. In this little introductory post I am going to show the proper way how to deal with missing value in similar situations.

**Starting point:**

So first of all let me give you a small example which shows the problem that I've already mentioned. When I try to subset observations in the mtcars dataset the result is fine as we expected:

{% highlight r %}
mtcars[mtcars$mpg == 21, ]

#               mpg cyl disp  hp drat    wt  qsec vs am gear carb
# Mazda RX4      21   6  160 110  3.9 2.620 16.46  0  1    4    4
# Mazda RX4 Wag  21   6  160 110  3.9 2.875 17.02  0  1    4    4
{% endhighlight %}

Now lets try the same thing in that case when there is missing data on the data frame object:

{% highlight r %}
airquality[airquality$Ozone > 20 & airquality$Solar.R > 300, ]

#     Ozone Solar.R Wind Temp Month Day
# NA      NA      NA   NA   NA    NA  NA
# NA.1    NA      NA   NA   NA    NA  NA
# 17      34     307 12.0   66     5  17
# 19      30     322 11.5   68     5  19
# NA.2    NA      NA   NA   NA    NA  NA
# 41      39     323 11.5   87     6  10
# NA.3    NA      NA   NA   NA    NA  NA
# NA.4    NA      NA   NA   NA    NA  NA
# 67      40     314 10.9   83     7   6
# NA.5    NA      NA   NA   NA    NA  NA
# NA.6    NA      NA   NA   NA    NA  NA
# NA.7    NA      NA   NA   NA    NA  NA
{% endhighlight %}

It seems as though there is a problem with the output. It is because R doesn't understand how to deal with NA's (unless we tell the instruction) when there is a logical condition. Cells which contains missing values is not smaller or greater than e.g 20 in the "Ozone" column. It is just a missing value that's why R attach this rows to the selected ones. This results usually unexpected outputs.

**Basic solution:**

You can tell to R that there are missing values on the dataset so needs to be care about it. The  is.na()  function would be the quick fix for this situations.

{% highlight r %}
airquality[airquality$Ozone > 20 & airquality$Solar.R > 300 & is.na(airquality$Ozone) == FALSE & is.na(airquality$Solar.R) == FALSE, ]

#    Ozone Solar.R Wind Temp Month Day
# 17    34     307 12.0   66     5  17
# 19    30     322 11.5   68     5  19
# 41    39     323 11.5   87     6  10
# 67    40     314 10.9   83     7   6
{% endhighlight %}

Honestly I do not really recommend this way to handle the problem because it easily becomes chaotic when there are many logical conditions.

**One better solution:**

It is much better to use which() function because in that case you don't need to use is.na() anymore inside square bracket. The code becomes also simpler than the previous one and result remains the same of course:

{% highlight r %}
airquality[which(airquality$Ozone > 20 & airquality$Solar.R > 300), ]

#    Ozone Solar.R Wind Temp Month Day
# 17    34     307 12.0   66     5  17
# 19    30     322 11.5   68     5  19
# 41    39     323 11.5   87     6  10
# 67    40     314 10.9   83     7   6
{% endhighlight %}

**Almost the best solution:**

One of the biggest advantage of the subset() function is that it do not needs to always reference the name of the data frame inside square bracket. This so called feature makes our code even simpler:

{% highlight r %}
subset(airquality, Ozone > 20 & Solar.R > 300)

#    Ozone Solar.R Wind Temp Month Day
# 17    34     307 12.0   66     5  17
# 19    30     322 11.5   68     5  19
# 41    39     323 11.5   87     6  10
# 67    40     314 10.9   83     7   6
{% endhighlight %}

**My personal recommendation:**

There are popular packages which are specialized for data manipulation. Actually it is worth to learn the use of these packages. Usually these are faster than the basic functions and also there are many useful features. Just check both of if and pick one which fits better for you.  

The [dplyr](https://dplyr.tidyverse.org/) solution:

{% highlight r %}
library(dplyr)

airquality %>%
  filter(Ozone > 20 & Solar.R > 300)

#    Ozone Solar.R Wind Temp Month Day
# 17    34     307 12.0   66     5  17
# 19    30     322 11.5   68     5  19
# 41    39     323 11.5   87     6  10
# 67    40     314 10.9   83     7   6
{% endhighlight %}

The [data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html) solution:

{% highlight r %}
library(data.table)

as.data.table(airquality)[Ozone > 20 & Solar.R > 300]

#    Ozone Solar.R Wind Temp Month Day
# 17    34     307 12.0   66     5  17
# 19    30     322 11.5   68     5  19
# 41    39     323 11.5   87     6  10
# 67    40     314 10.9   83     7   6
{% endhighlight %}

**Summary:**

There are many ways how to avoid unexpected output when you need to subset rows in data frame object which has missing values. All of the solutions I've showed handle the problem in some way but as a programmer I would recommend that you have to always care about clean code so in this situation pick the simplest code which results the same output. So use the dplyr or data.table package or if you don't prefer to use packages for some reasons function subsets() would also works well.
