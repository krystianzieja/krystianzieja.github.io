Simple Network Management Protocol (SNMP) enable network devices to exchange management information. It comes very handy if we want to monitor the status of our network devices. There are many open source systems that enable getting status information from routers, firewalls, switches and other network equipment, like for instance Cacti, OpenNMS, Zabix, Nagios.


Today we are going to configure SNMP server on Cisco ASA. It can be done via CLI or ASDM. Cisco ASA supports three versions of SNMP: version 1, version 2c and version 3. It is also capable to running all three versions of SNMP simultaneously. It is important to note that SNMP protocol allows information retrieval through a collection of GET commands and write configuration changes to a network device using SET command. However on Cisco ASA SET commands are not supported. Additionally we can configure Cisco ASA to send SNMP traps to management station, but that will be covered in later post.

The most secure version of SNMP is version 3, however not all network management servers are capable of working with SNMP v3, therefore we will configure SNMP in version 2c.

We start from enabling SNMP, it is enabled by default, but for a sake of completeness the needed command is provided below.

{% highlight text %}

asa(config)# snmp-server enable

{% endhighlight %}

Next we need to configure SNMP version 2c, using the below command:

{% highlight text %}

snmp-server host mgmt 192.168.0.254 poll community my-very-secret-shared-password version 2c

{% endhighlight %}

The ```snmp-server host``` commands enable us to configure the computer, which will have access to query Cisco ASA using SNMP. First we need to specify the interface on which the computer will communicate with Cisco ASA, in the example it is ```mgmt```. Then we provide the computer's IP address, in the example it is ```192.168.0.254```.

Next we have to configure if we want out management station to be enabled only to pooling by ```pool``` keyword or limit it to receive SNMP traps from Cisco ASA in that case we would use ```trap``` keyword. If we omit ```pool``` or ```trap``` keyword both polling and receiving traps will be enabled.

Then we need to configure community string, which follows ```community``` keyword, in the example above it is ```my-very-secret-shared-password```. The community string is a shared case sensitive password with maximum length of 32 characters. We must configure the same community string on our management station or we will not be able to authenticate on Cisco ASA.

The last keyword we are using is ```version```, which allows to specify SNMP version we want to use, in the example we specified ```version 2c```.

We can also configure SNMP location and contact information, which is pretty helpful when we are managing multiple devices, using the below commands.

{% highlight text %}

snmp-server location My Data Center
snmp-server contact John Doe

{% endhighlight %}

To verify our setup from management station we connect to Cisco ASA SNMP using ```snmpwalk``` command.

{% highlight text %}

snmpwalk -c my-very-secret-shared-password -v 2c 192.168.0.1

{% endhighlight %}

The ```-c``` option allows us to specify shared password (community string).

The ```-v``` option allows us to specify SNMP version, here we are using version 2c.

The IP address ```192.168.0.1``` is our Cisco ASA address on ```mgmt``` interface.
