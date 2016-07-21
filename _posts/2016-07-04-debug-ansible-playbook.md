Ansible is a great tool for automation common tasks in IT infrastructure like configuration management, provisioning and application deployment. The standard way of using ansible is by writing and running ansible playbooks, which are ```yaml``` files with description telling ansible what to do.


Such approach is great unless we do something wrong in playbook specification, the old truth says "mistakes happen". The easiest form of debugging ansible playbook is to provide ```-vvv``` option, which will tell ansible to run in verbose mode. Actually the number of ```v``` characters specify how much verbose information would be printed out. Where ```-v``` gives us the least verbose output, and ```-vvvv``` output includes connection debugging. From my experience ```-vvv``` is the most useful mode.

Below there is an example of running ansible playbook with verbose output.

{% highlight text %}

ansible-playbook -vvv install-vim.yml

{% endhighlight text %}
