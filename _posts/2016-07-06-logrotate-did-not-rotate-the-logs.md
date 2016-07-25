Logrotate was designed to ease the life of system administrator, when it comes to log management. IT allows us to rotate, compress and remove the log files, according to our configuration. However not every ```logroate``` configuration option is intuitive.

I will introduce a simple example, for weekly log rotation with detailed description of most people expectations and how it actually works. In RHEL based Linux systems, the main ```logrotate``` configuration file is ```/etc/logrotate.conf```. It defines the basic staff like appending date extension to rotated logfiles, how long we would keep the logfiles and specify include directory from which ```logrotate``` will read additional configuration files that are usually specific to application or service.

{% highlight text %}

weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
dateext

# uncomment this if you want your log files compressed
#compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d

# no packages own wtmp and btmp -- we'll rotate them here
/var/log/wtmp {
    monthly
    create 0664 root utmp
	minsize 1M
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}

{% endhighlight %}

The include directory is ```/etc/logrotate.d```. In our example we would use ```maillog``` file. The ```logrotate``` configuration for maillog is defined in ```/etc/logrotate.d/syslog```.

{% highlight text %}

/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    postrotate
	/bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}

{% endhighlight %}

From above configuration files we expect to rotate ```maillog``` on a weekly basis, to append date extension rotated log files and to keep the logs for four weeks.

Logrotate functions automatically due to the cron job that is created during installation ```/etc/cron.daily/logrotate```. Cron jobs defined in cron.daily will run according to the schedule defined in ```/etc/anacrontab``` file on RHEL 7 based system.

{% highlight text %}

# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# the maximal random delay added to the base delay of the jobs
RANDOM_DELAY=45
# the jobs will be started during the following hours only
START_HOURS_RANGE=3-22

#period in days   delay in minutes   job-identifier   command
1	5	cron.daily		nice run-parts /etc/cron.daily
7	25	cron.weekly		nice run-parts /etc/cron.weekly
@monthly 45	cron.monthly		nice run-parts /etc/cron.monthly

{% endhighlight %}

Anacron introduces random delay, which obviously can be changed according to our needs, and defines the start hours for cronjobs defines in ```/etc/cron.daily```, ```/etc/cron.weekly``` and ```/etc/cron.monthly```. Such setup prevents your infrastructure (for instance a remote file server) from beating when 1000 servers would rotate logs at the same time. Therefore the default setup works just fine in many situations, however not all. For instance if we want to guarantee that maillog would be rotated on Sunday we would have to adjust things.

Except the cron job ```logrotate``` is using a state file, that enables it to track which files require rotation. Below is example contents of the status file ```/var/lib/logrotate.status```

{% highlight text %}

cat /var/lib/logrotate.status
logrotate state -- version 2
"/var/log/yum.log" 2016-4-4-20:0:0
"/var/ossec/logs/ossec.log" 2016-7-24-3:8:1
"/var/log/up2date" 2016-4-4-20:0:0
"/var/log/freshclam.log" 2016-7-1-3:8:1
"/var/log/wtmp" 2016-4-4-20:0:0
"/var/log/spooler" 2016-7-17-3:42:1
"/var/log/btmp" 2016-7-1-3:8:1
"/var/log/rhsm/rhsmcertd.log" 2016-7-17-3:42:1
"/var/log/maillog" 2016-7-17-3:42:1
"/var/log/wpa_supplicant.log" 2016-4-4-20:0:0
"/var/log/secure" 2016-7-17-3:42:1
"/var/ossec/logs/active-responses.log" 2016-4-9-3:40:2
"/var/log/ppp/connect-errors" 2016-4-4-20:0:0
"/var/log/rhsm/rhsm.log" 2016-7-17-3:42:1
"/var/log/messages" 2016-7-17-3:42:1
"/var/log/cron" 2016-7-17-3:42:1

{% endhighlight %}

The important thing to notice is the header ```logrotate state -- version 2``` on newer systems it will state ```version 2``` on older ones ```version 1```. The major difference is that in version 2 ```logrotate``` will also store time of the last rotation for particular log, when version on only
stored date. And it can make a huge difference.

Let's start from printing out current date.

{% highlight text %}

date
Sun Jul 24 18:39:32 CDT 2016

{% endhighlight %}

Below is the list of maillog log files from test system.


{% highlight text %}

ls -lrt /var/log/maillog*

-rw-------. 1 root root        0 Jun 20 03:22 /var/log/maillog-20160626
-rw-------. 1 root root 48072824 Jul  4 02:21 /var/log/maillog-20160704
-rw-------. 1 root root 35555661 Jul 10 03:34 /var/log/maillog-20160710
-rw-------. 1 root root 38517842 Jul 17 03:41 /var/log/maillog-20160717
-rw-------. 1 root root 49692448 Jul 24 18:30 /var/log/maillog

{% endhighlight %}

