Many networks require us to control also outgoing traffic. In the past it was pretty easy, when our servers had to contact with the server placed in Internet, we just add entry to our Access Control List (ACL) with the server's public IP Address. However currently usage of services in the cloud is growing in popularity, and things got more complicated for network administrators. Public IP addresses of services like maps.google.com or db.us.clamav.net can change over night. Therefore specifying public IP addresses in access control lists for those type of services is not an effective technique.


Below article will explain how to configure Cisco ASA, to allow us to use hostnames in access control lists. That feature is available since Cisco ASA 8.4. This feature relies on new object type called fully qualified domain name (fqdn). FQDN object represents a hostname. The only requirement to use this feature on top of proper ASA version is access to DNS servers, you can use a public DNS server or the one located in your private network.

We start the configuration with setting up DNS server in Cisco ASA, so that ASA can resolve FQDN objects to ip address or ip addresses.

{% highlight text %}

domain-name mydomain.com
!
dns domain-lookup outside
dns server-group DefaultDNS
 name-server 208.67.222.222
 name-server 208.67.220.220
 domain-name mydomain.com

{% endhighlight %}

The ```domain-name``` command in global context sets the default domain name to be used be our Cisco ASA.

The ```dns domain-lookup interface_name``` command is used to enable the ASA to send DNS requests to a DNS server to perform a name lookup for supported commands. In that command we need to specify the interface name used to connect to DNS server. In the example above we used outside interface because we connect to public DNS servers. To get a list of interfaces available on our device we can issue ```show interfaces``` command.

The ```dns server-group group_name``` command allows us to define DNS Server group, in which we can specify the domain name, name servers, number of retries, and timeout values. The default group name used by ASA is ```DefaultDNS```.

The ```name-server ip_address``` command in dns server-group context, allows use to specify the DNS server IP address.

The ```domain-name``` command in dns server-group context sets the default domain name to be used be our Cisco ASA, when using DNS servers defined in that dns server group.

The next step is to create an FQDN object.

{% highlight text %}

object network maps.google.com
 fqdn v4 maps.google.com

{% endhighlight %}

First we create an object of type network using the command ```object network object_network_name```. You can use anything for ```object_network_name``` in the example I used ```maps.google.com``` just for the sake of configuration readability. In object network context we specify fqdn object, which in practice is a hostname that Cisco ASA will resolve to IP address or a set of IP addresses using DNS query. FQDN objects can work with either IPv4 or IPv6. The full command is ```fqdn v4 hostname_to_be_resolved``` for IPv4 or ```fqdn v6 hostname_to_be_resolved```.

Then we only need to use this object as access control entry (ACE) in access control list (ACL).

{% highlight text %}

access-list apps_access_in permit tcp any object maps.google.com eq www

{% endhighlight %}

We can verify that fqdn was resolved correctly using below command.

{% highlight text %}

sh access-list apps_access_in | inc maps.google.com

{% endhighlight %}

Result:

{% highlight text %}

access-list apps_access_in line 62 extended permit tcp any fqdn maps.google.com (resolved) eq www 0x6714177f

{% endhighlight %}

We can also use the ```show dns``` command to display currently resolved DNS addresses for all or specified fully qualified domain name (FQDN) hosts.

{% highlight text %}
# show dns

Name: cdn.redhat.com
  Address: 173.222.216.251                               TTL 00:01:08

Name: www.surveygizmo.com
  Address: 54.230.7.57                                   TTL 00:01:13
  Address: 54.230.7.73                                   TTL 00:01:13
  Address: 54.230.7.100                                  TTL 00:01:13
  Address: 54.230.7.148                                  TTL 00:01:13
  Address: 54.230.7.159                                  TTL 00:01:13
  Address: 54.230.7.201                                  TTL 00:01:13
  Address: 54.230.7.246                                  TTL 00:01:13
  Address: 54.230.7.48                                   TTL 00:01:13

{% endhighlight %}

To show the DNS cache, we use the ```show dns-hosts``` command.

{% highlight text %}

# show dns-hosts

Host                     Flags      Age Type   Address(es)
www.surveygizmo.com      (temp, OK) 0    IP    54.192.122.163  54.192.122.172
					       54.192.122.7  54.192.122.36
					       54.192.122.145  54.192.122.153
					       54.192.122.156  54.192.122.160
  d3gvv5iecquak.cloudfront.
hooks.slack.com          (temp, EX) 0    IP    54.230.120.39

{% endhighlight %}
