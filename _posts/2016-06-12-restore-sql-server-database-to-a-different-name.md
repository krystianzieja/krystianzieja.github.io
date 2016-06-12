Below tutorial explains how to restore Microsoft SQL Server Database backup to a database with a new name. We will be using two types of backups, namely full and differential. That will allow us to simulate a very common situation in real life. Restoring database to a new name is very handy is situation like the need to compare the current working database with the version you have in the backup, however without affecting live database.


The easiest solution would be to restore the full backup of the database, because then we would need one ```restore``` command (if we do not count ```restore filelistonly``` command). Nonetheless such situation is not common due to the fact that we are not performing full database backups each day, since we are trying to save the storage space and minimize the time required for backup. Below procedure presents a simple way of restoring Microsoft SQL Server database to the new database with a new name.

1. We start from creating a test database.

2. Then we create a new table and we insert one row.

3. After that we perform a full backup of our testdb database. 

{% highlight sql %}
backup database to disk = 'y:\backup\testdb.bak'
{% endhighlight %}
