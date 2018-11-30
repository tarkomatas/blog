---
author: tarkomatas
comments: true
date: 2018-05-15 19:14:07+00:00
layout: post
title: how to install and setup Virtualbox on Manjaro
categories:
- linux
- manjaro
---

**1. First install Virtualbox itself:**

{% highlight bash %}
sudo pacman -S virtualbox
{% endhighlight %}

**2. After the first startup You may get this error message:**

"Kernel driver not installed (rc=-1908)"

![](https://i.stack.imgur.com/WKnp1.png)

2.1. To fix this, at first check which Kernel do You have:

{% highlight bash %}
uname -r
{% endhighlight %}

2.2. And then run this commands If You have e.g. kernel 4.14

{% highlight bash %}
sudo pacman -S linux414-virtualbox-host-modules
sudo modprobe vboxdrv
{% endhighlight %}

2.3. Finally restart Virtualbox

**3. Install Guest additions on VM:**

3.1. Click to Devices/Insert Guest Additions CD Images and go through the whole process (it will reboot the system finally)
(Optional recommendation: set Drag and Drop & Shared Folders to 'Bidirectional')

**4. Close the VM and set the default shared folders**
(also it is recommended setup a pendrive as a shared folder if You want to access both on VM and physical one) :

4.1. Settings/Shared Folders/Adds new shared folder:

Folder path: path of the folder You want to share with VM (e.g. /media/user/stuffs)
Folder name: whatever you want
(Auto-mount has to be ticked!)

![]({{ site.baseurl }}/assets/images/2018/05/Screenshot-from-2018-05-15-20-24-01-e1526410290521.png)

**5. Enable to switch between workspaces with CTRL+ALT+ARROW hotkey:**

File/Preferences/Input: deselect 'Auto Capture Keyboard' option

![]({{ site.baseurl }}/assets/images/2018/05/Screenshot-from-2018-05-15-20-33-08-e1526410320890.png)

**6. Open VirtualBox to second workspace automatically after startup** (Gnome specific solution):

6.1. Install [Auto Move Windows Extension](https://extensions.gnome.org/extension/16/auto-move-windows/)
6.2. Open Tweaks and navigate to Extensions/Auto move windows then add a new rule:

select 'Oracle VM VirtualBox' from the list
workspace: 2

![]({{ site.baseurl }}/assets/images/2018/05/Screenshot-from-2018-05-15-20-40-12.png)
