Built-in iptables chains are set with ACCEPT policy, which is insecure. The iptables policy can be set either to ACCEPT or DROP. DROP is much better choice from security reasons.


We can change an iptables policy with with a single command. Below command demonstrates how to change the policy on INPUT and FORWARD chains to DROP.

{% highlight text %}

iptables -P INPUT DROP
iptables -P FORWARD DROP

{% endhighlight %}

To make your changes persistent, we need to execute one more command (my assumption is package iptables-persistent is installed).

{% highlight text %}

iptables-save > /etc/iptables/rules.v4

{% endhighlight %}

For IPv6 version of iptables we have to issue below commands:

{% highlight text %}

ip6tables -P INPUT DROP
ip6tables -P FORWARD DROP
ip6tables-save > /etc/iptables/rules.v6

{% endhighlight %}
