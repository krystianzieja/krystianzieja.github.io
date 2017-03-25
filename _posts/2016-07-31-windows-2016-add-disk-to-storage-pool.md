Expanding storage pools in Windows Server is very easy task.

We select a disk that we want to add to the pool with Get-PhysicalDisk cmdlet and then
execute Add-PhysicalDisk cmdlet.

```

PS C:\Users\Administrator> Get-PhysicalDisk

FriendlyName  SerialNumber        CanPool OperationalStatus HealthStatus Usage         Size
------------  ------------        ------- ----------------- ------------ -----         ----
VBOX HARDDISK VBaddc525a-ad9f49c5 False   OK                Healthy      Auto-Select  20 GB
VBOX HARDDISK VBc0c66778-fa603b61 False   OK                Healthy      Auto-Select 128 GB
VBOX HARDDISK VB5645a596-4c199bae False   OK                Healthy      Auto-Select  20 GB
VBOX HARDDISK VBd153ed15-f771c9d1 True    OK                Healthy      Auto-Select  20 GB


PS C:\Users\Administrator> Get-PhysicalDisk -CanPool $true

FriendlyName  SerialNumber        CanPool OperationalStatus HealthStatus Usage        Size
------------  ------------        ------- ----------------- ------------ -----        ----
VBOX HARDDISK VBd153ed15-f771c9d1 True    OK                Healthy      Auto-Select 20 GB


PS C:\Users\Administrator> $disk = Get-PhysicalDisk -CanPool $true
PS C:\Users\Administrator> Get-StoragePool

FriendlyName OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------ ----------------- ------------ ------------ ----------
Pool01       OK                Healthy      False        False
Primordial   OK                Healthy      True         False


PS C:\Users\Administrator> Add-PhysicalDisk -StoragePoolFriendlyName Pool01 -PhysicalDisks $disk

```
