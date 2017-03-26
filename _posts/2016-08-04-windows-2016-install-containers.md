Containers are a new feature of Windows Server 2016. They can be installed on both physical server and virtual machine.

To install Containers we just need to install Windows Feature.

```
PS C:\Users\Administrator> Install-WindowsFeature Containers

Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    Yes            SuccessRest... {Containers}
WARNING: You must restart this server to finish the installation process.


PS C:\Users\Administrator> Restart-Computer
```

On Nano server the installation is performed by Package Provider.

```

Install-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package

```

To manage containers on Windows Server 2016 or Nano Server we have to install Docker service.

First we install the Docker-Microsoft PackageManagement Provider.

```

PS C:\Users\Administrator> Install-Module -Name DockerMsftProvider -Repository PSGallery -Force

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\Administrator\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by
 running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install
and import the NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

```

Next we install docker package.

```

PS C:\Users\Administrator> Install-Package -Name docker -ProviderName DockerMsftProvider

The package(s) come(s) from a package source that is not marked as trusted.
Are you sure you want to install software from 'DockerDefault'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A

Name                           Version          Source           Summary
----                           -------          ------           -------
Docker                         17.03.0-ee       DockerDefault    Contains Docker EE for use with Windows Server 2016...

```

The last step of the installation is a reboot.

```
Restart-Computer
```

After computer boots up, we need to verify the status of Docker service.

```

PS C:\Users\Administrator> get-service -name Docker

Status   Name               DisplayName
------   ----               -----------
Running  Docker             Docker

```

To display basic information about we use ```docker info``` command as shown below.

```
PS C:\Users\Administrator>  docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.03.0-ee-1
Storage Driver: windowsfilter
 Windows:
Logging Driver: json-file
Plugins:
 Volume: local
 Network: l2bridge l2tunnel nat null overlay transparent
Swarm: inactive
Default Isolation: process
Kernel Version: 10.0 14393 (14393.693.amd64fre.rs1_release.161220-1747)
Operating System: Windows Server 2016 Datacenter Evaluation
OSType: windows
Architecture: x86_64
CPUs: 1
Total Memory: 2 GiB
Name: WIN-5HL01DCA13D
ID: 6R27:5WXC:X6P3:3QX4:AL65:BL3G:QXC2:XMN6:5VW5:CTH4:JVKI:XCNI
Docker Root Dir: C:\ProgramData\docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```

The best way to configure docker service is by editing ```daemon.json``` file located in ```C:\ProgramData\docker\network```. If the file does not exist we can create it. In the configuration file we need only to set values that are different from default values.

**CAUTION!!! Not all docker configuration options are available in Windows 2016.**
