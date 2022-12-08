---
layout: post
title:  "Configuring Multiple Databases in Rails 7"
author: Denis Kobare
date:   2022-12-08 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---


### Definition

Rails 7 natively supports attaching multiple databases to the application. But why 
would you want to have more than one database? Well, there are several reasons. 
For example in the case where your app is growing, and database concurrency issues 
arise. You may need to scale on a database level. As it becomes apparent that 
decoupling the data would prevent overload on one database.

Multiple databases are also useful for backups and readonly replicas.


This section takes a look at how the <span class="badge">disable_joins: true</span> option can be used to fetch data belonging to different databases using associations.


<br>
### Setting Up the Databases

1 . Edit the <span class="badge">database.yml</span> file to resemble this:

{% highlight yml %}

default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["MYSQL_USERNAME"] %>
  password: <%= ENV["MYSQL_PASSWORD"] %> 
  socket: /var/run/mysqld/mysqld.sock

development:
  some_name_1:
    <<: *default
    database: example_1
    
  some_name_2:   # NB: To migrate this specific db do: rake db:migrate some_name_2
    <<: *default
    database: example_2
    migrations_paths: db/example_2_migration_folder

{% endhighlight  %}

<br>

### Execution Example
Suppose we have an ecommerce app that has a database that's massively growing 
because customers are leaving tons reviews like nobody's business. Consequently, We want to 
separate the reviews from the main database. Here's how we'd do it:


<br>
#### Configuring The Databases

{% highlight yml %}

default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["MYSQL_USERNAME"] %>
  password: <%= ENV["MYSQL_PASSWORD"] %> 
  socket: /var/run/mysqld/mysqld.sock

development:
  ecommerce:
    <<: *default
    database: ecommerce_development
    
  customer_reviews:   
    <<: *default
    database: customer_reviews_development
    migrations_paths: db/customer_reviews_migrate

{% endhighlight  %}



<br>
#### Configuring ActiveRecord

We need to let Rails know which database to connect to.

We can use the <span class="badge">application_record.rb</span> file for one of the 
databases and create other new files that inherit from <span class="badge">ActiveRecord</span> 
for the subsequent databases.

{% highlight ruby %}

# app/models/application_record.rb

class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
  connects_to database: { writing: :ecommerce_development, reading: :ecommerce_development }
end

{% endhighlight  %}


<br>
For the customer_reviews_development database, create new file that inherits from <span class="badge">Activerecord</span>.

{% highlight ruby %}

# app/models/review_record.rb

class ReviewRecord < ActiveRecord::Base
  self.abstract_class = true
  connects_to database: { writing: :customer_reviews_development, reading: :customer_reviews_development }
end

{% endhighlight  %}


<br>
#### Writting the Models

The models will inherit from their respective <span class="badge">Activerecord</span> files as configured previously.

{% highlight ruby %}

# app/models/user.rb
class User < ApplicationRecord
  has_one :customer
  has_many :reviews, through: :customer, disable_joins: true  
end

{% endhighlight  %}


<br>
{% highlight ruby %}

# app/models/customer.rb
class Customer < ApplicationRecord
  has_many :reviews
end

{% endhighlight  %}


<br>
{% highlight ruby %}

# app/models/review.rb
class Review < ReviewRecord
  belongs_to :customer
end

{% endhighlight  %}


<br>
#### NOTE:
For <span class="badge">has_many :through</span> or <span class="badge">has_one :through</span> 
associations, we specify the <span class="badge">disable_joins: true</span> option 
for it to work. We disable joins because Rails lazy loads the associations by 
default and tries to perform a join on them. This throws an error because you can't 
join across different databases.

The <span class="badge">disable_joins: true</span> option allows us to avoid that problem. Instead of performing a join, it executes 
two separate queries to fetch the customer_id/s first and then the review/s data using the customer_id/s from the first query.


<br>
*Thanks for reading, see you in the next one!*
