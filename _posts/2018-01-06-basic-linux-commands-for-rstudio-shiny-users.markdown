---
author: tarkomatas
comments: true
date: 2018-01-06 18:27:58+00:00
layout: post
title: Basic Ubuntu commands for RStudio Shiny users
categories:
- linux
- rstats
---

[RStudio Shiny](https://shiny.rstudio.com/) is a great tool to create interactive reports and dashboards for R users. There are many ways to publish the results, e.g. [shinyapps.io](https://www.shinyapps.io/) is a free online platform but sooner or later it is recommended to install it to a Linux server especially at an enterprise level usage. By the way I use a [DigitalOcean VPS](https://m.do.co/c/29bc34eb98ad) (this is a referral link) mainly for practice and to find a place for my free time projects. At the very first period it is best to improve your Linux skills on a virtual server ([Oracle VM Virtual Box](https://www.virtualbox.org/)) because it is totally free and you cannot ruin anything.

Shiny needs a Linux environment so it is recommended to learn at least the basic commands. I use [Ubuntu 16.04 server](https://www.ubuntu.com/download/server) so commands may vary on different Linux distributions. There are great tutorials how to install and configure Linux servers here are some recommended materials:

  * [How to get your very own RStudio Server and Shiny Server with DigitalOcean by deanattali.com](https://deanattali.com/2015/05/09/setup-rstudio-shiny-server-digital-ocean/)
  * [How To Install Linux, Nginx, MySQL, PHP (LEMP stack) in Ubuntu 16.04 by digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04)
  * [Initial Server Setup with Ubuntu 16.04 by digitalocean.com](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)

In the followings I collected the basic Linux commands which are good to know for Shiny Server administrators or owners:

**install R packages from terminal to all users.**
It is quite rare that Shiny apps does not need any external packages so It is a useful command.

{% highlight bash %}
# install package 'ggplot2' from https://cran.rstudio.com/
sudo su - -c "R -e \"install.packages('ggplot2', repos='https://cran.rstudio.com/')\""

# install package dplyr from Github
# you have to install the package devtools at first before using this command.
sudo su - -c "R -e \"devtools::install_github('hadley/dplyr')\""
{% endhighlight %}

**Install, uninstall Linux packages**
Sometimes you have to install other Linux packages to use R packages (e.g. package Cairo needs libcairo2-dev and libxt-dev)

{% highlight bash %}
# install mysql-server
sudo apt-get install mysql-server

# uninstall mysql-server
sudo apt-get purge mysql-server
{% endhighlight %}

**Upgrade currently installed packages**
Soon or later previously installed packages need to be updated (e.g. R) if there is a new available version.

{% highlight bash %}
# updating the package list
sudo apt-get update

# upgrade installed packages
sudo apt-get upgrade
{% endhighlight %}

**Modify Shiny config file**
When you move files from your computer to the Server it is recommended to Use an FTP tool (e.g. [FileZilla](https://filezilla-project.org/)) but if some command lines needs to be modified it is best to use some built in Linux text editor. I use VIM [here](http://vim.wikia.com/wiki/Tutorial) is the basic tutorial of it.
[Here](http://docs.rstudio.com/shiny-server/) is more information about the config file.

{% highlight bash %}
# navigate to a specific folder.
cd /etc/shiny-server/

# optional: create a backup if it is needed
cp shiny-server.conf shiny-server-backup.conf

# open config file and modify it and save (hit the esc key and type: ':x' to quit).
sudo vim shiny-server.conf
{% endhighlight %}

**Check Shiny Server logs**
It is a little bit trickier to fix a bug because you have to check the log files to find what is the problem with the application.

{% highlight bash %}
# navigate to the folder of Shiny Server logs
cd /var/log/shiny-server

# print the list of the files of the current folder
ls

# open the file named 'testapp-shiny-20170729-065006-33136.log' (Copy a file name: all you have to do is left click and drag to make a selection and than simply right-click to paste)
# type 'q' to quit
less testapp-shiny-20170729-065006-33136.log
{% endhighlight %}

**Change permissions**
According to r Linux Server settings you need to change the ownership of files or folders e.g. to update the R scripts if somebody else uploaded it earlier.

{% highlight bash %}
# change the owner of the folder '/srv/shiny-server/test_app'. The new owner is user1.
chown -R user1 /srv/shiny-server/test_app
{% endhighlight %}

Finally some useful course for a deeper knowledge:
  * [Linux Command Line Basics by Udacity](https://eu.udacity.com/course/linux-command-line-basics--ud595)
  * [Configuring Linux Web Servers by Udacity](https://eu.udacity.com/course/configuring-linux-web-servers--ud299)
