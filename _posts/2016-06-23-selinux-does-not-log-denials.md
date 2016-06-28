Security Enhanced Linux (SELinux) is a mandatory access control (MAC) mechanism implemented in kernel. SELinux allows to constrain access controls by specifying which users and applications can access which resources. It creates fine grained security, instead of specifying only who can read, write and execute the file, with SELinux we can specify who can append to file, move a file, unlink a file etc. SELinux depends on the policy loaded into the system.


However enhanced security comes at a price. Quite often we would have to create our own SELinux policy, because the default policy would not allow the actions our system needs to perform. Usually if some action is denied by SELinux in the system it will be logged into audit log (ON RHEL based systems it is ```/var/log/audit/audit.log```).

Unfortunately in certain situations the denials may not be logged. The reason for that is that applications and system libraries often probe for more access than is actually required. So the creators of default SELinux policy decided to implement the least privilege principal and in the same time prevent filling the audit log with useless messages. Technically it is implemented as the set of ```dontaudit``` rules. The problem with this approach is that sometimes ```dontaudit``` prevent us to troubleshoot the problem, because the message we are interested in is not being logged.

Below instructions help us to resolve that problem.

First to review all ```dontaudit``` configured in the default policy we can use the below command.

{% highlight text %}

sesearch --dontaudit | less

{% endhighlight %}


To check the ```dontaudit``` rules for certain program we can use ```grep``` tool, like presented in the below example for displaying ```dontaudit``` rules for ```logrotate```.

{% highlight text %}

sesearch --dontaudit | grep logrotate

{% endhighlight %}

To temporary disable the ```dontaudit``` rules, which would enable logging exceptions in the log.

{% highlight text %}

semodule -DB

{% endhighlight %}

The ```-D``` option disables all ```dontaudit``` rules.

The ```-B``` option rebuilds the SELinux policy.

After running above command, we should try to execute the problematic application again, and check the audit log. After the problem is resolved we should revert our SELinux policy to the default settings using the following command.


{% highlight text %}

semodule -B

{% endhighlight %}
