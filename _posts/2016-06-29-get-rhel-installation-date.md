Sometimes we would like to verify when particular machine was installed. On RHEL based Linux systems there are couple ways of doing that. However my preferred way is to check installation date of basesystem package.


The ```basesystem``` package is the first installed rpm on RHEL based Linuxes (RHEL, Centos, Scientific Linux). Therefore to determine the installation date of the system we should check the installation date of this package.

{% highlight text %}

rpm -qi basesystem

Name        : basesystem
Version     : 10.0
Release     : 7.el7
Architecture: noarch
Install Date: Thu 10 Mar 2016 05:25:30 PM CST
Group       : System Environment/Base
Size        : 0
License     : Public Domain
Signature   : RSA/SHA256, Tue 01 Apr 2014 08:23:16 AM CDT, Key ID 199e2f91fd431d51
Source RPM  : basesystem-10.0-7.el7.src.rpm
Build Date  : Fri 27 Dec 2013 11:22:15 AM CST
Build Host  : ppc-015.build.eng.bos.redhat.com
Relocations : (not relocatable)
Packager    : Red Hat, Inc. <http://bugzilla.redhat.com/bugzilla>
Vendor      : Red Hat, Inc.
Summary     : The skeleton package which defines a simple Red Hat Enterprise Linux system
Description :
Basesystem defines the components of a basic Red Hat Enterprise Linux
system (for example, the package installation order to use during
bootstrapping). Basesystem should be in every installation of a system,
and it should never be removed.

{% endhighlight %}

The option ```-q``` tells ```rpm``` package manger to run a query, and ```-i``` displays an info about specific package.

The above command will work on both RHEL 6.x and RHEL 7.x versions.

Another available option in case we have ext3 or ext4 filesystem, is ```tune2fs``` tool that allows us tp check when the filesystem was created. Below command assumes that ```/dev/sda3``` holds ```/root```.

{% highlight text %}

tune2fs -l /dev/sda3 | grep created
Filesystem created:       Sun Mar 18 12:07:24 2012

{% endhighlight %}

One more option would be to check the modification time of ```/root/anaconda-ks.cfg``` file. That file is created by RHEL installer.

{% highlight text %}

stat /root/anaconda-ks.cfg

  File: ‘/root/anaconda-ks.cfg’
  Size: 1866      	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 538011144   Links: 1
Access: (0600/-rw-------)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:admin_home_t:s0
Access: 2016-03-10 17:28:32.042181324 -0600
Modify: 2016-03-10 17:28:32.043181322 -0600
Change: 2016-03-10 17:28:32.043181322 -0600

{% endhighlight %}

Obviously all above methods won't work if we cloned the disk (for instance cloning virtual machines), because all the dates would be set as they were in original copy.
