It is very common to see a 504 Gateway Timeout error using Nginx webserver. This timeout error is generated often by a number of reasons on the backend connection that is serving content. To fix 504 Gateway Time-out, you will have to figure out what configuration are you using.


Most probably the issue is related to ```fastcgi_read_timeout``` in websites configuration. To increase number of seconds before nginx will return 504 error, we need to modify our website configuration file, by adding the following line, in website configuration file.

```
vim /etc/nginx/sites-enabled/www.test.com
```

{% highlight text %}

fastcgi_read_timeout 900;

{% endhighlight %}

Below is example section from website configuration file.

{% highlight text %}

location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

    fastcgi_intercept_errors off;
    fastcgi_buffer_size 16k;
    fastcgi_buffers 4 16k;
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 900;
}

{% endhighlight %}

The last step is to restart nginx.

{% highlight text %}

systemctl restart nginx

{% endhighlight %}
