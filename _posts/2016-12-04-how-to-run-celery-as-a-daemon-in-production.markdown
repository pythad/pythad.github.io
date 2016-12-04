---
layout: post
title: How to run celery as a daemon?
excerpt: "Notes on how to run celery as a daemon using generic init-scripts, these should run on Linux, FreeBSD, OpenBSD, and other Unix-like platforms."
categories: [Notes, Python, Celery]
modified: 2016-12-04
comments: true
<!-- image:
  feature: https://dummyimage.com/1250x475/861d1d/ffffff&text=Runnig+Celery+as+a+daemon -->
---

# 1. Init-script: celeryd
## 1.1. celeryd
### What to do?

1. Create `/etc/init.d/celeryd` with the content from [celery repo](https://github.com/celery/celery/blob/master/extra/generic-init.d/celeryd)

2. Make `celeryd` executable


### How?

    sudo nano /etc/init.d/celeryd

Copy-paste code from [celery repo](https://github.com/celery/celery/blob/master/extra/generic-init.d/celeryd) to the file

Save `celeryd` (`CTR+X, y, Enter` from nano)

Run following commands from the terminal:

    sudo chmod 755 /etc/init.d/celeryd
    sudo chown root:root /etc/init.d/celeryd


## 1.2. Configuration
### What to do?

1. Create `/etc/default/celeryd`
2. Configure it


### How?

    sudo nano /etc/default/celeryd

Configure it depending on what you need. Options and template can be found in the [docs](http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#init-script-celeryd)

### My example:

{% highlight bash %}
CELERY_BIN="project/venv/bin/celery"

# App instance to use
CELERY_APP="project_django_project"

# Where to chdir at start.
CELERYD_CHDIR="/home/username/project/"

# Extra command-line arguments to the worker
CELERYD_OPTS="--time-limit=300 --concurrency=8"

# %n will be replaced with the first part of the nodename.
CELERYD_LOG_FILE="/var/log/celery/%n%I.log"
CELERYD_PID_FILE="/var/run/celery/%n.pid"

# Workers should run as an unprivileged user.
#   You need to create this user manually (or you can choose
#   a user/group combination that already exists (e.g., nobody).
CELERYD_USER="username"
CELERYD_GROUP="username"

# If enabled pid and log directories will be created if missing,
# and owned by the userid/group configured.
CELERY_CREATE_DIRS=1

export SECRET_KEY="foobar"
{% endhighlight %}


**Note**

You can set your environment variables in `/etc/default/celeryd`.
For more info about environment variable take a look at this [SO answer](http://stackoverflow.com/questions/22759798/why-are-my-environment-variables-not-detected-when-starting-up-celery)

### Test it

You can check if the worker is active by:

    sudo /etc/init.d/celeryd start
    sudo /etc/init.d/celeryd status


Don't forget to stop the worker:

    sudo /etc/init.d/celeryd stop 

# 2. Init-script: celerybeat
## 2.1. celerybeat
### What to do?

1. Create `/etc/init.d/celerybeat` with the content from [celery repo](https://github.com/celery/celery/blob/master/extra/generic-init.d/celerybeat)

2. Make `celerybeat` executable


### How?

    sudo nano /etc/init.d/celerybeat

Copy-paste code from [celery repo](https://github.com/celery/celery/blob/master/extra/generic-init.d/celerybeat) to the file

Save `celerybeat` (`CTR+X, y, Enter` from nano)

Run following commands from the terminal:

    sudo chmod 755 /etc/init.d/celerybeat
    sudo chown root:root /etc/init.d/celerybeat


## 2.2. Configuration
### What to do?

Configurate either `/etc/default/celerybeat` or stick with `/etc/default/celeryd`

### How?

    sudo nano /etc/default/celerybeat

Configure it depending on what you need. Options and template can be fount in the [docs](http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#init-script-celerybeat)

**Note**

If you don't require any special configuration you can use `/etc/default/celeryd` as you config file. Just don't create `/etc/default/celerybeat`

### Test it

You can check if the beat is active by:

    sudo /etc/init.d/celerybeat start
    sudo /etc/init.d/celerybeat status

Don't forget to stop the worker:

    sudo /etc/init.d/celerybeat stop 


# 3. Maintenance

As it was show you can control worker and beat with the following commands:

    /etc/init.d/celeryd {start|stop|restart}
    /etc/init.d/celerybeat {start|stop|restart}

If you want to see the logs you can open `CELERYD_LOG_FILE` provided in `/etc/default/celeryd`. In our example is was `/var/log/celery/worker1.log`:

    sudo nano /var/log/celery/worker1.log 


# Resources

 - [SO: How to run celery as a daemon in production?](http://stackoverflow.com/questions/22759798/why-are-my-environment-variables-not-detected-when-starting-up-celery)
 - [Celery: Pereodic tasks](http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html)
 - [Celery: Daemonizing](http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#init-script-celerybeat)
 - [Celery: generic-init.d](https://github.com/celery/celery/tree/master/extra/generic-init.d)
 - [Askubuntu: How to install an init.d script?](http://askubuntu.com/questions/335242/how-to-install-an-init-d-script)
 - [SO: Why are my environment variables not detected when starting up celery?](http://stackoverflow.com/questions/22759798/why-are-my-environment-variables-not-detected-when-starting-up-celery)