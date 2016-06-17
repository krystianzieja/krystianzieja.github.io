HAProxy is a well know open source proxy server. For me the selling point for HAProxy is its flexibilit of its configuration and stability. Today I will present how we can use HAProxy to redirect a single url, to another domain.


In this example we will simulate a simple scenario: We want to redirect web page ```http://testdomain.com/my-new-product``` to another URL ```http://mynewproduct.com```. All other URLs should ot be affected.

The first step is to define the frontend.

{% highlight text %}

frontend frontend_mydomain
  bind 10.10.100.103:80
  mode http
  option httplog
  option forwardfor
  log global

  default_backend server1

{% endhighlight %}


To match the URL we want to redirect, we need to define an ACL (access control list).

{% highlight text %}

acl is_my_url path -i /my-new-product

{% endhighlight %}

We start configuring ACL with ```acl``` keyword after that we provide the ACL name, in the example I used the name ```is_my_url```. Then we specify the test that HAProxy will perform on a request. The ```path``` keyword extracts the request's URL path, which starts at the first slash and ends before the question mark (without the host part). The ```-i``` directive means that we will match extracted URL path in case insensitive manner to a hard coded string ```blog```.

Next we need to define a redirect. This can be done using ```redirect location``` directive.

{% highlight text %}

redirect location http://mynewproduct.com code 301 if is_my_url

{% endhighlight %}

After ```redirect location``` directive we provide an address to which we want to redirect ```http://mynewblog.com``` and provide HTTP code that will be used in redirection ```code 301```. Most common codes for redirection are **301 - Moved Permanently** and **302 - Found**.

There is a difference between those two HTTP status codes. When search engine sees 301 status code it understands that original web page does not exist any longer. Therefore search engine gets new location header from the response and update the web page URL in their index with the new URL. The page rank will also be transfered. The browser works like that, when it sees 301 code it caches the mapping of old URL with new URL. The effect would be that the client browser won't request the original URL anymore, at least until the browser cache would be removed.
In contrast when search engine receives 302 code, it will index both URLs original and the new location. Therefore the original URL would still exists in search engine index. When client browser received 302, next time a client tries to access that URL the browser will use the original location.


After we modified the configuration file, we need to verify it using below commands.

{% highlight text %}

haproxy -c -f /etc/haproxy/haproxy.cfg

{% endhighlight %}

We should receive ```Configuration file is valid```. If configuration is correct we need to reload HAProxy. Depending on our OS version we need to use one of below commands.

{% highlight text%}

service haproxy reload

systemctl reload haproxy

{% endhighlight %}


For the sake of completeness the whole frontend configuration would look like below.

{% highlight text%}

frontend frontend_mydomain
  bind 10.10.100.103:80
  mode http
  option httplog
  option forwardfor
  log global

  acl is_my_url path -i /my-new-product
  redirect location http://mynewproduct.com code 301 if is_my_url  

  default_backend server1

{% endhighlight %}
