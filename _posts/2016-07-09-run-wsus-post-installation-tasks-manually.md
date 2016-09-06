# Run WSUS Post Installation Tasks Manually

Sometimes after reinstalling Windows Server Update Services using Server Manager, the post installation tasks fail. To resolve this problem we can perform those tasks manualy. WSUS comes with a program named WsusUtil, which we will use for that purpose.

The ```WsusUtil``` program is located in ```C:\Program Files\Update Services\Tools```. So in PowerShell or command line windows we have to change current directory using below command

```
cd 'C:\Program Files\Update Services\Tools'
```

To display available arguments that we can specify for post install task we execute below command:

```
PS C:\Program Files\Update Services\Tools> .\WsusUtil.exe postinstall /?

PostInstall [SQL_INSTANCE_NAME=<Name>] [CONTENT_DIR=<Path>]

```

So we can specify both Sql Server instance we would liek to use and also a folder where WSUS will store updates (that folder should exist prior to running this command).

In the example below we specify Windows Internal Database as SQL Server Instance, and C:\Wsus-Updates as our Content Directory

```
PS C:\Program Files\Update Services\Tools> .\WsusUtil.exe postinstall SQL_INSTANCE_NAME=microsoft##wid CONTENT_DIR=C:\WSUS-Updates
```