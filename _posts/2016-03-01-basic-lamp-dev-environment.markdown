---
layout		: 	post
title		:	"Basic LAMP Development Envinronment"
date		: 	2016-03-01 20:21:00 +530
categories	: 	tech
---

I have been a Web Developer for some time now. I used to work on Windows machine, I had installed XAMPP which caters to my needs back then. At first I had to manually start and stop the Apache, MySQL services. And later on, I found the setting to auto-matically start those services on Boot Up.

Now that I am on Ubuntu, for a fresh installation of LAMP stack I cannot recommend this particular blog [1] from Digital Ocean enough. I have used this site numerous times for installation of LAMP Stack on different machines.

What I really wanted to emphasize on is the beauty of Soft Link in Linux, and how it literally saved me hours, in accumulation, from not typing my root password. What most beginners usually do is, Copy & Paste an existing project or clone a git project directly in the /var/www/html folder. This leads to two things:

* Some one is going to get tired of entering password and will most probably do the following and the security risks with this move is just countless.
{% highlight shell %}chmod 777 to /var/www/html {% endhighlight%}

* Rest of them are going to continue punching in the password until some one tells them of either the above point or tell them what I am about to write here.

I am certain almost everyone does what I am about to tell you. But this is for beginners.

I use what's called a Soft Link, to know a little bit more about it [2]. 
You're basically creating a soft link in the /var/www/html that points to any other location on your machine that does not requires root credentials

Syntax:
{% highlight shell %}
 sudo ln -s /path/to/your/folder /var/www/html/somename
{% endhighlight %}

So, now you're project directory is sitting pretty in some folder that you have full control over and doesn't require any extra permission to run.

The output from this?
1. No more password prompt for permission for any change/add/remove
2. Cleaner Root Directory


References/URLs:

[1] https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04

[2] http://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/

Thank you for your time.
