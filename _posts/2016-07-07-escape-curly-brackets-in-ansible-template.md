One of the most common ansible modules is a template module, which uses Jinja2 templates. Template module process Jinja2 templates and create or modify a file on managed systems.

Jinja2 templates are pretty straight forward, however sometimes we can face a problem of escaping some characters, especially curly brackets (```{``` and ```}```). The reason for that is that curly brackets are used in Jinja2 templates as a variable placeholder, like presented in below example.

{% highlight text %}

<Directory /var/www/vhosts/{{ vhost_name }}>
        AllowOverride All
</Directory>

{% endhighlight %}

In the above example our variable is called ```vhost_name```. Its value would be evaluated by substituting variable name with the value provided to the template. So how we can use curly brackets in the template in such a way that they won't be treated as variable placeholder. The easiest solution is to put the as string between another set of curly brackets as presented below.

 {% raw %}

\"timestamp\": \"%{{ "{" }}%Y-%m-%dT%H:%M:%S%z{{ "}" }}t\",

 {% endraw %}

So to use ```{``` in the template we write the following expression:

{% raw %}

{{ "{" }}

{% endraw %}

and to use ```}``` in the template we use:

{% raw %}

{{ "}" }}

{% endraw %}

Template engine treats the above as variables and it evaluates them on run time. However instead of providing the variable name in the template, and variable's value in the ansible playbook. We provide hard coded string value.

Below is the full example of custom log format for Apache access log, which transforms information available from Apache into a json array.

{% raw %}

LogFormat "{ \
      \"host\":\"{{ server_name }}\", \
      \"path\":\"/var/log/httpd/{{ vhost_name }}-logstash_access.log\", \
      \"tags\":[\"{{ vhost_name }}\",\"{{ server_name }}\"], \
      \"message\": \"%h %l %u %t \\\"%r\\\" %>s %b\", \
      \"timestamp\": \"%{{ "{" }}%Y-%m-%dT%H:%M:%S%z{{ "}" }}t\", \
      \"clientip\": \"%a\", \
      \"realclientip\": \"%{{ "{" }}X-Forwarded-For{{ "}" }}i\", \
      \"duration\": %D, \
      \"status\": %>s, \
      \"request\": \"%U%q\", \
      \"urlpath\": \"%U\", \
      \"urlquery\": \"%q\", \
      \"method\": \"%m\", \
      \"bytes\": %B, \
      \"vhost\": \"%v\" \
    }" logstash_json_{{ vhost_name }}

{% endraw %}
