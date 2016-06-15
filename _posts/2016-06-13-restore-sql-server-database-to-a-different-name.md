Below tutorial explains how to restore Microsoft SQL Server Database backup to a database with a new name. We will be using two types of backups, namely full and differential. That will allow us to simulate a very common situation in real life. Restoring database to a new name is very handy is situation like the need to compare the current working database with the version you have in the backup, however without affecting live database.


The easiest solution would be to restore the full backup of the database, because then we would need one ```restore``` command (if we do not count ```restore filelistonly``` command). Nonetheless such situation is not common due to the fact that we are not performing full database backups each day, since we are trying to save the storage space and minimize the time required for backup. Below procedure presents a simple way of restoring Microsoft SQL Server database to the new database with a new name.

We start from creating a test database. The folder ```F:\testdb\``` must exist prior to executing this command.

{% highlight sql %}

create database [testdb]
on  primary (
  name = N'testdb',
  filename = N'F:\testdb\testdb.mdf',
  size = 5120KB,
  maxsize = UNLIMITED,
  filegrowth = 1024KB
)
log on (
  name = N'testdb_log',
  filename = N'F:\testdb\testdb_log.ldf',
  size = 1024KB,
  maxsize = 2048GB,
  filegrowth = 10%
)
go

{% endhighlight %}

Then we create a new table and we insert one row.

{% highlight sql %}

use testdb
go

create table t(
  id numeric
);
go

insert into t values(10);
go

{% endhighlight %}

We verify the contents of the ```t``` table.

{% highlight sql %}

select *
from t;
go

{% endhighlight %}

Result:

| id |
| -- |
| 10 |

After that we perform a full backup of our ```testdb``` database. We assume that a destination folder already exists on the disk.

{% highlight sql %}

backup database testdb to
disk = 'Y:\DatabaseBackup\testdb\testdb_20160610.bak'
go

{% endhighlight %}

We insert another row into our ```t``` table.  

{% highlight sql %}

insert into t values(20);

{% endhighlight %}

Next we create a differential backup of our ```testdb``` database.

{% highlight sql %}

backup database testdb to
  disk = 'Y:\DatabaseBackup\testdb\testdb_20160610_diff.bak'
  with differential

{% endhighlight %}

Then we insert one more row into the ```t``` table.

{% highlight sql %}

insert into t values(30);

{% endhighlight %}

So at the the end we have 3 rows in ```t``` table.

| id |
| -- |
| 10 |
| 20 |
| 30 |

We create a folder on disk to store the restored database.

{% highlight text %}

mkdir F:\testdb_restore

{% endhighlight %}

We issue below command, that will generate a list of files included in the backup.

{% highlight sql %}

restore filelistonly from
  disk = 'Y:\DatabaseBackup\testdb\testdb_20160610.bak'

{% endhighlight %}

Interesting columns for us are logical names and physical names.

 LogicalName | PhysicalName
 -- | --
 testdb | F:\testdb\testdb.mdf
 testdb_log | F:\testdb\testdb_log.ldf

We start with restoring a full backup. We need to specify ```norecovery``` option because after full backup is restored, we are going to restore the differential backup. Another crucial option in this step is ```move```, since ```restore``` command on a Microsoft SQL Server will try to restore the files back to their original location, which is exactly the opposite we are trying to achieve. In the original location we have currently running database and we want to restore to new location and different database name, so that we can compare two versions of the same database.


{% highlight sql %}

restore database testdb_restore from
disk = 'Y:\DatabaseBackup\testdb\testdb_20160610.bak'
with
move 'testdb' to 'F:\testdb_restore\testdb_restore.mdf',
move 'testdb_log' to 'F:\testdb_restore\testdb_restore_log.ldf',
recovery

{% endhighlight %}

After restoring full database backup, we are going to restore a differential backup. This time we specify the ```norecovery``` option, because it would be the last backup to restore. After that restore the database ```testdb_restore``` should be fully operational.

{% highlight sql %}

restore database testdb_restore from
disk = 'Y:\DatabaseBackup\testdb\testdb_20160610.bak'
with
move 'testdb' to 'F:\testdb_restore\testdb_restore.mdf',
move 'testdb_log' to 'F:\testdb_restore\testdb_restore_log.ldf',
norecovery

{% endhighlight %}

At the end we verify the contents of the table ```t``` in database ```testdb_restore```.

{% highlight sql %}

select *
from t;
go

{% endhighlight %}

Result:

| id |
| -- |
| 10 |
| 20 |
