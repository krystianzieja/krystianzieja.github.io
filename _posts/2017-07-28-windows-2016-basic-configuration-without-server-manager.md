On Windows 2016 Server Core we do not have access to server manager.
However Microsoft provides a convenient way to configure the server with ``` sconfig.cmd ``` tool.
``` sconfig.cmd ``` is available on both server core and full desktop experience installations.


Below example shows how to enable Remote Desktop using sconfig.cmd.

We start from typing sconfig.cmd and below table with options will be shown.

![sconfig menu]({{ site.url }}/images/w2016-sconfig-01.png)

Then we type 7 to configure Remote Desktop and we select the desired options

![sconfig configuration]({{ site.url }}/images/w2016-sconfig-02.png)
