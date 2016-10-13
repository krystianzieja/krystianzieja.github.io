PSAD is ...


Install PSAD

{% highlight text %}

apt-get install psad

{% endhighlight %}

Next we need to modify the configuration file ```/etc/psad/psad.conf```.

We start from setting email address for notifications.

{% highlight text %}

EMAIL_ADDRESSES             test@email.com;

{% endhighlight %}


Then we set hostname.

{% highlight text %}

HOSTNAME                    server-name;

{% endhighlight %}

Next we can adjust the default settings that defines, what type of scans will trigger PSAD alert.

{% highlight text %}

PORT_RANGE_SCAN_THRESHOLD   1;

PROTOCOL_SCAN_THRESHOLD     4;

{% endhighlight %}

Then we set scan persistence

{% highlight text %}

ENABLE_PERSISTENCE          N;

SCAN_TIMEOUT                3600;  ### seconds

{% endhighlight %}

Next we enable auto blocking of offending hosts.

{% highlight text %}

ENABLE_AUTO_IDS             Y;

{% endhighlight %}

It is important to note that for PSAD to work, you have to configure iptables. We just need to add two rule one for INPUT table and one for FORWARD table and we make them persistent across system reboots.


{% highlight text %}

iptables -A INPUT -j LOG

iptables -A FORWARD -j LOG

iptables-save > /etc/iptables/rules.v4

{% endhighlight %}

If you have also IPv6 enabled you should add below.

{% highlight text %}

ip6tables -A INPUT -j LOG

ip6tables -A FORWARD -j LOG

ip6tables-save > /etc/iptables/rules.v6

{% endhighlight %}

The last thing to do is to point PSAD to a file, which contains iptables log. On RHEL based distributions that would be ```/var/log/messages``` and on Debian based distributions i.e. Ubuntu ```/var/log/syslog```, so we need to point to proper file in ```/etc/psad/psad.conf``` file.

{% highlight text %}

IPT_SYSLOG_FILE             /var/log/syslog;

{% endhighlight %}


We can verify PSAD status with the below command.

{% highlight text %}

psad --Status

{% endhighlight %}
