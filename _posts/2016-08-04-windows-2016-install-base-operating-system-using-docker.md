Before deploying containers we need to download the base system. Microsoft provides base systems for both Server Core and Server Nano.

The first step is to search for base image we want to use.

```
PS C:\Users\Administrator> docker search microsoft
NAME                                     DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
microsoft/aspnet                         ASP.NET is an open source server-side Web ...   575                  [OK]
microsoft/dotnet                         Official images for .NET Core for Linux an...   478                  [OK]
mono                                     Mono is an open source implementation of M...   224       [OK]
microsoft/mssql-server-linux             Official images for Microsoft SQL Server o...   203
microsoft/nanoserver                     Windows Server 2016 Nano Server base OS im...   150
microsoft/windowsservercore              Windows Server 2016 Server Core base OS im...   146
microsoft/aspnetcore                     Official images for running compiled ASP.N...   119                  [OK]
microsoft/iis                            Internet Information Services (IIS) instal...   103
microsoft/azure-cli                      Docker image for Microsoft Azure Command L...   81                   [OK]
microsoft/mssql-server-windows-express   Official Microsoft SQL Server Express Edit...   63
microsoft/mssql-server-windows           Official images for Microsoft SQL Server f...   37
microsoft/aspnetcore-build               Official images for building ASP.NET Core ...   31                   [OK]
microsoft/powershell                     Official PowerShell Core releases from htt...   30                   [OK]
microsoft/oms                            Monitor your containers using the Operatio...   19                   [OK]
microsoft/dotnet-samples                 .NET Core Docker Samples                        16                   [OK]
microsoft/cntk                           CNTK images from github.com/Microsoft/CNTK...   7                    [OK]
microsoft/powershell-nightly             Nightly builds of PowerShell Core for CI        6                    [OK]
microsoft/applicationinsights            Application Insights for Docker helps you ...   5                    [OK]
microsoft/dotnet-nightly                 Preview bits of the .NET Core CLI               4                    [OK]
berlius/microsoft-malmo                  Microsoft-malmo - artificial intelligence ...   1
microsoft/aspnetcore-build-nightly       Images to build preview versions of ASP.NE...   1                    [OK]
renerchen/microsoft                                                                      0
microsoft/iot-gateway-opc-ua-proxy       Azure IoT Gateway with OPC UA Proxy module      0                    [OK]
dreher/microsoft                         Microsoft Test Repo                             0
microsoft/iot-gateway-opc-ua             Azure IoT Gateway with OPC UA Publisher mo...   0                    [OK]
```

We will use Windows Server Core Image.

```
PS C:\Users\Administrator> docker pull microsoft/windowsservercore
Using default tag: latest
latest: Pulling from microsoft/windowsservercore
3889bb8d808b: Pull complete
503d87f3196a: Pull complete
Digest: sha256:0565e89ad5e54bada152062976c80524c3a46ec52af2a54bc29f8e9d1d9e8f1b
Status: Downloaded newer image for microsoft/windowsservercore:latest
```

Next we verify the images installed.

```
PS C:\Users\Administrator>  docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
microsoft/windowsservercore   latest              b4713e4d8bab        2 weeks ago         10.1 GB
```

Then we can run our image:

```
PS C:\Users\Administrator> docker run -d -t microsoft/windowsservercore
1afbb007429730474e9280e13866c3772bb37ff34369f5600cc9dc8b04bd011e

```

We can verify that container is running in the background by running the following command.

```
PS C:\Users\Administrator> docker ps
CONTAINER ID        IMAGE                         COMMAND                    CREATED             STATUS              PORTS               NAMES
1afbb0074297        microsoft/windowsservercore   "c:\\windows\\system..."   6 seconds ago       Up 2 seconds                            amazing_ri
tchie
```


We can verify IP address of a container by using below command

```
docker exec -it  1afbb0074297 ipconfig

Windows IP Configuration


Ethernet adapter vEthernet (Container NIC 91cc4be5):

   Connection-specific DNS Suffix  . : DFW1.RACKSPACE.COM
   Link-local IPv6 Address . . . . . : fe80::84ae:3c10:d2aa:f4fa%17
   IPv4 Address. . . . . . . . . . . : 172.20.190.76
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Default Gateway . . . . . . . . . : 172.20.176.1
```

At the end we can verify connectivity between our server and container.

```
PS C:\Users\Administrator> ping 172.20.190.76

Pinging 172.20.190.76 with 32 bytes of data:
Reply from 172.20.190.76: bytes=32 time<1ms TTL=128
Reply from 172.20.190.76: bytes=32 time<1ms TTL=128
```
