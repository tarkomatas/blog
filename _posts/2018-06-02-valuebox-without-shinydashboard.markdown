---
author: tarkomatas
comments: true
date: 2018-06-02 09:03:12+00:00
layout: post
title: valueBox without ShinyDashboard
category:
- rstats
---

**Aim of the Fuction:**

Reproduce the [valueBox](https://www.rdocumentation.org/packages/shinydashboard/versions/0.7.0/topics/valueBox) function From [ShinyDashboard](https://rstudio.github.io/shinydashboard/) package with almost the same functionality. This code allows us to use valueboxes without loading ShinyDashboard package and without using the required dashboard UI structure.

**Usage:**

`valueBox(value, subtitle, icon, color)`

**Arguments:**

`value` = The numeric value to display in the box.
`subtitle` = The label text value to display in the box.
`icon` = Font Awesome icon which appears on the right side of the valuebox. Be aware of the right spelling, check [Font Awesome](https://fontawesome.com/icons?d=gallery) website for details. Valid parameter can be e.g. `tachometer`
`color` = Background color of the valueBox. Use can pick character color names like `red`, or `green`, It also possible to use Hex colors like `#ff9999`

**Example:**

<iframe src="https://myhappydata.shinyapps.io/value_box/" style="border: none; width: 100%; height: 780px;"></iframe>

The source code of the Shiny app can be found [here](https://github.com/tarkomatas/valuebox).
