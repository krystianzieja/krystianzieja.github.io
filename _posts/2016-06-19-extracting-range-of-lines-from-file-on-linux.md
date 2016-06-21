It is very common requirement to extract only a range of lines from text file, for instance when we want to get only one table from MySQL dump. On Linux systems sed comes to our aid.


Sed is a stream editor, which is capable to parse and transform text using simple programming language. Sed is a line oriented text processing utility.

To extract specified range of lines from a text file and store them in a new file, we can use the following command.

{% highlight text %}

sed -n 'start-line-number,end-line-numberp' big-file.log > result.log

{% endhighlight %}

The example full command with line numbers would look like that.

{% highlight text %}

sed -n '24300,26800p' big-file.log > result.log

{% endhighlight %}

The ```-n``` option disable automatic printing, and ```sed``` will only produce output when explicitly told to via the ```p``` command.

The ```start-line-number,end-line-number``` specifies an address range described by two numbers separated by a comma (','). An address range matches lines starting from where the first address matches, and continues until the second address matches (inclusively).

The ```p``` option forces ```sed``` to print pattern space to standard output.

The ```big-file.log``` represents the file we want to process with ```sed```.

The ```>``` is standard output redirection.

The ```result.log``` represents the file that will be created as a result of ```sed``` operation.


## Improve performance

We should remember about one small thing, that could improve performance. Let's do a simple example. We start with a MySQL dump named ```mobi.sql```, which has 18647 lines.

{% highlight text %}

wc -l mobi.sql
18647 mobi.sql

{% endhighlight %}

Let's try to extract lines between 9000 and 10000 to a file named result.sql. To measure performance we will use ```time``` utility. On many Linux systems ```time``` utility can be easily confused with ```time``` bash built-in.

To install ```time``` use one of below commands.

For ```yum``` package manager:

{% highlight text %}

yum install time

{% endhighlight %}

For ```apt``` package manager:

{% highlight text %}

apt-get install time

{% endhighlight %}

Verify that time utility was installed:

{% highlight text %}

which time
/usr/bin/time

{% endhighlight %}

Please be sure to execute the right ```time``` command, as it is illustrated below it is easy to make a mmistake.

Incorrect version of ```time```

{% highlight text %}

time -v sleep 1
-bash: -v: command not found

real	0m0.001s
user	0m0.000s
sys	0m0.002s


{% endhighlight %}

Correct version of ```time```

{% highlight text %}

/usr/bin/time -v sleep 1
	Command being timed: "sleep 1"
	User time (seconds): 0.00
	System time (seconds): 0.00
	Percent of CPU this job got: 0%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:01.00
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 660
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 1
	Minor (reclaiming a frame) page faults: 207
	Voluntary context switches: 3
	Involuntary context switches: 1
	Swaps: 0
	File system inputs: 40
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0


{% endhighlight %}


{% highlight text %}

/usr/bin/time -v "sed -n '9000,10000p' mobi.sql > result.sql"

{% endhighlight %}

Result:

{% highlight text %}

	Command being timed: "sed -n 9000,10000p mobi.sql"
	User time (seconds): 6.33
	System time (seconds): 14.96
	Percent of CPU this job got: 70%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:30.28
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 3016
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 0
	Minor (reclaiming a frame) page faults: 804
	Voluntary context switches: 4190
	Involuntary context switches: 119
	Swaps: 0
	File system inputs: 36872560
	File system outputs: 2027072
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0

{% endhighlight %}

The small trick we can use is to stop ```sed``` from processing the file, just after we reached the last line we want to extract. We can do it using ```q``` command after line number, which means that after reaching that line number (in the example it is line number 10001) ```sed``` will stop processing the input, and exit without processing any more commands.

{% highlight text %}

/usr/bin/time -v sed -n '9000,10000p;10001q' mobi.sql > result.sql

{% endhighlight %}


Results:

{% highlight text %}

	Command being timed: "sed -n 9000,10000p;10001q mobi.sql"
	User time (seconds): 2.88
	System time (seconds): 7.01
	Percent of CPU this job got: 49%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:19.81
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 3016
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 1
	Minor (reclaiming a frame) page faults: 803
	Voluntary context switches: 4871
	Involuntary context switches: 43
	Swaps: 0
	File system inputs: 19472072
	File system outputs: 2027072
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0


{% endhighlight %}


Changing the ```sed``` command allowed us to save around 11 seconds of wall clock (0:30.28 vs 0:19.81) and also File system inputs and outputs decreased. The reason for it is that ```sed``` command without ```q``` will process the ```mobi.sql``` till the end of the file.
