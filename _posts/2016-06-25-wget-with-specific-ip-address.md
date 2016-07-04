Wget is extremely useful Linux tool. It allows us to download files in non-interactive manner from the network. It support HTTP, HTTPS and FTP. It has multiple handy options, but today we will concentrate on ```--bind-address```.


By default ```wget``` binds to first available non local network interface IP address. That is not always a desired behavior, for instance when we have multiple IP addresses on a computer (either due to several network interfaces or using a software like keepalived) and we want to use a specific IP address. The potential reason are firewall restrictions. The ```wget``` utility makes it simple by providing us with the option ```--bind-address``` that allows us to specify the IP address to which ```wget``` will bind.

{% highlight text %}

 wget --bind-address=192.168.123.10 http://www.mywebsite.com

{% endhighlight %}

To sum up, additional quick tip, to display ip addresses assigned to computer in Linux we can use the ```ip addr``` command.

{% highlight text %}

ip addr

{% endhighlight %}
