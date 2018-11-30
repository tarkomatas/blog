---
author: tarkomatas
comments: true
date: 2017-02-26 20:59:10+00:00
layout: post
title: Strange bug when you use Plotly on your own server
category:
- rstats
---

I used my Shiny apps only locally and only with [shinyapps.io](http://www.shinyapps.io/) in the past, but now I've got a VPS. When I migrated my previous apps to the VPS I found a strange bug. When there was ggplot based Plotly plot in my code I got an error message: "An error has occurred. Check your logs or contact the app author for clarification.".

I checked the log file which said:

{% highlight r %}
Warning: Error in <anonymous>: cannot open file 'Rplots.pdf'
Stack trace (innermost first):
    88: <anonymous>
    87: grid.Call
    86: convertUnit
    85: convert
    84: unitConvert
    83: %||%
    82: gg2list
    81: ggplotly.ggplot
    80: ggplotly
    79: ggplotly [/srv/shiny-server/survey_from_gf/server.R#136]
    78: func
    77: origRenderFunc
    76: output$plotlyBar
     1: runApp
{% endhighlight %}


Fortunately I found [this](https://github.com/ropensci/plotly/issues/494) blog post which says I just only need to paste pdf(NULL) to my code which solves the problem.

So if you have a same issue you just only need to paste it to the start of the renderPlotly function like this:

{% highlight r %}
shinyServer(
  function(input, output) {
     output$plotlyPlot <- renderPlotly({
        pdf(NULL)

        ggplot(mtcars, aes(factor(cyl))) +
        geom_bar()

        ggplotly(p)
     })
})
{% endhighlight %}

Write me if you know a more elegant way to fix this bug.
