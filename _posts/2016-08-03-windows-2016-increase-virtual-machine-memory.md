Increasing and decreasing virtual machine memory running on Hyper-V server, can be accomplished by running ``` Set-VMMemory ``` cmdlet.

Below example assigns 4 GB of static memory to virtual machine named nano01
```
Set-VM -Name nano01 -MemoryStartupBytes 4GB
```

To configuyre dynamic memory ranging from 2GB to 4GB use the following example.

```
PS C:\Users\Administrator> Set-VM -Name nano01 -DynamicMemory -MemoryMinimumBytes 2GB -MemoryMaximumBytes 4GB -MemorySta
rtupBytes 2GB

```
