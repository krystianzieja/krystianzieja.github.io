Holland Backup is very neat software that simplifies MySQL backups and can also work with other types of backups.


If you are working on newer ubuntu than 14.10 you have to perform installation from source. We start from downloading the newest version.

{% highlight text %}

wget https://github.com/holland-backup/holland/archive/master.zip

{% endhighlight %}

and extract it

{% highlight text %}

unzip master.zip

{% endhighlight %}

To install holland backup we would need python setuptools package and python 2.7 package. If we want to backup MySQL we also need to install python MySQL interface.

{% highlight text %}

apt-get install python2.7-minimal python-setuptools python-mysqldb

{% endhighlight %}

Next we install Holland Backup by using following command.

{% highlight text %}

cd holland-master
python setup.py install

{% endhighlight %}

Assuming that we want to use ```mysqldump``` plugin for holland we also need to install it.

{% highlight text %}

cd plugins/holland.backup.mysqldump/

python setup.py install

cd ../holland.lib.mysql

python setup.py install

cd ../holland.lib.common/

python setup.py install


{% endhighlight %}

Next we need to copy the configuration files to ```/etc```.

{% highlight text %}

mkdir -p /etc/holland
cp -r config/* /etc/holland/

{% endhighlight %}

Assuming that we will be using mysqldump provider to backup our MySQL database, we can adjust its configuration to suit our needs, the file we need to modify is ```/etc/holland/providers/mysqldump.conf```.

{% highlight text %}

file-per-database   = yes

level               = 3

{% endhighlight %}

The ```file-per-database``` setting means that holland will create a separate backup file for every database.

The ```level``` parameter allows us to specify the compression level, where 1 is the fastest and 9 is the slowest.

We can also modify the backupset settings which are defined in ```/etc/holland/backupsets/default.conf```. For instance we can change estimated-size-factor from 1.0 to 0.6.


{% highlight text %}

estimated-size-factor = 0.6

{% endhighlight %}

We also need to prepare the folder for holland logs.

{% highlight text %}

mkdir -p /var/log/holland/

{% endhighlight %}

On top of making backups of MySQL databases with ```mysqldump```, holland backup can be configured to make a tar archive of specified directory, which makes it a perfect tool for backing up simple web servers. To use that functionality we have to install tar plugin.

{% highlight text %}

cd ../holland.backup.tar

python setup.py install

{% endhighlight %}

Next we need to configure the tar provider in file ```/etc/holland/providers/tar.conf```

{% highlight text %}

[tar]
directory = /var/www/vhosts

[compression]
method     = gzip
inline     = yes
level      = 3

{% endhighlight %}

Then we create a new backupset for tar provider; we create a file ```/etc/holland/backupsets/website.conf``` with following contents.

{% highlight text %}

[holland:backup]
plugin = tar
backups-to-keep = 1
auto-purge-failures = yes
purge-policy = after-backup

{% endhighlight %}

We can verify our setup by running hollack backup from command line.

{% highlight text %}

holland bk

{% endhighlight %}


If everything works correctly the last step would be to create a cron job. We append below line to ```/etc/crontab```.


{% highlight text %}

0  2    * * *   root	holland bk

{% endhighlight %}
