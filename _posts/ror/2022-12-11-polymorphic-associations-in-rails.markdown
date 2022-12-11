---
layout: post
title:  "Polymorphic Associations in Rails"
author: Denis Kobare
date:   2022-12-08 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Definition

In Rails, a polymorphic association is an Active Record association that can 
connect a model to multiple other models and therefore eliminating the need to add
multiple index columns for the respective associations.


<br>
### Application Example

There are three models: 
<span class="badge">review.rb</span>,
<span class="badge">movie.rb</span>,
<span class="badge">actor.rb</span>

A review belongs to both a movie and an actor. There are two ways to create these associations:


<br>
#### 1. By Adding movie_id and actor_id columns in reviews table

With this approach, you create the movie_id and actor_id columns in reviews 
table and then edit the associations in the models like so:

<br>
{% highlight ruby %}

# app/models/review.rb
class Review < ApplicationRecord
  belongs_to :movie
  belongs_to :actor
end


# app/models/Movie.rb
class Movie < ApplicationRecord
  has_many :reviews
end


# app/models/actor.rb
class Actor < ApplicationRecord
  has_many :reviews
end

{% endhighlight  %}


There's no problem with this, it works just fine. But wait, what if you start having additional 
other tables that have a similar association with reviews? That will quickly become 
redundant and that's where polymorphic associations come to our rescue. 


<br>
#### 2. Using a polymorphic association

The first approach works well but as we have seen, it can become unnecessarily messy. 
A polymorphic association can help us avoid that. Essentially, since both the 
movies and the actors table have a similar association with the reviews table,
then we can definitely use shared generic columns to map these associations.

When using the polymorphic approach, rails creates a virtual table to represent 
the the table that has associations with multiple other tables. In this case, 
the <span class="badge">reviews</span> table.


  i). Configure a virtual table for <span class="badge">reviews</span> and call it something like <span class="badge">reviewable</span>.

<br>
{% highlight ruby %}

# app/models/review.rb
class Review < ApplicationRecord
  belongs_to :reviewable, polymorphic: true
  
  def self.create_review(description, reviewable)
    Review.create(description: description, reviewable_id: reviewable.id, reviewable_type: reviewable.class.name)
  end
  
end

{% endhighlight  %}


  <br>
  ii). Map the virtual table to the other tables like so:

{% highlight ruby %}

# app/models/movie.rb
class Movie < ApplicationRecord
  has_many :reviews, as: :reviewable  
end


# app/models/actor.rb
class Actor < ApplicationRecord
  has_many :reviews, as: :reviewable
end

{% endhighlight  %}



  <br>
  iii). Add the generic columns for the virtual table on the table in focus (reviews table)

{% highlight terminal %}

# alter table table_name add column [virtual_table_name]_id int(11);
# alter table table_name add column [virtual_table_name]_type varchar(255);

$ alter table reviews add column reviewable_id int(11);

$ alter table reviews add column reviewable_type varchar(255);

{% endhighlight  %}
  


<br>
The name of the class is stored in the <span class="badge">reviewable_type</span> column so that rails can 
use that in combination with the <span class="badge">reviewable_id</span> to determine the records associated with them.


  <br>
  iv). Test it

{% highlight terminal %}

movie = Movie.find(22)
customer = Customer.find(155)
description = "Awesome!"

review.create_review(description, movie)
review.create_review(description, customer)

{% endhighlight %}


<br>
Rails can automatically query the database using the polymorphic association:
 
{% highlight terminal %}

Movie.first.reviewable

SELECT `reviews`.* FROM `reviews` WHERE `reviews`.`reviewable_id` = 1 AND `reviews`.`reviewable_type` = 'Movie'

{% endhighlight %}

<br>
*Thanks for reading, see you in the next one!*
