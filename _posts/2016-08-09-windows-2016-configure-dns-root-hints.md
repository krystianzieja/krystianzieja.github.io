Root Hints are alternative to forwarders way to resolve DNS queries.


To get a list of currently configured root hints in Windows DNS server we should use Get-DnsServerRootHint cmdlet.

```
PS C:\Users\Administrator> Get-DnsServerRootHint

NameServer          IPAddress
----------          ---------
A.ROOT-SERVERS.NET. {198.41.0.4, 2001:503:ba3e::2:30}
B.ROOT-SERVERS.NET. {192.228.79.201, 2001:500:84::b}
C.ROOT-SERVERS.NET. {192.33.4.12, 2001:500:2::c}
D.ROOT-SERVERS.NET. {199.7.91.13, 2001:500:2d::d}
E.ROOT-SERVERS.NET. 192.203.230.10
F.ROOT-SERVERS.NET. {192.5.5.241, 2001:500:2f::f}
G.ROOT-SERVERS.NET. 192.112.36.4
H.ROOT-SERVERS.NET. {198.97.190.53, 2001:500:1::53}
I.ROOT-SERVERS.NET. {192.36.148.17, 2001:7fe::53}
J.ROOT-SERVERS.NET. {192.58.128.30, 2001:503:c27::2:30}
K.ROOT-SERVERS.NET. {193.0.14.129, 2001:7fd::1}
L.ROOT-SERVERS.NET. {199.7.83.42, 2001:500:9f::42}
M.ROOT-SERVERS.NET. {202.12.27.33, 2001:dc3::35}

```

To add a new root hint server or add IPv4 / IPv6 address to existing root hint we use Add-DnsServerRootHint cmdlet.

```
PS C:\Users\Administrator> Add-DnsServerRootHint -NameServer G.ROOT-SERVERS.NET -IPAddress 2001:500:12::d0d
PS C:\Users\Administrator> Get-DnsServerRootHint

NameServer          IPAddress
----------          ---------
A.ROOT-SERVERS.NET. {198.41.0.4, 2001:503:ba3e::2:30}
B.ROOT-SERVERS.NET. {192.228.79.201, 2001:500:84::b}
C.ROOT-SERVERS.NET. {192.33.4.12, 2001:500:2::c}
D.ROOT-SERVERS.NET. {199.7.91.13, 2001:500:2d::d}
E.ROOT-SERVERS.NET. 192.203.230.10
F.ROOT-SERVERS.NET. {192.5.5.241, 2001:500:2f::f}
G.ROOT-SERVERS.NET. {192.112.36.4, 2001:500:12::d0d}
H.ROOT-SERVERS.NET. {198.97.190.53, 2001:500:1::53}
I.ROOT-SERVERS.NET. {192.36.148.17, 2001:7fe::53}
J.ROOT-SERVERS.NET. {192.58.128.30, 2001:503:c27::2:30}
K.ROOT-SERVERS.NET. {193.0.14.129, 2001:7fd::1}
L.ROOT-SERVERS.NET. {199.7.83.42, 2001:500:9f::42}
M.ROOT-SERVERS.NET. {202.12.27.33, 2001:dc3::35}

```
