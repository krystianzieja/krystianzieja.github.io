PHP supports backticks operator ``. PHP will try to execute the command that is enclosed between backticks. Having this operator enabled can potentially create a vulnerability. For instance when our application accepts not validated user input inside backticks operator.

Example:

```
<?php
$output = `ls -l`;
echo "<pre>$output</pre>";
?>
```

When we execute above file the php server will return a list of files in current directory.

```
drwxrwxr-x.  16 www-data www-data  20480 Oct 11 10:53 css
-rwxrwxr-x.   1 www-data www-data   3075 Nov 25  2015 css.php
```

We can disable backticks operator using two techniques. The first is to enable safe_mode in php.ini (however safe_mode setting is deprectated in PHP 5.4 and removed in PHP 7.x) or disable shell_exec function.

To disable shell_exec function we need to edit php.ini file and modify ```disable_functions``` setting.

```
disable_functions=shell_exec
```

The last step is to restart Apache Server and test if backticks operator is disabled.

```
service httpd restart
```

or

```
systemctl restart httpd
```
