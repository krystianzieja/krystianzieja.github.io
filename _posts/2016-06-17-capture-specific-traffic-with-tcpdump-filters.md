Tcpdump is a standard utility for capturing network packets in multiple operating systems. Output from tcpdump is often used by wireshark, which is de facto the standard tool for analyzing network communication. By default the tcpdump  will capture all the network packets, which in many cases is not a desired, because it will contain to many packets that are not interesting for our analysis.


To resolve that problem we can use filters for tcpdump, which can limit the network packets that will be captured to those that satisfy our filter. The simple form of tcpdump command is:

{% highlight text %}

tcpdump -i eth0 -s 65535 -w my_dump.pcap

{% endhighlight %}

The ```-i``` parameter allows us to specify the interface from which we will gather network packets. The ```-s``` parameter allows us to specify how many bytes of data from each packet will be gathered. The ```-w``` parameter allows us to specify the file to which raw packets will be saved.

If we want to use filter expressions the tcpdump command is modified by appending filter expression at the end.

{% highlight text %}

tcpdump -i eth0 -s 65535 -w my_dump.pcap filter_expression

{% endhighlight %}


The detailed description of tcpdump filters can be found in map pages.

{% highlight text %}

man 7 pcap-filter

{% endhighlight %}

In the example below we will define a filter that captures all traffic to destination ports 80 and 443 and which destination is not a private network (in other words the destination host is not within 10.0.0.0/8 subnet, 172.16.0.0/12 subnet and 192.168.0.0/16 subnet).

{% highlight text %}

tcpdump -i eth0 -s 65535 -w http_https_external_capture.pcap '(dst port 80 or dst port 443) and (dst net not 10.0.0.0/8) and (dst net not 172.16.0.0/12) and (dst net not 192.168.0.0/16)'

{% endhighlight %}

Let's decompose our filter expression ```dst port``` filter allows us to specify the destination port, we are interested in two ports 80 and 443, so we join two ```dst port``` filters with logical OR using ```or``` operator. The resulting expression is ```(dst port 80 or dst port 443)```.

The ```dst net``` filter enables us to specify the destination network. We want to gather all network packets except those designated to private networks. Therefore we need to negate ```dst net``` filter with logical NOT operator ```not```. The example resulting expression are: ```(dst net not 10.0.0.0/8)```, ```(dst net not 172.16.0.0/12)```, ```(dst net not 192.168.0.0/16)```.

The last step is to join all our filter expressions with logical AND using ```and``` operator, which means that only packets that match all our conditions will be gathered.
