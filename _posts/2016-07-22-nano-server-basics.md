Nano Server is the new installation option available in Windows Server 2016.
Nano Server is a remotely administered server operating system optimized for private clouds and datacenters. It is much smaller than Server Core, however it has some limitations when compared to both Core and Desktop Experience installation options.

Below list presents those limitations:

- Nano Server is "headless;" there is no local logon capability or graphical user interface.
- Only 64-bit applications, tools, and agents are supported.
- Nano Server cannot serve as an Active Directory domain controller.
- Group Policy is not supported. However, you can use Desired State Configuration to apply settings at scale.
- Nano Server cannot be configured to use a proxy server to access the internet.
- NIC Teaming (specifically, load balancing and failover, or LBFO) is not supported. Switch-embedded teaming (SET) is supported instead (details about Switch-embedded teaming are provided below).
- System Center Configuration Manager and System Center Data Protection Manager are not supported.
- Best Practices Analyzer (BPA) cmdlets and BPA integration with Server Manager are not supported.
- Nano Server does not support virtual host bus adapters (HBAs).
- Nano Server does not need to be activated with a product key. When functioning as a Hyper-V host, Nano Server does not support Automatic Virtual Machine Activation (AVMA). Virtual machines running on a Nano Server host can be activated using Key Management Service (KMS) with a generic volume license key or using Active Directory-based activation.
The version of Windows PowerShell provided with Nano Server has important differences. You can find a list of unsupported PowerShell features in [Microsoft documentation](https://technet.microsoft.com/en-us/windows-server-docs/get-started/powershell-on-nano-server)
- Nano Server is supported only on the Current Branch for Business (CBB) model--there is no Long-Term Servicing Branch (LTSB) release for Nano Server at this time. To maintain support, administrators must stay no more than two CBB releases behind. However, these releases do not auto-update existing deployments; administrators perform manual installation of a new CBB release at their convenience.

## Other important things to note

There are no upgrade or migration paths to Nano Server.

Installation of Nano Server is performed by VHD Configuration.

## SET Teaming

Traditionally teaming native to Windows has been done through LBFO (Load Balancer Fail Over) where multiple NICs are grouped together into a team (up to 32 NICs which can be of different types) and that resultant teamed adapter is used by a Hyper-V switch (or other uses). Traditional LBFO teaming is not compatible with RDMA nor SDNv2 and the Virtual Filtering Platform (VFP) that powers SDNv2.

Switch Embedded Teaming (SET) is a new technology in Windows Server 2016 which allows multiple network adapters to be joined together within the vSwitch itself without utilizing LBFO teaming. SET enables up to 8 adapters to be joined together but they must all be identical in terms of make, model, driver and firmware. The idea behind SET is that these will be 10 Gbps or bigger NICs and so 8 should be more than enough for anyone.

A huge benefit of SET over LBFO is that SET enables the NICs to run as converged which means the NICs support regular IP traffic in addition to RDMA. Prior to Windows Server 2016 it is necessary to have two separate sets of NICs. One set to use with vSwitch and another set to use with RDMA. Now the same NICs can be used with both.
