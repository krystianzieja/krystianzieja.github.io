Storage pools enable us to group physical disks together for more efficient way to use capacity. In some cases storage pools can also improve performance.

To create a storage pool we need to have some unused hard drives in the server.

First we need to identify disks that can be added to our server pool

```
PS C:\Users\Administrator> Get-PhysicalDisk -CanPool $true

FriendlyName  SerialNumber        CanPool OperationalStatus HealthStatus Usage        Size
------------  ------------        ------- ----------------- ------------ -----        ----
VBOX HARDDISK VBaddc525a-ad9f49c5 True    OK                Healthy      Auto-Select 20 GB
VBOX HARDDISK VB5645a596-4c199bae True    OK                Healthy      Auto-Select 20 GB

```

Next we have to identify Storage Subsystem, by using Get-StorageSubsystem cmdlet

```
PS C:\Users\Administrator> Get-StorageSubSystem

FriendlyName                       HealthStatus OperationalStatus
------------                       ------------ -----------------
Windows Storage on WIN-5HL01DCA13D Healthy      OK

```

Now when we have all the required details we create a small PS script to create our Storage Pool.

```
PS C:\Users\Administrator> $subsystem = Get-StorageSubSystem | select -ExpandProperty FriendlyName
PS C:\Users\Administrator> $disks = Get-PhysicalDisk -CanPool $true
PS C:\Users\Administrator> New-StoragePool -FriendlyName "Pool01" -StorageSubSystemFriendlyName $subsystem -PhysicalDisks $disks

FriendlyName OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------ ----------------- ------------ ------------ ----------
Pool01       OK                Healthy      False        False
```

After creating the storage pool we have to create a virtual disk on that pool. Virtual disks enable us to create a resilient storage. There are three types of resiliency layouts:

**Simple**

Data is stripped between physical disks. It maximize the capacity however single disk failure results in data loss.

**Mirror**

Data is stripped across physical disks, creating 2 or three copies of data. Such configuration can whitstand a single or multiple disk failures, however the biggest drawback is capacity, because additional drives are used for redundancy.

To protect against single disk failure, we have to use at least two physical disks.

To protect against two disk failures, we have to use at least five physical disks.

**Parity**

Data and parity bit are stripped across physical disks. Storage capacity is better than with Mirror. We can write one or two copies of parity information.


To protect against single disk failure, we have to use at least three physical disks.

To protect against two disk failures, we have to use at least seven physical disks.

There are two options for Virtual Disks Provisioning:

Thin - storage is allocated only when data is being written.

Thick - storage is allocated immediately from the pool. With this model we cannot oversubscribe storage.

To create a virtual disk we use the following command

```
PS C:\Users\Administrator> New-VirtualDisk -StoragePoolFriendlyName Pool01 -ProvisioningType Thin -ResiliencySettingName
 Mirror -Size 19GB -FriendlyName VDisk01

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach  Size
------------ --------------------- ----------------- ------------ --------------  ----
VDisk01      Mirror                OK                Healthy      False          19 GB
```

Instead of creating Virtual Disk we can create a volume (a volume is how the disk presents itself in operating system).

Next we create a volume (a volume is how the disk presents itself in operating system).

```
PS C:\Users\Administrator> get-disk

Number Friendly Name Serial Number                    HealthStatus         OperationalStatus      Total Size Partition
                                                                                                             Style
------ ------------- -------------                    ------------         -----------------      ---------- ---------
0      VBOX HARDDISK VBc0c66778-fa603b61              Healthy              Online                     128 GB MBR
3      VDisk01       {32269b7d-3474-4360-9c41-6367... Healthy              Online                      19 GB GPT


PS C:\Users\Administrator> New-Volume -DiskNumber 3 -FriendlyName DATA01 -FileSystem NTFS -DriveLetter F

DriveLetter FileSystemLabel FileSystem DriveType HealthStatus OperationalStatus SizeRemaining     Size
----------- --------------- ---------- --------- ------------ ----------------- -------------     ----
F           DATA01          NTFS       Fixed     Healthy      OK                     18.81 GB 18.87 GB

```
