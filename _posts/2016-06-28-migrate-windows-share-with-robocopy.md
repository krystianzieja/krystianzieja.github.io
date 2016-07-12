Moving Windows share between servers is a common task. One of the easiest way to migrate all of the from old server to new is by using ```robocopy``` tool.

Robocopy allows us to copy the files over the network but on top of that it gives us possibility to copy the permissions and mirror whole directory structure and many more. The most often used ```robocopy``` command I use to migrate files between servers is presented below.

{% highlight text %}

robocopy source target /MIR /SEC /SECFIX /r:5 /w:5

{% endhighlight %}

The ```source``` parameter is a network share from which we want to migrate the data.

The ```target``` parameter specifies a target folder to which the files will be copied. The target folder must exists prior to running this command. So for instance if we want to migrate the data from ```old-server\backup``` to ```C:\backup``` we have to create a folder called ```backup``` (i.e. ```C:\mkdir backup```) on new machine before running ```robocopy```.

The ```/SEC``` option specifies that files will be copied with security information.

The ```/SECFIX``` option specifies that security information will be fixed on all files even those that were skipped.

The ```/MIR```  option tells ```robocopy``` to mirror the whole directory tree from source to target.

The ```/r:X``` option specifies the X number of retries on failed copies. The default value of X is one million.

The ```/w:X``` option specifies the wait time between retries, in seconds. The default wait time is 30 seconds.

By using smaller ```/r:X``` and ```/w:X``` values then default settings, we prevent long running ```robocopy``` session that has problems caused by network communication.

The full command for copying network share ```\\old-server\backup``` to folder ```C:\backup``` located on machine from, which we run ```robocopy``` is presented below:

{% highlight text %}

robocopy \\old-server\backup C:\backup /MIR /SEC /SECFIX /r:5 /w:5

{% endhighlight %}
