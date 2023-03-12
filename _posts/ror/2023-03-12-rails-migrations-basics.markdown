---
layout: post
title:  "Rails Migrations Basics"
author: Denis Kobare
date:   2023-03-12 09:35:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Introduction

Rails Migrations are a powerful tool that allows developers to manage database 
changes and schema updates in a version-controlled and consistent way. Migrations 
help developers to easily modify the structure of their application’s database, 
and they are a key component of the Ruby on Rails framework.

In this section, we'll cover the basics of how to create, modify, and manage 
migrations in a Rails application.


<br>
### Creating a Migration

To create a new migration in a Rails application, you can use the 
<span class="badge">rails generate</span> command followed by the migration name 
and the list of attributes that you want to add to the database table. For example, 
to create a migration that adds a new column to the users table, you can run the 
following command:

<br>
{% highlight terminal %}

$ rails generate migration add_email_to_users email:string

{% endhighlight %}


<br>
This will create a new migration file in the <span class="badge">db/migrate</span> 
directory with a name that looks something like 
<span class="badge">20210312132235_add_email_to_users.rb</span>. The name of the 
migration file includes a timestamp to ensure that each migration is executed in 
the correct order.

The migration file will contain two methods: <span class="badge">up</span> and 
<span class="badge">down</span>. The <span class="badge">up</span> method is used 
to define the changes that need to be made to the database, and the 
<span class="badge">down</span> method is used to define how to revert those changes.

Here’s an example of what the up method might look like for the 
<span class="badge">add_email_to_users</span> migration:


<br>
{% highlight ruby %}

def up
  add_column :users, :email, :string
end

{% endhighlight %}



<br>
This code will add a new column named email to the users table with a data type 
of string.

The <span class="badge">down</span> method is used to undo the changes made by 
the <span class="badge">up</span> method. Here’s an example of what the 
<span class="badge">down</span> method might look like for the 
<span class="badge">add_email_to_users</span> migration:


<br>
{% highlight ruby %}

def down
  remove_column :users, :email
end

{% endhighlight %}


<br>
This code will remove the email column from the users table.



<br>
### Running Migrations

Once you’ve created a migration, you can run it by running the following command:


<br>
{% highlight terminal %}

$ rails db:migrate

{% endhighlight %}


<br>
This will execute all the pending migrations in your application. Rails keeps 
track of which migrations have already been run, so running 
<span class="badge">rails db:migrate</span> multiple times will not apply the 
same migration more than once.


You can also roll back a migration by running the following command:

{% highlight terminal %}

$ rails db:rollback

{% endhighlight %}


<br>
This will undo the last migration that was applied. You can roll back multiple 
migrations by specifying the number of migrations to roll back:


<br>
{% highlight terminal %}

$ rails db:rollback STEP=2

{% endhighlight %}

<br>
This will roll back the last two migrations that were applied.


<br>
### Modifying a Migration

If you need to modify an existing migration, you can edit the migration file directly. 
However, you should be aware that once a migration has been run, it should not 
be modified unless absolutely necessary. Modifying a migration that has already 
been applied can cause issues with the consistency of your database schema.

If you need to make a change to a migration that has already been run, you 
should create a new migration that modifies the existing schema. For example, 
if you need to rename a column that was added in a previous migration, you could 
create a new migration that renames the column:

<br>
{% highlight terminal %}

$ rails generate migration rename_email_column_in_users

{% endhighlight %}



<br>
This will create a new migration file that you can use to rename the column. 
Here’s an example of what the up method might look like:


<br>
{% highlight ruby %}

def up
  rename_column :users, :email, :new_email
end

{% endhighlight %}

<br>
This code will rename the email column to new_email.



<br>
### Adding Indexes

Adding indexes to your database tables can help to improve query performance. 
You can create an index on a column in a table using a migration. Here’s an 
example of how to create an index on the email column in the users table:


<br>
{% highlight ruby %}

def change
  add_index :users, :email
end

{% endhighlight %}

<br>
This code will create an index on the email column in the users table. Rails will 
automatically generate a name for the index based on the table and column names.



<br>
### Dropping Tables

If you need to remove a table from your database, you can do so using a migration. 
Here’s an example of how to remove the users table:

<br>
{% highlight ruby %}

def change
  drop_table :users
end

{% endhighlight %}


<br>
This code will remove the users table from your database.



<br>
### Conclusion

Migrations are a powerful tool that allow you to manage database changes and 
schema updates in a version-controlled and consistent way. In this tutorial, 
we covered the basics of how to create, modify, and manage migrations in a 
Rails application.

Remember that once a migration has been applied, it should not be modified unless 
absolutely necessary. If you need to make a change to a migration that has already 
been applied, create a new migration that modifies the existing schema.

With Rails Migrations, you can easily modify the structure of your application’s 
database and keep your schema in sync with your application’s code.


<br>
*Thanks for reading, see you in the next one!*
