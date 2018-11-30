---
author: tarkomatas
comments: true
date: 2018-06-09 11:08:20+00:00
layout: post
title: Connect MySQL database with DBeaver through SSH
category:
- linux
---

**Aim:**

The aim of this post is to create a straightforward tutorial how to connect to a server MySQL database with [DBeaver](https://dbeaver.io/) through SSH.

**Prerequisites:**
  * DBeaver (version 5.0.1) has already installed natively to your PC (I used Manjaro)
  * Ubuntu 16.04 VPS (e.g. [DigitalOcean](https://m.do.co/c/29bc34eb98ad)) which has a [LAMP](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04) or [LEMP](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04) installation
  * Root privilege on your VPS

**Step 1: allow remote access to MySQL**

Open a terminal application and login to your VPS at first via SSH:

{% highlight bash %}
ssh -i PATH_OF_THE_PRIVATE_KEY@SERVER_IP
{% endhighlight %}

Navigate to '/etc/mysql/mysql.conf.d/' folder and open the MySQL config file 'mysqld.cnf' with a text editor (like nano):

{% highlight bash %}
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
{% endhighlight %}

Comment out the line 'bind-address = 127.0.0.1'. Make sure This like has to begin with '#' like this:

{% highlight bash %}
# bind-address = 127.0.0.1
{% endhighlight %}

Save the file and exit (CTRL+X and press 'Y' for yes).
Restart the MySQL server to make the changes come into effect:

{% highlight bash %}
sudo systemctl restart mysql
{% endhighlight %}

**Step 2: Allow connection to the mysql port through UFW**

For security reasons the best way is just to gain access only to your PC's IP you use:

{% highlight bash %}
sudo ufw allow from IP_OF_YOUR_PC to any port 3306
{% endhighlight %}

**Step 3: Connect to MySQL server in Dbeaver**

There are two important windows has to fill it in the right way.
In the 'General' window you have to take care of only the 'User name' and 'Password' fields.
![]({{ site.baseurl }}/assets/images/2018/06/Screenshot-from-2018-06-09-10-49-42.png)

* User name: (existing) user name you want to use at MySQL connection
* Password: Password of the user you selected before

When you have already filled the required fields click to Next button. After it the second important window appears:

![]({{ site.baseurl }}/assets/images/2018/06/Screenshot-from-2018-06-09-10-55-57.png)

First of all click to the checkbox icon 'Use SSH Tunnel' to gain access to the settings. Fields you have to fill in the right way:

* HOST/IP: IP of the VPS
* User Name: (existing) user name you want to use at MySQL connection (same as on the other page)
* Authentication Method: 'Public Key'
* Private Key: Path of the private key on your PC

If everything has configured correctly when you click to 'Test Connection' this message box will appear:

![]({{ site.baseurl }}/assets/images/2018/06/Screenshot-from-2018-06-09-11-04-08.png)

When the test connection works click to the 'Finsih' button. After the whole procedure you can connect to any MySQL database which your user has _permission to access_.
