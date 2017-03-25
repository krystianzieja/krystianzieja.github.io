Remote administration of Hyper-V host that is a part of a domain requires no special configuration, you just need appropriate permissions (for instance being a part of Hyper-V Administrators group, or specific permissions delegated to your user account). When Hyper-V server is a part of workgroup things are slightly more complicated.

Below procedure presents what steps must be performed to enable remote management of Hyper-V host in workgroup.

First we ensure that our network is identified as Private.

```
PS C:\Users\Administrator> Get-NetConnectionProfile


Name             : Network
InterfaceAlias   : Ethernet
InterfaceIndex   : 3
NetworkCategory  : Public
IPv4Connectivity : Internet
IPv6Connectivity : NoTraffic



PS C:\Users\Administrator> Set-NetConnectionProfile -InterfaceIndex 3 -NetworkCategory Private
PS C:\Users\Administrator> Get-NetConnectionProfile


Name             : Network
InterfaceAlias   : Ethernet
InterfaceIndex   : 3
NetworkCategory  : Private
IPv4Connectivity : Internet
IPv6Connectivity : NoTraffic
```

Next we enable PowerShell Remoting.

```
PS C:\Users\Administrator> Enable-PSRemoting
```

Then we have to enable WSMan Credential Role as the server.

```
PS C:\Users\Administrator> Enable-WSManCredSSP -Role Server

CredSSP Authentication Configuration for WS-Management
CredSSP authentication allows the server to accept user credentials from a remote computer. If you enable CredSSP
authentication on the server, the server will have access to the user name and password of the client computer if t
client computer sends them. For more information, see the Enable-WSManCredSSP Help topic.
Do you want to enable CredSSP authentication?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


cfg               : http://schemas.microsoft.com/wbem/wsman/1/config/service/auth
lang              : en-US
Basic             : false
Kerberos          : true
Negotiate         : true
Certificate       : false
CredSSP           : true
CbtHardeningLevel : Relaxed
```

On the workstation from which we will manage our Hyper-V server we need to perform the following actions.

First we need to specify that we trust our Hyper-V server.

```
PS C:\Users\Administrator> Set-Item "WSMAN:\localhost\Client\TrustedHosts" -Value hyperv01

WinRM Security Configuration.
This command modifies the TrustedHosts list for the WinRM client. The computers in the TrustedHosts list might not be
authenticated. The client might send credential information to these computers. Are you sure that you want to modify
this list?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

On the client we also have to enable WSMAN Credential Role as client.

```
PS C:\Users\Administrator> Enable-WSManCredSSP -Role Client -DelegateComputer hyperv01

CredSSP Authentication Configuration for WS-Management
CredSSP authentication allows the user credentials on this computer to be sent to a remote computer. If you use CredSSP
 authentication for a connection to a malicious or compromised computer, that computer will have access to your user
name and password. For more information, see the Enable-WSManCredSSP Help topic.
Do you want to enable CredSSP authentication?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


cfg         : http://schemas.microsoft.com/wbem/wsman/1/config/client/auth
lang        : en-US
Basic       : true
Digest      : true
Kerberos    : true
Negotiate   : true
Certificate : true
CredSSP     : true

```

The last step on our workstation is to adjust the local computer policy. The setting we have to modify is located under Computer Configuration\Administrative Templates\System\Credential Delegation.

We need to modify the following setting: "Allow delegating fresh credentials with NTLM-only server authentication"

In this setting we need to set "wsman\hostname", where hostname is the name of our Hyper-V server that we want to manage.

![Local Computer Configuration]({{ site.url }}/images/w2016-remote-hyperv-management-01.png)
