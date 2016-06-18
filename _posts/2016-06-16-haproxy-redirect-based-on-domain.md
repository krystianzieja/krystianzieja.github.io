Another post about HAProxy flexibility. Today we will create a redirect based on domain name. However we can use the same approach to route traffic based on domain name to different backend servers.


The scenario for today is to redirect all requests to ```mytestdomain.com``` to external domain ```myredirecteddomain.com```.

First we need to create a frontend in haproxy configuration file - ```/etc/haproxy/haproxy.cfg```.

{% highlight text %}

frontend frontend_mydomain
  bind 10.10.100.103:80
  mode http
  option httplog
  option forwardfor
  log global

  default_backend server1

{% endhighlight %}

Setup as as above will create a frontend listening on ip address ```10.10.100.103``` and port TCP 80. All the requests will be passed to backend server named ```server1```. In our test scenario we assume that ```10.10.100.103``` is NAT'ed on a router to some public IP address, that is used by two domains ```mytestdomain.com``` and ```myothertestdomain.com```. We want to redirect the traffic for ```mytestdomain.com``` to externally hosted domain named ```myredirecteddomain.com```. The requests to ```myothertestdomain.com``` should reach backend server named ```server1```.

To match traffic that needs to be redirected we need to create access control list (ACL).

{% highlight text %}

acl is_mytestdomain.com hdr(host) -i mytestdomain.com

{% endhighlight %}

To define access control list we use ```acl``` directive which is followed by ACL name, in our case ```is_mytestdomain.com``` then it is followed by a check to be performed on a request. The ```hdr``` keyword extracts the specified header from the HTTP request. If we want to route traffic based on domain name, we would need to use ```host``` header, because in that header domain is defined, so the full criteria to match against would be ```hdr(host)``` we compare value from the header in case insensitive manner , due to ```-i``` operator, to a string ```mytestdomain.com```.

After we defined ACL we need to specify an action that will take place when check from acl is true.

{% highlight text %}

acl is_mytestdomain.com hdr(host) -i mytestdomain.com
redirect location http://myredirecteddomain.com code 301 if is_mytestdomain.com

{% endhighlight %}

To force a redirect we use ```redirect location``` directive followed by target URL. We can also specify a HTTP redirection code, two most common are 301 and 302. The detailed difference description between the two can be found in that [post]({% post_url 2016-06-15-haproxy-redirect-based-on-url %}).

The final configuration is provided below.

{% highlight text %}

frontend frontend_mydomain
  bind 10.10.100.103:80
  mode http
  option httplog
  option forwardfor
  log global

  acl is_mytestdomain.com hdr(host) -i mytestdomain.com
  redirect location http://myredirecteddomain.com code 301 if is_mytestdomain.com

  default_backend server1

{% endhighlight %}

In case we need to redirect both ```mytestdomain.com``` and ```www.mytestdomain.com```, we have two options either we define two ACLs and use logical OR operator, or we switch ```hdr``` keyword to ```hdr_end```. The first option is pretty straight forward. We create the second ACL almost identical as the first with the exception to string we compare the value of ```hdr(host)``` to, which in that case is ```www.mytestdomain.com```. Then in the line with ```redirect location``` we join two ACLs using logical OR: ```if is_mytestdomain.com or is_www.mytestdomain.com```, which means that redirect will occur whenever one of our ACLs will be true.

{% highlight text %}

acl is_mytestdomain.com hdr(host) -i mytestdomain.com
acl is_www.mytestdomain.com hdr(host) -i www.mytestdomain.com
redirect location http://myredirecteddomain.com code 301 if is_mytestdomain.com or is_www.mytestdomain.com

{% endhighlight %}


The second option with replacing ```hdr``` with ```hdr_end``` is probably simpler, however it is not equivalent to the above. The ```hdr_end``` will match a suffix of the header, instead of whole server. Therefore ```hdr_end(host) -i mytestdomain.com``` will match both ```mytestdomain.com``` and ```www.mytestdomain.com```. However it will also match ```test.mytestdomain.com```.

{% highlight text %}

acl is_mytestdomain.com hdr_end(host) -i mytestdomain.com
redirect location http://myredirecteddomain.com code 301 if is_mytestdomain.com

{% endhighlight %}