What is missing in the above list? The log file named ```/var/log/maillog-20160724```. It was 18:39 and looking at log files from previous weeks the log rotation occurs occurs around 3:30. LEt's check ```/var/log/cron*``` logfiles to prove that.

{% highlight text %}

grep -i logrotate /var/log/cron-20160717
2016-07-10T03:34:06.176071-05:00 lb03 run-parts(/etc/cron.daily)[7977]: finished logrotate
2016-07-11T03:15:01.301859-05:00 lb03 run-parts(/etc/cron.daily)[22977]: starting logrotate
2016-07-11T03:15:06.508115-05:00 lb03 run-parts(/etc/cron.daily)[23029]: finished logrotate
--- snip ---
2016-07-16T03:20:01.412037-05:00 lb03 run-parts(/etc/cron.daily)[13192]: starting logrotate
2016-07-16T03:20:08.054672-05:00 lb03 run-parts(/etc/cron.daily)[13244]: finished logrotate
2016-07-17T03:42:01.908538-05:00 lb03 run-parts(/etc/cron.daily)[29286]: starting logrotate

grep -i logrotate /var/log/cron

2016-07-17T03:42:04.969239-05:00 lb03 run-parts(/etc/cron.daily)[29325]: finished logrotate
2016-07-18T03:41:01.163150-05:00 lb03 run-parts(/etc/cron.daily)[12187]: starting logrotate
2016-07-18T03:41:03.809543-05:00 lb03 run-parts(/etc/cron.daily)[12222]: finished logrotate
--- snip ---
2016-07-24T03:08:01.902101-05:00 lb03 run-parts(/etc/cron.daily)[7062]: starting logrotate
2016-07-24T03:08:06.803429-05:00 lb03 run-parts(/etc/cron.daily)[7097]: finished logrotate

{% endhighlight %}

As we can see the logrotate was run at 2016-07-24T03:08:01, so why the file ```maillog-20160724``` was not created. The answer is pretty simple because ```logroate``` when it has state file in version 2 compares also the time portion, not only a date like in version 1. The ```logrotate``` run at 2016-07-24T03:08:01 and the state file shows for maillog the date of last rotation 2016-7-17-3:42:1. Hence when logrote run the difference was smaller then seven days and no log rotation occurred.

If we run now (around 18:40) ```logrotate``` cronjob manually, the log rotation will occur.

{% highlight text %}

/etc/cron.daily/logrotate

cat /var/lib/logrotate.status

logrotate state -- version 2
"/var/log/freshclam.log" 2016-7-1-3:8:1
"/var/log/wtmp" 2016-4-4-20:0:0
--- snip ---
"/var/log/maillog" 2016-7-24-18:43:17
--- snip ---
"/var/log/cron" 2016-7-24-18:43:17

{% endhighlight %}

Now we can verify that the file ```maillog-20160724``` was created.

{% highlight text %}

ls -lrt /var/log/maillog*
-rw-------. 1 root root 48072824 Jul  4 02:21 /var/log/maillog-20160704
-rw-------. 1 root root 35555661 Jul 10 03:34 /var/log/maillog-20160710
-rw-------. 1 root root 38517842 Jul 17 03:41 /var/log/maillog-20160717
-rw-------. 1 root root 49751510 Jul 24 18:43 /var/log/maillog-20160724
-rw-------. 1 root root    21953 Jul 24 18:45 /var/log/maillog

{% endhighlight %}

I hope the above is pretty clear. But what should we do, if we want to guarantee that ```maillog``` log file will be rotated every Sunday. Obviously we could run ```logrote``` with force option ```-f```, which instructs ```logrotate``` to rotate the log files, even if it thinks it is not required. However running force option for all the log files can not be the best option, because for instance ```/var/log/wtmp``` and ```/var/log/btmp```, which by default are rotated on monthly basis.
Instead of that approach we will modify ```logrotate``` configuration for syslog files and we will setup a separate cron job to run ```logrotate -f``` for only that configuration file.

Below is modified version of ```/etc/logrotate.d/syslog```

{% highlight text %}

weekly
# keep 4 weeks worth of backlogs
rotate 4
# create new (empty) log files after rotating old ones
create
# use date as a suffix of the rotated file
dateext

/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    postrotate
        /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}

{% endhighlight %}

Next we will create a custom logrotate bash script. We start from copying the original one located in ```/etc/cron.daily```.

{% highlight text %}

cp /etc/cron.daily/logrotate /usr/local/sbin/

{% endhighlight %}

and we modify it in the following way

{% highlight text %}

#!/bin/sh

/usr/sbin/logrotate -f /etc/logrotate.d/syslog
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0

{% endhighlight %}

The last step is to create a new cron job, we will use ```/etc/crontab``` file this time.

{% highlight text %}

0 2  *  *  *  0 root /usr/local/sbin/logrotate

{% endhighlight %}
