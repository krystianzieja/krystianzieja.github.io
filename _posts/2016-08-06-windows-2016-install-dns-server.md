Installing DNS Server on Windows Server 2016 is a simple as enabling Windows feature.

```
PS C:\Users\Administrator> Install-WindowsFeature DNS

Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    No             Success        {DNS Server}
```

On Nano Server we would use Install-NanoServerPackage cmdlet.

```
Install-NanoServerPackage -Package Microsoft-NanoServer-DNS-Package
```

To add forwarders to DNS Server we use Add-DnsServerForwarder cmdlet. Below example demonstrates how to add forwarder to OpenDNS.

```
PS C:\Users\Administrator> Add-DnsServerForwarder 208.67.222.222

```

To verify we use Get-DnsServerForwarder.

```
PS C:\Users\Administrator> Get-DnsServerForwarder
UseRootHint        : True
Timeout(s)         : 3
EnableReordering   : True
IPAddress          : 208.67.222.222
ReorderedIPAddress : 208.67.222.222

```

To setup a conditional forwarded for mydomain.com, so that all queries related to mydomain.com are forwarded to DNS server with IP 10.200.200.20, we use Add-DnsServerConditionalForwarderZone cmdlet.

```
PS C:\Users\Administrator> Add-DnsServerConditionalForwarderZone -Name mydomain.com -MasterServers 10.200.200.20

```
