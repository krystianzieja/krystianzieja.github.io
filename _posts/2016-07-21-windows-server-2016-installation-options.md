Windows Server 2016 has three installation options. Two of them (**Server Core**, **Server with Desktop Experience**) were available in previous releases. The new option is **Nano Server**.

*CAUTION!:* In previous Windows Server releases we could migrate from core installation to full desktop experience, in Windows 2016 such conversion is not possible.

Each of the installation options is aimed to serve different needs:

**Nano Server:** is a remotely administered server operating system optimized for private clouds and datacenters. It is similar to Server Core mode, but significantly smaller, has no local logon capability, and only supports 64-bit applications, tools, and agents. It takes up far less disk space, sets up significantly faster, and requires far fewer updates and restarts than the other options.

**Server Core:** reduces the space required on disk, the potential attack surface, and especially the servicing requirements. If you do not have a particular need for GUI, it is the recommended option.

**Server with Desktop Experience:** installs the standard user interface and all tools, including client experience features that required a separate installation in Windows Server 2012 R2.
It can be managed by local Server Manager.
