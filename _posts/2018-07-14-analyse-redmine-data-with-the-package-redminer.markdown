---
author: tarkomatas
comments: true
date: 2018-07-14 09:14:58+00:00
layout: post
title: Analyse Redmine data with the package redmineR
category:
- rstats
---

**Introduction:**

[Redmine](https://www.redmine.org/) is a great tool to organize tasks. It has many built in functions like spent time report but also there are many limitations. Thanks to the package [redmineR](https://github.com/openanalytics/redmineR) we can access to Redmine data and use the power of R to analyze and visualize it.

In this little introductory tutorial I will focus only on spent time data but I'd like to highlight that there are many more analytical potential on the redmineR package. Check the documentation for more details (help(package = "redmineR")).

**Starting point:**

First of all we have to connect to our Redmine database with the API key. In this example I use a dummy Redmine site with a random data which has been prepared for this tutorial. My code is fully reproducible so the URL and the API key is valid! (Just please do not delete it :) )

{% highlight r %}
library(devtools)
# install_github("openanalytics/redmineR")
library(redmineR)

Sys.setenv("REDMINE_URL" = "http://testredminer.m.redmine.org/")
Sys.setenv("REDMINE_TOKEN" = "0bd65b60431ef3917af1e263cbd44246e1458a32")
{% endhighlight %}

**Basic data preparation:**

Once we connect I just download all the spent time data which is available on the server and store it as an R data frame object.

{% highlight r %}
activity_list <- redmine_time_entries()

head(activity_list)

#  id                   project         user       activity hours comments   spent_on           created_on           updated_on
#1 39 2, Mobile app development Agnes Yellow 3, development     3          2018-07-13 2018-07-13T21:23:51Z 2018-07-13T21:23:51Z
#2 38 2, Mobile app development Agnes Yellow 3, development     4          2018-07-13 2018-07-13T21:22:30Z 2018-07-13T21:22:30Z
#3 35 2, Mobile app development Agnes Yellow 3, development     3          2018-07-13 2018-07-13T21:20:41Z 2018-07-13T21:20:41Z
#4 20 2, Mobile app development  James White 3, development     8          2018-07-13 2018-07-13T19:52:55Z 2018-07-13T19:59:59Z
#5 12 2, Mobile app development   Jenny Blue 3, development     5          2018-07-13 2018-07-13T19:51:56Z 2018-07-13T19:51:56Z
#6 11 2, Mobile app development    Tom Black 3, development     9          2018-07-13 2018-07-13T19:48:37Z 2018-07-13T19:48:37Z
{% endhighlight %}

As we can see the are lists on the data frame which wont be so optimal for us later when need to transform the data a little bit. So I unlist the column 'user' to get access to usernames:

{% highlight r %}
activity_list$user <- unlist(lapply(activity_list$user, `[[`, 2))
{% endhighlight %}

I filter the data to a specific week. When data manipulation is required I use the functions of package [dplyr](https://cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html). I pick the days between '2018-07-09' and '2018-07-13'.

{% highlight r %}
activity_list <- activity_list %>%  
                  filter(spent_on >= "2018-07-09" & spent_on <= "2018-07-13")
{% endhighlight %}

**Analysis of users daily performance:**

At first I group the data by users and days and after it I just summarize how many working hours the users did per day.

{% highlight r %}
daily_performance_db <- activity_list  %>%
                          group_by(user, spent_on) %>%
                          summarise(actual_worked_hours = sum(hours))
{% endhighlight %}

I assume that the expected working hours per day is 8. If I divide the actual worked hours by the expected working hours and I exact 1 out of it finally the result can be a type of performance indicator for the managers.

{% highlight r %}
daily_performance_db <- daily_performance_db %>%
                          mutate(expected_working_hours = 8,
                                 performance_pc = (actual_worked_hours / expected_working_hours) - 1)  

head(daily_performance_db)

# A tibble: 6 x 5
# Groups:   user [2]
#  user         spent_on   actual_worked_hours expected_working_hours performance_pc
#  <chr>        <chr>                    <dbl>                  <dbl>          <dbl>
#1 Agnes Yellow 2018-07-09                10                        8          0.25
#2 Agnes Yellow 2018-07-10                 6                        8         -0.25
#3 Agnes Yellow 2018-07-11                 9.5                      8          0.188
#4 Agnes Yellow 2018-07-12                10                        8          0.25
#5 Agnes Yellow 2018-07-13                10                        8          0.25
#6 James White  2018-07-09                10                        8          0.25
{% endhighlight %}

With the package [ggplot2](https://ggplot2.tidyverse.org/) we can visualize the result:

{% highlight R %}
library(ggplot2)
library(scales)

ggplot(daily_performance_db, aes(x = spent_on, y = performance_pc, fill = user)) +
  geom_bar(stat = "identity", position = position_dodge()) +
  scale_y_continuous(labels = scales::percent)
{% endhighlight %}

![]({{ site.baseurl }}/assets/images/2018/07/Screenshot-from-2018-07-14-11-07-10.png)

According to our chart the workload is not the same between days.  For example Jenny did an overtime on Monday but in the next few days she only worked 4 hours.

**Analysis of users weekly performance:**

With the analogy of the previous example we can also calculate the weekly performance. We just don't need the 'spent on' column as a grouping variable anymore.

{% highlight R %}
weekly_performance_db <- activity_list %>%
  group_by(user) %>%
  summarise(actual_worked_hours = sum(hours)) %>%
  mutate(expected_working_hours = 40,
         performance_pc = (actual_worked_hours / expected_working_hours) - 1)
{% endhighlight %}

Now just determine a tolerance level which indicates us the "outliers". In this example I use +10% as a maximum and -10% as a minimum level. So if somebody works more or less than 10 percent as we expect we mark him or her.

{% highlight R %}
t_level = c(0.1, -0.1)

weekly_performance_db <- weekly_performance_db %>%
                            mutate(performance = ifelse(between(performance_pc, min(t_level), max(t_level)), "expected", NA),
                                   performance = ifelse(performance_pc < min(t_level), "underperformance", performance),
                                   performance = ifelse(performance_pc > max(t_level), "outperformance", performance))
weekly_performance_db

# A tibble: 4 x 5
#  user         actual_worked_hours expected_working_hours performance_pc performance     
#  <chr>                      <dbl>                  <dbl>          <dbl> <chr>           
#1 Agnes Yellow                45.5                     40          0.137 outperformance  
#2 James White                 40.8                     40          0.02  expected        
#3 Jenny Blue                  28                       40         -0.3   underperformance
#4 Tom Black                   34.5                     40         -0.137 underperformance
{% endhighlight %}

As we see only James performed as we expect. Agnes outperformed a little bit so there is a danger of born-out or overload. In the other hand Jenny and Tom worked less hours than the expectation maybe the task allocation between our collages is not proper.

**Summary:**

This tutorial helped us to prove that there is an analytical potential on the package redmineR so I highly recommend to dig deeper into it.
