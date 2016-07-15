Debian is awesome Linux distribution however sometime we suffer from its pretty conservative policy to introduce newer version of common packages. For instance Debian 8 Jessie ships with LibreOffice 4.3, which is pretty old, currently there is LibreOffice 5.0 and LibreOffice 5.1 available.


Instead of downloading ```deb``` package directly from LibreOffice website and installing it with ```dpkg```, we can configure ```jessie-backports``` repository provided by Debian, which contains backported versions of packages from snatch (Debian 9). If we want to have a well tested and very stable system installing from ```jessie-backports``` repository is not recommended, because Debian maintainers cannot spend so much time on testing backported packages as they do for packages from standard repository. However for those willing to have the newest version it is a way to go.

To configure ```jessie-backports``` repository we start from modifying our ```/etc/apt/sources.list``` file by appending below line.

{% highlight text %}

deb http://ftp.debian.org/debian jessie-backports main

{% endhighlight %}


Then we have to resynchronize package index, using below command.

{% highlight text %}

apt-get update

{% endhighlight %}

The only thing left is to actually install the LibreOffice package.

{% highlight text %}

apt-get -t jessie-backports install libreoffice

{% endhighlight %}

The ```-t``` option allows us to specify from where that package will be retrieved.

If we had installed version 4.3 before, it is advisable to remove it prior to installing version 5.1.

We can verify all installed LibreOffice packages using ```dpkg``` tool.

{% highlight text %}

dpkg -l | grep libreoffice

{% endhighlight %}

Then remove all of them using ```apt-get remove```

{% highlight text %}

apt-get remove libreoffice libreoffice-avmedia-backend-gstreamer libreoffice-base libreoffice-base-core libreoffice-base-drivers libreoffice-calc libreoffice-common libreoffice-core libreoffice-draw libreoffice-evolution libreoffice-gnome libreoffice-gtk libreoffice-help-en-us libreoffice-impress libreoffice-java-common libreoffice-math libreoffice-report-builder-bin libreoffice-sdbc-firebird libreoffice-sdbc-hsqldb libreoffice-style-galaxy libreoffice-style-tango libreoffice-writer

{% endhighlight %}
