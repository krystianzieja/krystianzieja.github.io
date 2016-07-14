On Debian based Linux systems we use apt package manager, which makes a decent job for managing our packages. However from time to time something can go wrong and then we need to reinstall the package.


We can do that using ```apt-get``` command provided below.

{% highlight text %}

apt-get --reinstall install package-name

{% endhighlight %}

Below example shows how to reinstall MySQL Server package.

{% highlight text %}

apt-get --reinstall install mysql-community-server

{% endhighlight %}
