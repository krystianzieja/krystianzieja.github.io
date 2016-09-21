Nagios is one of the most useful pieces of software when you need to periodically check service availability. Nagios is considered one of the best open source monitoring packages on the market. Most common way to configure nagios is be editing configuration in a text editor. It is a great method however it can be error prone, due to typos or other mistakes.


Today I would like to share a small tip how we can verify nagios configuration prior to reloading or restarting the servive. It is very easy one liner.

{% highlight text %}
nagios -v /etc/nagios/nagios.cfg
{% endhighlight %}

When output contains below, we can safely reload nagios service.

{% highlight text %}
Total Warnings: 0
Total Errors:   0

Things look okay - No serious problems were detected during the pre-flight check

{% endhighlight %}
