The most likely cause for the following error message ``` (host mail.test.com[10.10.100.200] said: 550 Access denied - Invalid HELO name (See RFC2821 4.1.1.1) (in reply to MAIL FROM command)) ```
in postfix ```mail.log``` or ```maillog``` file is misconfiguration of ```myhostname``` parameter in ```main.cf``` configuration file.

The change is pretty simple we have to modify postfix main configuration file, which is usually located in ```/etc/postfix/main.cf``` and set ```myhostname``` variable to a proper FQDN of our mail server (the FQDN must be verifiable by using DNS query, so do not use local or localdomain as domain name).  
