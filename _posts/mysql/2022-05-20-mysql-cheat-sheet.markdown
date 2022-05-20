---
layout: post
title:  "MySQL Cheat Sheet"
author: Denis Kobare
date:   2022-05-20 07:00:00 +0300
img: /assets/img/svg/mysql.svg
categories: databases
sub_category: mysql
type: tutorials
technology: MySQL
permalink: "category/:categories/mysql/tutorials/:year:month/:title"
---


<br>
1 . Update only part of a date column e.g year

{% highlight sql %}

update orders set created_at = date_format(created_at, '2018-%m-%d') where year(created_at)=2022;

{% endhighlight %}


<br>
2 . Update column with same values as a column from a join table:

{% highlight sql %}

update order_items left join orders on orders.id = order_items.order_id set order_items.created_at = orders.created_at;

{% endhighlight %}


<br>
3 . With every query that affects data, always begin with the start transaction method, run the Query, and then check if the results are OK. If the results are OK then COMMIT, if not, you can simply ROLLBACK and avoid a disaster. 

{% highlight sql %}

START TRANSACTION;
  UPDATE myTable
  SET name = 'Sam'
  WHERE recordingTime BETWEEN  '2018-04-01 00:00:59' AND '2019-04-12 23:59:59'; 
ROLLBACK;
-- COMMIT; -- commit if you are sure

{% endhighlight %}


<br>
4 . Modify Column

{% highlight sql %}

alter table users modify column sign_in_count int(3);

{% endhighlight %}


<br>
5 . Select Distinct Rows

{% highlight sql %}

select distinct location from users;

{% endhighlight %}


<br>
6 .  View table structure

{% highlight sql %}

    describe attendances;

{% endhighlight %}


<br>
7 .  Create a table and set a unique key

{% highlight sql %}

CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `first_name` varchar(255) DEFAULT NULL,
  `middle_name` varchar(255) DEFAULT NULL,
  `sur_name` varchar(255) DEFAULT NULL,
  UNIQUE KEY `UC_FirstMidSurName` (`sur_name`),
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=latin1 

{% endhighlight %}


<br>
8 .  Create a table and set a unique compound key

{% highlight sql %}

CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `first_name` varchar(255) DEFAULT NULL,
  `middle_name` varchar(255) DEFAULT NULL,
  `sur_name` varchar(255) DEFAULT NULL,
  UNIQUE KEY `UC_FirstMidSurName` (`first_name`,`middle_name`,`sur_name`), -- compound key
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=latin1 

{% endhighlight %}


<br>
9 .  Create a table with datetime columns that have a default current time stamp

{% highlight sql %}

CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `first_name` varchar(255) DEFAULT NULL,
  `middle_name` varchar(255) DEFAULT NULL,
  `sur_name` varchar(255) DEFAULT NULL,
  `created_at` datetime DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,  
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=17 DEFAULT CHARSET=latin1 


{% endhighlight %}


<br>
10 .  View the script that created a table

{% highlight sql %}

    show create table users

{% endhighlight %}


<br>
11 .  Add column to an existing table

{% highlight sql %}

    alter table users add column email varchar(255);

{% endhighlight %}


<br>
12 .  Add column to an existing table after a given column

{% highlight sql %}

    alter table users add column email varchar(255) after sur_name;

{% endhighlight %}


<br>
13 .  Create a table with the same metadata as an existing table;

{% highlight sql %}

    create table sale_orders like orders;

{% endhighlight %}


<br>
14 .  Query data from multiple related tables using left join method

{% highlight sql %}

select prices.id, clients.name as client, products.id, products.name as product, sizes.id, sizes.name as size, prices.price_cents from prices left join sizes on prices.size_id=sizes.id left join clients on prices.client_id=clients.id left join products on sizes.product_id=products.id where clients.name like '%Jenkins%' order by products.id;

{% endhighlight %}


<br>
15 .  Use alias to shorten table names while querying

{% highlight sql %}

select p.id, clients.name as client, products.id, products.name as product, sizes.id, sizes.name as size, p.price_cents from prices as p left join sizes as s on p.size_id=s.id left join clients as c on p.client_id=c.id left join products as d on s.product_id=d.id where c.name like '%Jenkins%' order by d.id;

{% endhighlight %}


<br>
16 .  If you want to update a table while using a where clause that specifies columns from multiple other related tables

{% highlight sql %}

update prices left join sizes on prices.size_id=sizes.id left join clients on prices.client_id=clients.id left join products on sizes.product_id=products.id set prices.price_cents = 51724 where products.id in (14) and sizes.id in (42) and clients.id in (1, 2, 3, 7, 12, 13, 15);

{% endhighlight %}


<br>
17 .  Insert multiple values

{% highlight sql %}

insert into prices (client_id, size_id, price_cents) values ('3', '193', '159483'), ('7', '193', '159483'), ('1', '193', '159483'), ('15', '193', '159483');

{% endhighlight %}


<br>
18 .  Insert multiple values from another table

{% highlight sql %}

insert into users (first_name, middle_name, sur_name) select first_name, middle_name, sur_name from admin_users;

{% endhighlight %}


<br>
19 .  Insert multiple values from another table when the column names are different.

{% highlight sql %}

insert into users (first_name, middle_name, sur_name) select name, second_name, last_name from staff_users;

{% endhighlight %}


<br>
20 .  Create database backup file

{% highlight sql %}

mysqldump -u user_name_here -p database_name_here > backup_file_name_here.sql

{% endhighlight %}


<br>
21 .  Insert data  from the backup file back into the database

{% highlight sql %}

mysql -u user_name_here -p database_name_here < backup_file_name_here.sql

{% endhighlight %}


<br>
*Thanks for reading, see you in the next one!*
