---
author: tarkomatas
comments: true
date: 2018-11-10 15:38:23+00:00
layout: post
title: Detailed introduction of "myprettyreport" R package
category:
- rstats
---

**Package introduction:**
Package myprettyreport helps to export ggplot2 graphs into a good-looking PDF file in a clear and easy way with a wide range of flexibility. It has a modular structure so the report elements can be combined in many ways.

**Installation:**
This package currently only available on [Github](https://github.com/tarkomatas/myprettyreport) so the proper way to install it:

{% highlight r %}
#install.packages("devtools")
devtools::install_github("tarkomatas/myprettyreport")
{% endhighlight %}

**Quick overview:**

Functions of the package:
* start_report() is the first mandatory function to generate the report.
* add_cover_page() function generates the cover page of the report.
* add_new_page() function adds a single report page to the report.
* add_multiple_page() function adds multiple report pages to the report.
* end_report() function generates the final output and closes the process.

Quick example:

{% highlight r %}
library(ggplot2)
sample_plot <- ggplot(data = mtcars, mapping = aes(x = wt, y = mpg)) +
  geom_point() +
  stat_smooth(method = 'lm', color = "#f44242", fill = "#fd9068")

library(magick)
sample_logo <- image_read("https://raw.githubusercontent.com/tarkomatas/website/master/img/1.png")

library(myprettyreport)
start_report() %>%

  add_cover_page(
    logo = sample_logo,
    logo_size = 0.3
  ) %>%

  add_new_page(
    plot = sample_plot,
    need_header = TRUE,
    logo = sample_logo,
    logo_size = 0.2
  ) %>%

  end_report()
{% endhighlight %}

Result:

page1             |  page2
:-------------------------:|:-------------------------:
![](https://raw.githubusercontent.com/tarkomatas/myprettyreport/master/man/figures/README_example1.png)  |  ![](https://raw.githubusercontent.com/tarkomatas/myprettyreport/master/man/figures/README_example2.png)


## Basics

A minimal example which generates a PDF file but it contains just a blank page:

{% highlight r %}
start_report() %>%
  end_report()
{% endhighlight %}

[report output]({{ site.baseurl }}/assets/images/2018/09/report_output.pdf)

As we can see each elements can be added to each other with the magrittr::%>% (pipe) function.

Insert a new report page to a PDF file which creates a skeleton ggplot2 object as default.

{% highlight r %}
start_report() %>%
  add_new_page() %>%
  end_report()
{% endhighlight %}

Now let's create our first very own ggplot2 object and export it. The report syntax is almost the same except we have to add our ggplot2 object to a report as a plot  parameter value.

{% highlight r %}
library(ggplot2)

sample_plot <- ggplot(data = mtcars, mapping = aes(x = wt, y = mpg)) +
  geom_point() +
  stat_smooth(method = 'lm', color = "#f44242", fill = "#fd9068")

start_report() %>%
  add_new_page(plot = sample_plot) %>%
  end_report()
{% endhighlight %}

The result will be equivalent so far if we just use the ggplot2 built in export function:

{% highlight r %}
ggsave("report_output.pdf", width = 21, height = 29.7, units = "cm")
{% endhighlight %}

[output_ggsave]({{ site.baseurl }}/assets/images/2018/09/output_ggsave.pdf)
[output_myprettyreport]({{ site.baseurl }}/assets/images/2018/09/output_myprettyreport.pdf)

Although the result is the same it is easier to modify the report layout a little bit thanks to the package myprettyreport. We could create a footer and a header section like this:

{% highlight r %}
start_report() %>%
    add_new_page(
        plot = sample_plot,
        need_header = TRUE,
        need_footer = TRUE,
        footer_text = "Copyright © Tamas Marko | 2018 | myhappydata.com") %>%
    end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/11/report_output.pdf)

Also it could be possible to add a logo to a top left corner. All we need is to load a logo file into R and add it as a logo parameter. Optionally the size of the logo could be specified (valid values are between 0 and 1).  

{% highlight r %}
library(magick)
sample_logo <- image_read("https://raw.githubusercontent.com/tarkomatas/website/master/img/1.png")

start_report() %>%
    add_new_page(
        plot = sample_plot,
        need_header = TRUE,
        need_footer = TRUE,
        footer_text = "Copyright © Tamas Marko | 2018 | myhappydata.com",
        logo = sample_logo,
        logo_size = 0.2) %>%
    end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/11/report_output-1.pdf)

