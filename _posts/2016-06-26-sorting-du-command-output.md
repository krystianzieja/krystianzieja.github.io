The ```du``` utility is used to estimate disk usage. One of the most commonly used options for this tool is ```-h```, which reports disk usage in human readable form (with alphabetical suffix K,M,G).


The problem rises when we want to sort output from the smallest to largest, because standard piping to ```sort``` won't work. We need to use ```sort -h``` the ```-h``` option for sort, will inform sort that we want to perform sort operation according to human readable numbers (K,M,G). The full command for checking the size of projects directory is:

{% highlight text %}

du -h projects/* | sort -h

{% endhighlight %}

If we want to reverse the order and display from largest to smallest we need to use ```-r``` option.

{% highlight text %}

du -h projects/* | sort -hr

{% endhighlight %}

Another useful option for the ```du``` command is ```-s```, that will display only a total disk usage for each argument. This will reports subdirectory sizes rather than sizes of individual files in each subdirectory. Compare below to examples. The first shows the size of each subdirectory under projects directory and sizes of files stored in the first level of projects directory tree.

{% highlight text %}

du -sh projects/* | sort -h

{% endhighlight %}

Second displays only the size of projects folder in total.

{% highlight text %}

du -sh projects | sort -h

{% endhighlight %}
