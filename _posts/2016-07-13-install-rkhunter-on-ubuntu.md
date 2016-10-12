As one of the steps to harden our Linux system, we can install rkhunter package. Rootkit Hunter is a tool that checks our system for presence of rootkits.


Installation and configuration of Rootkit Hunter is pretty easy, just follow the example below. The test system used was Ubuntu 16.04 LTS.

Install rkhunter package.

{% highlight text %}

sudo apt-get install rkhunter

{% endhighlight %}

Next we need to set system defaults for our rkhunter. The file with those settings is ```/etc/default/rkhunter```. We setup it as in below example.

{% highlight text %}

CRON_DAILY_RUN="true"

CRON_DB_UPDATE="true"

DB_UPDATE_EMAIL="false"

REPORT_EMAIL="root"

APT_AUTOGEN="yes"

{% endhighlight %}

```CRON_DAILY_RUN="true"``` means that Rootkit Hunter will run as cron job on a daily basis.

```CRON_DB_UPDATE="true"``` means that Rootkit Hunter database will be updated every week.

```DB_UPDATE_EMAIL="false"``` this option disables email with status of Rootkit Hunter database update.

```REPORT_EMAIL="root"``` this option allows to set email address to which reports would be sent, you can provide your email here, or keep the default setting, which is root and redirect ```root``` to your email in ```/etc/aliases``` file (do not forget to run newalias command after modifying the file).

```APT_AUTOGEN="yes"``` tells apt package manager to update Rootkit Hunter database after each package install or removal.


The main RootKit configuration file is ```/etc/rkhunter.conf```, before running RootKit hunter you should adjust that configuration to suit your needs for instance if you need to allow root login over SSH (not recommended), you should set the below option.

{% highlight text %}
ALLOW_SSH_ROOT_USER=yes
{% endhighlight %}

We can verify our setup by running cron job manually ```/etc/cron.daily/rkhunter``` and verifying the log file ```/var/log/rkhunter.log```
