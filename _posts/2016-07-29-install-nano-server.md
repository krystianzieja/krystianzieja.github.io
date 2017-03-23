Nano Server was first released with  Windows Server 2016.
Installing Nano Server is pretty simple task, and resulting server is very small (the basic install is around 450MB)

First we need to get Nano Server Image Generator from Windows installation media, assuming that Windows installation DVD is in D:\ drive, we just need to use Copy-Item cmdlet.

```
Copy-Item -Recurse D:\NanoServer C:\
```

Next we need to import the proper PowerShell module to create Nano Server

```

cd C:\NanoServer
Import-Module .\NanoServerImageGenerator -Verbose

```

The next step is to create Nano Server Installation Media, by using New-NanoServerImage cmdlet.

New-NanoServerImage accepts following parameters

Parameter | description
-- | --
Edition | Datacenter or Standard
DeploymentType | Guest or Host. We select Guest when Nano will be virtual VM, Host when it will run on normal hardware
MediaPath | Path to Windows 2016 installation MediaPath
BasePath | Folder to which packages will be copied
TargetPath | Path where created VHD, VHDX or WIM will be stored
ComputerName | Name of created Nano Server

We start from creating two new folders one for storing packages to build Nano Server

```
New-Item -Type Directory -Path C:\NanoBuild
```

The second for storing resulting virtual hard drive

```
New-Item -Type Directory -Path C:\NanoVHD
```

Next we create our Nano server

```
New-NanoServerImage -Edition Datacenter -DeploymentType Guest -MediaPath D:\ -BasePath C:\NanoBuild -TargetPath C:\NanoVHD\nano01.vhdx
```

We will be asked to provide administrator's password and in few minutes nano server image will be created.

The last step is to create the new virtual machine with our new nano server image

```
New-VM -Name nano01 -BootDevice VHD -VHDPath C:\NanoVHD\nano01.vhdx
```
