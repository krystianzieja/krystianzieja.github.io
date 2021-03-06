In Windows Network Location is assigned to each network interface automatically by the operating system. Sometimes however it becomes pretty annoying, when Windows decides that your local connection would be assigned to Public network category and as the end result we cannot RDP to the server because by default Remote Desktop is disabled on Public Networks.


We can easily change this setting using PowerShell. First we need to check the network categories to which our network interface is assigned, that can be done using ```Get-NetConnectionProfile``` cmdlet.

{% highlight powershell %}

Get-NetConnectionProfile


Name             : Unidentified network
InterfaceAlias   : DRAC
InterfaceIndex   : 21
NetworkCategory  : Public
IPv4Connectivity : LocalNetwork
IPv6Connectivity : LocalNetwork

Name             : Network
InterfaceAlias   : LAN
InterfaceIndex   : 19
NetworkCategory  : Public
IPv4Connectivity : Internet
IPv6Connectivity : LocalNetwork
 
{% endhighlight %}

To change the network profile assignment from Public to Private we should use ```Set-NetConnectionProfile``` cmdlet.

{% highlight powershell %}

Set-NetConnectionProfile -InterfaceAlias LAN -NetworkCategory Private

{% endhighlight %}

Next we just need to verify.

{% highlight powershell %}

Get-NetConnectionProfile


Name             : Unidentified network
InterfaceAlias   : DRAC
InterfaceIndex   : 21
NetworkCategory  : Public
IPv4Connectivity : LocalNetwork
IPv6Connectivity : LocalNetwork

Name             : Network
InterfaceAlias   : LAN
InterfaceIndex   : 19
NetworkCategory  : Private
IPv4Connectivity : Internet
IPv6Connectivity : LocalNetwork
 
{% endhighlight %}

Except from Private and Public network category there is also a Domain category, however this cannot be changed by using PowerShell. Domain category will be used when a computer is connected to Active Directory domain.
