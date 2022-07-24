---
layout: post
title:  "Postgres Cheat Sheet"
author: Denis Kobare
date:   2022-07-21 13:40:00 +0300
img: /assets/img/svg/postgresql.svg
categories: databases
sub_category: postgres
type: tutorials
technology: Postgres
permalink: "category/:categories/postgres/tutorials/:year:month/:title"
---


<br>
1 . Connect to postgresql

a). First Option 

{% highlight terminal %}

$ sudo -u postgres psql

{% endhighlight %}


a). Second Option

{% highlight terminal %}

# switch user
$ sudo su - postgres

# connect to postgresql
$ psql

{% endhighlight %}


<br>
2 . Exit postgres

{% highlight sql %}

\q

{% endhighlight %}


<br>
3 . List databases

{% highlight sql %}

\l

# include details like description, tablespace & DB size
\l+

{% endhighlight %}


<br>
4 . Connect to a database 

{% highlight sql %}

\c database_name

{% endhighlight %}


<br>
5 . Create a database

{% highlight sql %}

create database database_name

{% endhighlight %}


<br>
6 . Delete a database

{% highlight sql %}

drop database database_name

{% endhighlight %}


<br>
7 .  List tables

{% highlight sql %}

\dt

{% endhighlight %}


<br>
8 . Describe table

{% highlight sql %}

\d table_name

# include more details
\d+ table_name

{% endhighlight %}


<br>
9 . List users and their roles

{% highlight sql %}

\du

{% endhighlight %}


<br>
10 . Show current user

{% highlight sql %}

select current_user;

{% endhighlight %}


<br>
11 . Show connection information

{% highlight sql %}

\conninfo

{% endhighlight %}


<br>
12 .  Rename a database

{% highlight sql %}

alter database current_db_name rename to new_db_name;

{% endhighlight %}


<br>
13 .  Grant all permissions on database to user

{% highlight sql %}

grant all privileges on database db_name to user_name;

{% endhighlight %}


<br>
14 .  List tables for a schema

{% highlight sql %}

\dt name_of_schema.*

{% endhighlight %}


<br>
15 .  Add a comment on a table

{% highlight sql %}

Comment on table user is 'App tenant records';

{% endhighlight %}


<br>
16 .  Add a comment on a column

{% highlight sql %}

Comment on column user.pin is 'Tenant tax identification Number';

{% endhighlight %}


<br>
17 .  Copy table data to CSV file

{% highlight sql %}

\copy (SELECT * FROM table_name) TO 'file_path_and_name.csv' WITH CSV

{% endhighlight %}


<br>
18 .  Execute commands from a file

{% highlight terminal %}

psql mydb -f file.sql

{% endhighlight %}


<br>
19 .  Connect remote PostgreSQL

{% highlight terminal %}

psql -U user_here -h host_ip_here -p port_here -d db_name_here

# example
psql -U admin -h 35.21.09.12 -p 2506 -d mydb

{% endhighlight %}


<br>
20 .  Generate HTML report

{% highlight terminal %}

psql -c "\l+" -H postgres > database.html

{% endhighlight %}


<br>
21 .  Print the psql version

{% highlight terminal %}

psql -V

{% endhighlight %}


<br>
22 .  Force password

{% highlight terminal %}

psql -W mydb

{% endhighlight %}


<br>
23 .  Connect to a host/port

{% highlight terminal %}

psql -h localhost -p 5432 mydb

{% endhighlight %}


<br>
24 .  Connect as a specific user

{% highlight terminal %}

psql -U user_name database_name

{% endhighlight %}


<br>
25 .  Backup all databases

{% highlight terminal %}

pg_dumpall -U postgres > all.sql

{% endhighlight %}


<br>
26 .  Backup a database

{% highlight terminal %}

pg_dump -d db_name -f mydb_backup.sql

# Important flags

-a   Dump only the data, not the schema
-s   Dump only the schema, no data
-c   Drop database before recreating
-C   Create database before restoring
-t   Dump the named table(s) only
-F   Format (c: custom, d: directory, t: tar)

{% endhighlight %}


<br>
27 .  Restore a database

{% highlight terminal %}

psql -U user db_name < mydb_backup.sql

# alternative

pg_restore -d db_name mydb_backup.sql -c

# Important flags

-U   Specify a database user
-c   Drop database before recreating
-C   Create database before restoring
-e   Exit if an error has encountered
-F   Format (c: custom, d: directory, t: tar, p: plain text sql(default))


{% endhighlight %}


<br>
28 .  Import CSV file into table

{% highlight sql %}

\copy table FROM '<path>' CSV
\copy table(col1,col1) FROM '<path>' CSV

{% endhighlight %}


<br>
29 .  List table access privileges

{% highlight sql %}

\dp

{% endhighlight %}


<br>
30 .  Change user password

{% highlight terminal %}

# a).

# 1. Login into the psql:

$ sudo -u postgres psql


# 2. Then in the psql console change the password and quit:

postgres=# \password postgres
Enter new password: <new-password>
postgres=# \q

# or 

# b). 

# 1. To log in without a password:

sudo -u user_name psql db_name

# 2. To reset the password if you have forgotten:

ALTER USER user_name WITH PASSWORD 'new_password';


{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