Currently 2 themes have been implemented yet. Now we are changing the theme from the default to a theme called flashy. In this case we have to modify the default values of the header and footer colors also because the default color of the text is white.

{% highlight r %}
start_report() %>%
    add_new_page(
        plot = sample_plot,
        need_header = TRUE,
        need_footer = TRUE,
        footer_text = "Copyright © Tamas Marko | 2018 | myhappydata.com",
        logo = sample_logo,
        logo_size = 1,
        theme = "flashy",
        header_color = "#f44242",
        footer_color = "#3d3c3c") %>%
    end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/09/report_output-3.pdf)

We may notice that if we use this theme, the position of the logo will also change (now it is on the bottom left corner). So the size parameter of the logo will probably need to be reconfigured also.  

The next step could be adding one more page to our report. In this case the preferred way is to use the add_multiple_page() function because it results more simplified syntax:

{% highlight r %}
sample_plot2 <- ggplot(airquality, aes(x=Temp)) +
  geom_histogram(binwidth=1, fill = "#f66f6f")
plot_list <- list(sample_plot, sample_plot2)

start_report() %>%
    add_multiple_page(
        plot = plot_list,
        need_header = TRUE,
        need_footer = TRUE,
        page_number = rep("Copyright © Tamas Marko | 2018 | myhappydata.com", length(plot_list)),
        logo = sample_logo,
        logo_size = 1,
        theme = "flashy",
        header_color = "#f44242",
        footer_color = "#3d3c3c") %>%
    end_report()
{% endhighlight %}

Important to note that the add_multiple_page() function was executed (not the add_new_page()) and it has a page_number parameter instead of footer_text! So in this case we have to multiply the footer text value.

[report_output]({{ site.baseurl }}/assets/images/2018/09/report_output-4.pdf)

Now let's add a cover page to our report. We have to insert the add_cover_page() function after the start_report() function.

{% highlight r %}
start_report() %>%
    add_cover_page() %>%
    add_multiple_page(
        plot = plot_list,
        need_header = TRUE,
        need_footer = TRUE,
        page_number = rep("Copyright © Tamas Marko | 2018 | myhappydata.com", length(plot_list)),
        logo = sample_logo,
        logo_size = 1,
        theme = "flashy",
        header_color = "#f44242",
        footer_color = "#3d3c3c") %>%
    end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/11/report_output-2.pdf)

It is also possible to change the theme of the cover page if we want the same "flashy" theme everywhere:

{% highlight r %}
start_report() %>%
    add_cover_page(
        theme = "flashy"
    ) %>%
    add_multiple_page(
        plot = plot_list,
        need_header = TRUE,
        need_footer = TRUE,
        page_number = rep("Copyright © Tamas Marko | 2018 | myhappydata.com", length(plot_list)),
        logo = sample_logo,
        logo_size = 1,
        theme = "flashy",
        header_color = "#f44242",
        footer_color = "#3d3c3c") %>%
    end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/09/report_output-6.pdf)

Now let's add multiple plots to a single page:

{% highlight r %}
start_report() %>%
  add_new_page(
     plot = plot_list,
     plot_hpos = c(1, 2),
     plot_vpos = c(1, 2),
     plot_area_layout = grid::grid.layout(2, 2,
                                          width = c(1, 1),
                                          heigh = c(1, 1))
  ) %>%
  end_report()
{% endhighlight %}

