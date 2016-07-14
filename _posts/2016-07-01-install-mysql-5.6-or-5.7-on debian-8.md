Debian 8 ships MySQL Server 5.5, which for some use cases can be too old. However there is a simple way to install MySQL Server in newer version on Debian Jessie.


First we need to download ```deb``` package that will configure MySQL apt repository. The newest version of this package can be found [http://dev.mysql.com/downloads/repo/apt/](http://dev.mysql.com/downloads/repo/apt/). We can download ```deb``` package using ```wget``` command.

{% highlight text %}

wget http://dev.mysql.com/get/mysql-apt-config_0.7.3-1_all.deb

{% endhighlight %}

Then we need to install it, using ```dpkg``` tool.

{% highlight text %}

dpkg -i mysql-apt-config_0.7.3-1_all.deb

{% endhighlight %}

During the installation of this package we will be asked which version of MySQL either 5.6 or 5.7 we want to use.

After configuring APT repository, we need to resynchronize the package index files from their sources, using ```apt-get update``` command.

{% highlight text %}

apt-get update

{% endhighlight %}

The last step is to install MySQL Server.

{% highlight text %}

apt-get install mysql-community-server

{% endhighlight %}


If we had installed MySQL 5.5 Server provided by standard Debian repository before, it would be the best to create a dump of all databases (or any other form of backup) and remove it from the server before proceeding with the above steps. Package removal can be performed with ```apt-get purge```. Then after installing MySQL 5.6 or MySQL 5.7 restore the databases.


{% highlight text %}

apt-get purge package-name

{% endhighlight %}
