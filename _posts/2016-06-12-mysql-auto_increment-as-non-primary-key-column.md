Most often the AUTO_INCREMENT column in MySQL is used as primary key. However it is possible to define a column as AUTO_INCREMENT, which is not a defined as primary key. It is not the most useful MySQL feature, but it is worth to know.


The only requirement for AUTO_INCREMENT column in MySQL is that it is defined as a part of some index. That means that below table creation script will fail, because AUTO_INCREMENT column is not a part of any index.

{% highlight sql %}

create table test(
  id int auto_increment,
  code char(16) primary key
);

{% endhighlight %}

MySQL will return the following error.

{% highlight text %}

ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key

{% endhighlight %}

Below table definition will also fail, due to a fact that defining column as not null does not create an index.

{% highlight sql %}

create table test(
  id int auto_increment not null,
  code char(16) primary key
);

{% endhighlight %}

The error will be exactly the same as the one above.

{% highlight text %}

ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key

{% endhighlight %}


This table definition will work just fine, because there is an index defined on AUTO_INCREMENT column. Additionally AUTO_INCREMENT column will behave as we expect.

{% highlight sql %}

create table test(
  id int auto_increment,
  code char(16) primary key,
  key (id)
);

insert into test(code) values('AAA-BBB-CCC-0000');

insert into test(code) values('AAA-BBB-CCC-0001');

select *  from test;

{% endhighlight %}

Result:

|----|------------------|
| id | code             |
|----|------------------|
|  1 | AAA-BBB-CCC-0000 |
|  2 | AAA-BBB-CCC-0001 |
|----|------------------|

It is important to note, that the index created on column ```id``` is not unique, which means that we can insert another row with ```id``` equal 2.

{% highlight sql %}

insert into test(id, code) values(2, 'AAA-BBB-CCC-0002');

select *  from test;

{% endhighlight %}

Result:

|----|------------------|
| id | code             |
|----|------------------|
|  1 | AAA-BBB-CCC-0000 |
|  2 | AAA-BBB-CCC-0001 |
|  2 | AAA-BBB-CCC-0002 |
|----|------------------|

When we tried to insert another row into ```t``` table the AUTO_INCREMENT column will be incremented to value of 3.

{% highlight sql %}

insert into test(id, code) values(null, 'AAA-BBB-CCC-0003');

select *  from test;

{% endhighlight %}


Result:

|----|------------------|
| id | code             |
|----|------------------|
|  1 | AAA-BBB-CCC-0000 |
|  2 | AAA-BBB-CCC-0001 |
|  2 | AAA-BBB-CCC-0002 |
|  3 | AAA-BBB-CCC-0003 |
|----|------------------|

If we want to prevent the possibility of duplicate values in ```id``` column we have to define an unique index on AUTO_INCREMENT column.


{% highlight sql %}

drop table test;

create table test(
  id int auto_increment,
  code char(16) primary key,
  constraint unique key(id)
);

insert into test(code) values('AAA-BBB-CCC-0000');

insert into test(code) values('AAA-BBB-CCC-0001');

select *  from test;

{% endhighlight %}

Result:

|----|------------------|
| id | code             |
|----|------------------|
|  1 | AAA-BBB-CCC-0000 |
|  2 | AAA-BBB-CCC-0001 |
|----|------------------|

When we try to insert another row with ```id``` equal 2, it will fail.

{% highlight sql %}

insert into test(id, code) values(2, 'AAA-BBB-CCC-0002');

{% endhighlight %}

Result

{% highlight text %}

ERROR 1062 (23000): Duplicate entry '2' for key 'id'

{% endhighlight %}
