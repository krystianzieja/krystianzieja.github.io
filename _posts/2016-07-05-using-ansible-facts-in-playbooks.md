Ansible gathers facts about about remote systems by default before executing tasks defined in the playbook. Obviously we can disable this functionality by setting ```gather_facts``` parameter to false.


However the default functionality comes very handy when we want to use information regarding our target machine in the playbook. To check what facts are available for particular machine we can use the following ansible command.

{% highlight text %}

ansible -m setup 192.168.100.10

{% endhighlight %}

Above command will return a bunch of useful information, which can be used in our playbooks.

Below playbook demonstrates how we can print target's hostname, and target's default IP v4 address.

**test-ansible-facts.yml**

{% highlight yml %}

---
- hosts: 192.168.100.10

  tasks:
  - name: debug
    debug: msg="hostname = {{ ansible_hostname }} ip address = {{ ansible_default_ipv4['address'] }}"

{% endhighlight %}


Next we run our playbook using ```ansible-playbook``` command.

{% highlight text %}

ansible-playbook test-ansible-facts.yml

{% endhighlight %}

And the return output is:

{% highlight text %}

TASK [debug] *******************************************************************
ok: [192.168.100.10] => {
    "msg": "hostname = dev03 ip address = 192.168.100.10"
}

{% endhighlight %}

The debug module prints statements during execution and can be useful for debugging variables or expressions. Many of you can say that printing ansible facts is not so useful and you would be correct. One of multiple practical applications would be to pass those variables for instance to our Jinja2 template.

In this example I will show you how you can define a virtual host for apache using the variable gathered by ansible.

We start from defining our templates let's call it ```template-test.conf```

{% highlight text %}

<VirtualHost {{ server_ip }}:80>
        ServerName {{ vhost_name }}
        DocumentRoot /var/www/vhosts/{{ vhost_name }}
        ErrorLog        /var/log/httpd/{{ vhost_name }}-errors
        CustomLog  /var/log/httpd/{{ vhost_name }}-access.log combined
</VirtualHost>

{% endhighlight %}

Next we will define our playbook, let's name it test-create-vhost.yml

{% highlight yml %}

---
- hosts: 192.168.100.10

  tasks:
  - name: "Create virtual host in Apache."
    template: dest=/etc/httpd/conf.d/{{ name }}.conf src=template-test.conf force=yes
    vars:
      vhost_name: "{{ ansible_hostname }}.{{ ansible_domain }}"
      server_ip: "{{ ansible_default_ipv4['address'] }}"

{% endhighlight %}


Out template uses two variables ```vhost_name``` and ```server_ip```. We set those variables in out playbook using a template module. The variable specification section starts with ```vars:``` keyword.
To set values for ```vhost_name``` variable we use two facts gathered by ansible ```ansible_hostname``` and ```ansible_domain```. To specify ```server_ip``` variable we use ```ansible_default_ipv4['address']```.

The last step is to run our playbook.

{% highlight text %}

ansible-playbook test-create-vhost.yml

{% endhighlight %}
