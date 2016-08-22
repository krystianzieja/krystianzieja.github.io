Sometimes we need to suppress column names in MySQL query results. It is very easy to do by invoking mysql client with ```-N``` option, which suppress columns names in the query output.


Example:

Standard execution:

{% highlight sql%}

mysql test

mysql> select id, name from vendors;
+----+----------------------+
| id | name                 |
+----+----------------------+
|  1 | ACT                  |
|  2 | ACT2                 |
|  3 | ACT3                 |
|  4 | ACT4                 |
+----+----------------------+

{% endhighlight %}

Execution without column names:

{% highlight sql%}

mysql -N test

mysql> select id, name from vendors;
+----+----------------------+
|  1 |                  ACT |
|  2 |                 ACT2 |
|  3 |                 ACT3 |
|  4 |                 ACT4 |
+----+----------------------+

{% endhighlight %}


If we also need to remove the grid we can add ```-s``` option while invoking MySQL client.

{% highlight sql%}

mysql -N -s test

mysql> select id, name from vendors;
1	ACT
2	ACT2
3	ACT3
4	ACT4


{% endhighlight %}