In this case we have to specify the layout of the plot area with the grid::grid.layout() function. Also we have to determine the position of every single plot (plot_hpos and plot_vpos). For example if we create a 2x2 plot area plot_hpos = c(1, 2) means that the first plot in the plot_list will be in the left side of the page, and second one will be on the other side.

Optionally of course we could also use other libraries to reach the same goal. My personal recommendation is the [patchwork](https://github.com/thomasp85/patchwork) package. As I've mentioned already the result will be the same:

{% highlight r %}
library(patchwork)

multiple_plot_one_page <- sample_plot + patchwork::plot_spacer() +
                          patchwork::plot_spacer() + sample_plot2 +
                          patchwork::plot_layout(ncol = 2, nrow = 2)

start_report() %>%
  add_new_page(
    plot = multiple_plot_one_page
  ) %>%
  end_report()
{% endhighlight %}

[report_output]({{ site.baseurl }}/assets/images/2018/11/report_output-3.pdf)
[report_output_patchwork]({{ site.baseurl }}/assets/images/2018/11/report_output_pw.pdf)

Finally create a report which has multiple pages and also multiple plots in every page:

{% highlight r %}
plot_list <- list(
  list(sample_plot, sample_plot2),
  list(sample_plot2, sample_plot)
)

start_report() %>%
 add_multiple_page(
    plot = plot_list,
    plot_hpos = c(1, 2),
    plot_vpos = c(1, 2),
    plot_area_layout = grid::grid.layout(2, 2,
                                         width = c(1, 1),
                                         heigh = c(1, 1))
 ) %>%
 end_report()
{% endhighlight %}

So if we compare this syntax to the previous one we could see that the only difference is that we need to create sublists inside our list and every plot which has been defined inside a sublist will appear in the same report page.  

## Advanced features

**Export multiple ggplot2 objects into multiple files**

{% highlight r %}
create_multiple_report <- function(db, xvar, yvar) {
  sample_plot <- ggplot(data = db, mapping = aes_string(x = xvar, y = yvar)) +
    geom_point() +
    stat_smooth(method = 'lm', color = "#f44242", fill = "#fd9068")

  start_report(
    filename = paste0(xvar,".pdf")
  ) %>%
    add_new_page(plot = sample_plot) %>%
    end_report()
}

lapply(colnames(mtcars)[5:7], create_multiple_report, db = mtcars, yvar = "mpg")
{% endhighlight %}

[drat]({{ site.baseurl }}/assets/images/2018/09/drat.pdf)
[qsec]({{ site.baseurl }}/assets/images/2018/09/qsec.pdf)
[wt]({{ site.baseurl }}/assets/images/2018/09/wt.pdf)

**Customize the report with extra grid parameters**

The package can allows us to pass extra grid parameters to our report to customize the final output. It can be useful if we need e.g. an additional text field. First of all we have to know the default layout structure of the report. If you haven't heard about the basics of grid layouts it is recommended to go deeper into that topic at first. I share the structure of the layouts which was produced by the grid::grid.show.layout() function:

[cover_page_theme_basic]({{ site.baseurl }}/assets/images/2018/11/cover_page_theme_basic.pdf)
[report_page_theme_basic]({{ site.baseurl }}/assets/images/2018/11/report_page_theme_basic.pdf)

[cover_page_theme_flashy]({{ site.baseurl }}/assets/images/2018/11/cover_page_theme_flashy.pdf)
[report_page_theme_flashy]({{ site.baseurl }}/assets/images/2018/11/report_page_theme_flashy.pdf)

So e.g. if we want to insert an extra text field in the report page header when the theme is the basic, the syntax is the following:

{% highlight r %}
rp_e_layout_params = function() {
    grid::grid.text("example text",
                    y = 0.7,
                    vp = grid::viewport(layout.pos.row = 1,
                                        layout.pos.col = 1))
 }
 start_report() %>%
   add_new_page(
     plot = sample_plot,
     need_header = TRUE,  
     extra_layout_params = rp_e_layout_params
   ) %>%
   end_report()
{% endhighlight %}
