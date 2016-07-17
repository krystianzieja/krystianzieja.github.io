The ability to join multiple lines in bash scripting comes often pretty handy. In Linux joining lines can be accomplished by using ```paste``` command.


The ```paste``` command can both work on files or process the lines from standard input. Let's start from creating a simple file called ```lines.txt``` with the following content.

{% highlight text %}

line 1
line 2
line 3

{% endhighlight %}

Next we use ```paste -s``` command to merge 3 lines into one.

{% highlight text %}

paste -s lines.txt

line 1	line 2	line 3

{% endhighlight %}

By default ```paste``` command will use tab as a delimiter for joining lines, however we can always change it by using ```-d``` option, which allows us to specify the delimiter. In below command we use space as a delimiter.

{% highlight text %}

paste -s -d ' ' lines.txt

line 1 line 2 line 3

{% endhighlight %}

The last example shows how we can pipe the output of ```dpkg -l``` command to ```grep``` command then to ```awk``` command and at the end to ```paste``` command. It is a practical example how to generate the list of packages for removal by ```apt-get purge``` when uninstalling LibreOffice.

{% highlight text %}

dpkg -l | grep -i libreoffice | awk '{ print $2 }' | paste -s -d ' '

hyphen-en-us libreoffice libreoffice-avmedia-backend-gstreamer libreoffice-base libreoffice-base-core libreoffice-base-drivers libreoffice-calc libreoffice-common libreoffice-core libreoffice-draw libreoffice-impress libreoffice-java-common libreoffice-math libreoffice-report-builder-bin libreoffice-sdbc-firebird libreoffice-sdbc-hsqldb libreoffice-style-galaxy libreoffice-writer mythes-en-us uno-libs3 ure

{% endhighlight %}
