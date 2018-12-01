---
author: tarkomatas
comments: true
date: 2018-08-25 14:58:11+00:00
layout: post
title: 'Redmine tips #1: Using ''Watch'' button to gather important issues'
category:
- redmine
---

**Introduction:**

Redmine is a great tool to organize tasks and do the project management stuff. As soon as we have learnt the basic usage of it, it seems that it has some limitations especially if we try to compare it with the competitors (e.g. Asana or MS Project). In this situation I highly recommend to check the plugin list from the official website in the first place. However unfortunately there are many plugins which hasn't been updated for years. Also sometimes it could solve our problem if we understand the nature of the software very well and try to use the available functions in a let's call extended way. In this post I prepared for an example to show what I mean.

If multiple users become to use Redmine to manage the daily tasks, many issues will be generated sooner which results that hard to find some specific tasks. With the extended usage of the Watch button we can create a custom list (query) which gathers the selected issues and that we can easily find and rerun any time we need. Let's see the details.

**Brief summary:**

First of all I will mark some tasks as watched after it I will create a custom query which collects the previously marked issues and finally I will stick it to my personal page.

**Prerequisites:**
  * Redmine 3.4.0 or newer

**1. Usage of issue watch function**

Issue watch was originally created to the following purpose: "Watchers are like followers, like how you can get sent notifications when someone answers your topic on some forum softwares. Normally you only get notified when an issue that you either created or got assigned to is updated, but if you are neither of those, you can also "watch" it, so you will get notified regardless if you are directly related to that issue or not."  Not only is it recommended to use it to get notified but also it is useful to mark some important or top priority issues as we will see before long.

We can easily use the watch function if we open any issue and click to the star symbol in the bottom of the page:

![]({{ site.baseurl }}/assets/images/2018/08/Screenshot-from-2018-08-25-13-17-35.png)

Please repeat the process also with some additional issues.

**2. Custom query to gather issues**

In the Issues menu everybody can create custom queries. Query is a very important function of Redmine because it is a proper way to organize or filter issues with a wide range of flexibility.

First of all add watcher as a new filter and click to the Apply button. After it only the watched issues can be seen. If the current settings seems good for you click to the Save button:

![]({{ site.baseurl }}/assets/images/2018/08/Screenshot-from-2018-08-25-16-14-15-1.png)

Add a name to your custom query and click the Save button again in the bottom of the page. (Not required but can be useful to customize the Selected columns list  a little bit (like I did in the picture below) or add some additional extra filters (e.g. if you want to watch issues only in a specific project).)

![]({{ site.baseurl }}/assets/images/2018/08/Screenshot-from-2018-08-25-16-27-00.png)

If every single step has been taken over, the query appears in the right side of the screen in the Custom queries section:

![]({{ site.baseurl }}/assets/images/2018/08/Screenshot-from-2018-08-25-16-34-26.png)

Now if you want to find your watched issues you just have to run this query every time you need.

**3. Add a query to My Page site**

The My Page site has a useful feature since Redmine version 3.4.0 when it is possible to add custom query to the site. This way we can stick our currently watched issues to our My Page site so it is not necessary to open the issues subpage and rerun the query anymore.

Here are the steps to set up:

![]({{ site.baseurl }}/assets/images/2018/08/Screenshot-from-2018-08-25-16-43-16.png)

**Summary:**

As we have seen with a little creativity we can create our let's call favorite issues list in Redmine that we can find easily.
