RSS can enable VM to support additional network traffic loads by distributting traffic accross multiple processor cores.  To enable RSS on Hyper-V server processor must support this feature and VM must have multiple cores assigned.

To verify the current setting we use again Get-NetAdapterRss cmdlet.

```
PS C:\Users\Administrator> Get-NetAdapterRss -Name Team01


Name                                            : Team01
InterfaceDescription                            : Microsoft Network Adapter Multiplexor Driver
Enabled                                         : False
NumberOfReceiveQueues                           : 0
Profile                                         : NUMAStatic
BaseProcessor: [Group:Number]                   : 0:0
MaxProcessor: [Group:Number]                    : 0:0
MaxProcessors                                   : 1
RssProcessorArray: [Group:Number/NUMA Distance] : 0:0/0
IndirectionTable: [Group:Number]                :


```

To enable Receive Side Scaling we execute Set-NetAdapterRss cmdlet.

```
PS C:\Users\Administrator> Set-NetAdapterRss -Name Team01 -Enabled $true
PS C:\Users\Administrator> Get-NetAdapterRss -Name Team01


Name                                            : Team01
InterfaceDescription                            : Microsoft Network Adapter Multiplexor Driver
Enabled                                         : True
NumberOfReceiveQueues                           : 0
Profile                                         : NUMAStatic
BaseProcessor: [Group:Number]                   : 0:0
MaxProcessor: [Group:Number]                    : 0:0
MaxProcessors                                   : 1
RssProcessorArray: [Group:Number/NUMA Distance] : 0:0/0
IndirectionTable: [Group:Number]                :

```
