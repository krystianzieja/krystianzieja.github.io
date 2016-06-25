One of the most handy tool in Linux arsenal of tools is ```grep```. It allows us to search through file using regular expressions and print only the lines that matched our regexp.

However sometimes we want to print only the string that matched instead of whole line. Fortunately ```grep``` can help us also with that requirement. In the example below we will select the rule id from ModSecurity log. The example line from ModSecurity log:

{% highlight text %}

Message: Access denied with code 403 (phase 2). Pattern match "(?i:(\\!\\=|\\&\\&|\\|\\||>>|<<|>=|<=|<>|<=>|xor|rlike|regexp|isnull)|(?:not\\s+between\\s+0\\s+and)|(?:is\\s+null)|(like\\s+null)|(?:(?:^|\\W)in[+\\s]*\\([\\s\\d\"]+[^()]*\\))|(?:xor|<>|rlike(?:\\s+binary)?)|(?:regexp\\s+binary))" at REQUEST_COOKIES:_cc. [file "/etc/httpd/modsecurity.d/activated_rules/modsecurity_crs_41_sql_injection_attacks.conf"] [line "70"] [id "981319"] [rev "2"] [msg "SQL Injection Attack: SQL Operator Detected"] [data "Matched Data: xOR found within REQUEST_COOKIES:test_cookie: AWL8NlCp2OgGRqGc0dZwBClmxvChYC3KeNEp0MGNJFUWuD2XuQU1pPSu6faOHofbLNOWJrWb6T1gLM/qicpLSe8pgTXv5WqJTgBlbzGLZvWVTQF9ygDbSpziyw+LsrNyZZR3vNpw/ngxZb5f/mFTpXHStDsvYVD87SeSeVZEvf/qCzr5Sfy96W5Tj6Gw7owNF/tYP4b9ZeOQ9VooAiwTwPYcmLAzs+xOR/Dh2g/DjZDGoGvENXz5jNZh1Dsgb/qhdQB6adywb0O/mxs2ZF8dnHgwMNrt06WHSq/XrzE/aCWZh76jgEe4yUK+ckiV7VlSMXUudApEQpFXknTt8IU+b+jEE+5FX34N7H4CPoMTl0Qnellt"] [severity "CRITICAL"] [ver "OWASP_CRS/2.2.6"] [maturity "9"] [accuracy "8"] [tag "OWASP_CRS/WEB_ATT

{% endhighlight %}

Let's assume that we want to grab all ModSecurity rules id that were triggered. In the log the rule is is provided in below string.

{% highlight text %}

[id "rule_number"]

{% endhighlight %}

Example:

{% highlight text %}

[id "981319"]

{% endhighlight %}


To extract only matched string we use the ```-o``` option in ```grep``` command.

{% highlight text %}

grep -Eo '\[id "[0-9]+"\]' /var/log/httpd/modsec_audit.log

[id "981319"]
[id "981319"]
[id "981319"]
[id "981319"]

{% endhighlight %}

The ```-E``` option specifies that ```grep``` will use extended regular expression. Our regular expression ```'\[id "[0-9]+"\]'``` is pretty simple. We start from escaping (using "\" character) the opening square bracket. That is required because square brackets have special meaning in regular expressions, the allow us to specify a class of characters. After the square bracket we provide a string ```id "``` (id, space, double quote) that must be matched, which is below by at least one number, ```[0-9]+``` means one or more digit from a set between 0 and 9. We end the expression with escaped string ```"\]```.

A small tip at the end, if we want to remove duplicates from the output we can use the ```sort``` command with ```-u``` option to guarantee uniqueness.


{% highlight text %}

grep -Eo '\[id "[0-9]+"\]' /var/log/httpd/modsec_audit.log | sort -u

[id "981319"]

{% endhighlight %}
