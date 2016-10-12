If we want to install exactly the same packages on new server as we had installed on old servers, but we do not have a documentation and we are not using tools like ansible, chef or puppet, ```dpkg``` command can be helpful.


To backup our current packages selection we issue below command:

{% highlight text %}

sudo dpkg --get-selections > ubuntu-packages.txt

{% endhighlight %}

Then we copy this file to new machine and restore the package selection with the following command:

{% highlight text %}

dpkg --set-selections < ubuntu-packages.txt

{% endhighlight %}
