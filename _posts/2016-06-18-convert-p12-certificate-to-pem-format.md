In cryptography, PKCS #12 defines an archive file format for storing many cryptography objects as a single file. It is commonly used to bundle a private key with its X.509 certificate or to bundle all the members of a chain of trust. One of the common place we can find those types of certificates is when we are using two factor authentication based on certificates on Cisco ASA.


PKCS #12 (or p12) are often used interchangeable with PFX, which is a format developed by Microsoft . However PKCS #12 is a successor of PFX.

The easiest way to convert the p12 file to pem certificate and associated key is to use openssl library.

To extract certificate object from pkcs #12 file we use the below ```openssl``` command.

{% highlight text %}

openssl pkcs12 -in my-file.p12 -out my-cert.pem -clcerts -nokeys

{% endhighlight %}

To extract private key from p12 file we use the below command:

{% highlight text %}

openssl pkcs12 -in my-file.p12 -out my-key.key -nocerts -nodes

{% endhighlight %}

The ```-in``` parameter specifies the path to p12 file.

The ```-out``` parameter specifies the filename for the extracted object from p12 (which in this example is either cert or private key).

The ```-clcerts``` parameter informs openssl to only output client certificates (not CA certificates).

The ```-nokeys``` parameter specifies that no keys will be extracted.

The ```-nocerts``` parameter specifies that no certificates will extracted.

The ```-nodes``` parameter informs openssl not to encrypt private key.

Description of all other parameters can be found on man page for pkcs12.

{% highlight text %}

man pkcs12

{% endhighlight %}
