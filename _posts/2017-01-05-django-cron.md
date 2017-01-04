---
layout: post
title:  "Cron Job in Django"
date:   2017-01-05 01:35:33 +0530
categories: jekyll update
---

A week ago I was handed a task to run a backend program that would periodically push data from one project to another one. Although I have experience writing snippets in PHP and setting "cron jobs" on Linux (Ubuntu) Servers on AWS, I wanted to give Django a shot at this, eventhough there are better ways of doing this.

I started off with creating the said snippet in Django, that would pull chunks of data from Source Project and push those data to the new Django Project. 

And to help me create the backend process, I choose [Django-cron](http://django-cron.readthedocs.io/en/latest/introduction.html). Which is relatively simpler than writing [custom django-admin commands](https://docs.djangoproject.com/en/1.10/howto/custom-management-commands/).

Steps to use Django-cron is very simple, just follow the steps listed in the [link](http://django-cron.readthedocs.io/en/latest/installation.html). A fast-forward steps are as follows, note that it is assumed that you're in a virtualenv.

1.{% highlight shell %}
$ pip install django_cron
{% endhighlight %}

2.{% highlight shell %}
INSTALLED_APPS = [
    ...
    "django_cron",
]
{% endhighlight %}

3.{% highlight shell %}
./manage.py migrate django_cron
{% endhighlight %}

4.Create a new file `cron.py` in one of the app in your project directory and add the following codes.
{% highlight python %}
from django_cron import CronJobBase, Schedule

# create a class extending CronJobBase
class PushCronJob(CronJobBase):
    RUN_EVERY_MINS = 5 # runs every 5 mins
    schedule = Schedule(run_every_mins=RUN_EVERY_MINS)
    code = "my_app.super_awesome_push_cron" # has to be unique

    def do(self):
        # put in your snippet here
{% endhighlight %}

5. Once done, add a variable name `CRON_CLASSES` to your `settings.py` file.
{% highlight shell %}
CRON_CLASSES = [
    # in-quotes your_appname/any_subdirectory/cronjob_name
    "my_app.cron.PushCronJob",
]
{% endhighlight %}

And Voila! you have your cronjob created using django_cron!
To run your all your cron jobs, fetched from the list of CRON_CLASSES:
{% highlight shell %}
./manage.py runcrons
{% endhighlight %}

To run specific cron job
{% highlight shell %}
./manage.py runcrons "my_app.cron.PushCronJob"
{% endhighlight %}

To setup your Cron Job in Linux.

* create a shell script that would activate the virtual environment and run the cron_jobs.
First:
{% highlight shell %}
vi cron_job_init.sh
{% endhighlight %}

Second:
{% highlight shell %}
#!/bin/bash
source /path/to/my_env/bin/activate
delay(3) # [1]
python /path/to/my_project/manage.py runcrons
{% endhighlight %}

* Open Crontab with `-e` to create new Cron Jobs
{% highlight shell %} $ crontab -e {% endhighlight %}

* Add the following line to run the `cron_job_init.sh` script every 5 minutes.
{% highlight shell %}
# each star represent minute, hour, day of month, month of year, day of week
*/5 * * * * sh path/to/cron_job_init.sh
{% endhighlight %}

And there you have it!
Cron Job set to run every 5 minutes.

I spent a good half of the day trying to fix a problem where although cron_job was initiated, no result were generated. Only to realize that in the shell script, the `source /path/to/my_env/bin/activate` took certain amount of time to complete, within which the next line `runcrons` got executed. Since my environment was not set at that moment, my cron job failed. So, I decided to put a small delay of 3 seconds to make sure my environment was activated.

Today has been a good learning experience for me, tried out a new package and got it working soon.

Thank you for your time.