FTP is very old protocol, which is not so commonly used today as it was 10 years ago. However there are situations when it still can be very useful. For instance when recovering data using SystemRescueCD or similar Linux distribution, we would need to copy files between servers.


There are multiple ftp client tools on Linux systems, one of my favorites is lftp. When we have to move the whole directory tree between client and server or other way around, lftp offers very neat ```miror``` command. The basic systax is pressented below:

{% highlight text %}

mirror source target

{% endhighlight %}

The above command will download whole directory tree starting from ```source``` folder from FTP server to local computer folder specified by ```target``` parameter. It is important to remember that if target directory ends with slash, the source basename is appended to the target directory name.

A simpler to remember form of the above command is:

{% highlight text %}

mirror remote_directory local_directory

{% endhighlight %}


If instead of download whole directory tree from the server, we want to upload it, we can use the reverse option, which is specified by ```-R``` option.

{% highlight text %}

mirror -R local_directory remote_directory

{% endhighlight %}

The simplest form of this command is presented below:

{% highlight text %}

mirror -R

{% endhighlight %}

The above will mirror the current FTP directory to current local directory. If we need to change local directory from lftp prompt, we can use ```lcd``` command.


{% highlight text %}

lftp> lcd backup
lcd ok, local cwd=/home/backup

{% endhighlight %}

Obviously we can also use full paths in the ```mirror``` command, like presented above.


{% highlight text %}

mirror -R /var/backup/logs /var/log

{% endhighlight %}

The above will download the whole contents of ```/var/log``` folder from FTP server to local folder ```/var/backup/logs```.
