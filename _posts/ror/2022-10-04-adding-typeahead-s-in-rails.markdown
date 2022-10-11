---
layout: post
title:  "Adding Typeahead in Rails"
author: Denis Kobare
date:   2022-10-02 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Definition
Typeahead is JavaScript library that provides a list of suggestions for users as 
they search or fill a form, improving the user experienc as a result.

<br>
### Create the models

1 . Create the <span class="badge">todo_list.rb</span> model

 i). Generate the models
 
{% highlight terminal %}

 $ rails g model todo_list
 
 $ rails g model task 

{% endhighlight %}
 
   
 <br>
 ii). Edit the files
  
{% highlight ruby %}
# models/todo_list.rb

class TodoList < ApplicationRecord

  has_many :tasks
  
end

{% endhighlight %}


<br>
{% highlight ruby %}
# models/task.rb

class Task < ApplicationRecord

  belongs_to :todo_list
  
end

{% endhighlight %}


<br>
2 . Create migrations

 i). Edit the migrations

{% highlight ruby %}
# db/migrate/20221002112836_create_todo_lists.rb

class CreateTodoLists < ActiveRecord::Migration[7.0]
  def change
    create_table :todo_lists do |t|
      t.string :name

      t.timestamps
    end
  end
end

{% endhighlight %}


<br>
{% highlight ruby %}
# db/migrate/20221002114011_create_tasks.rb

class CreateTasks < ActiveRecord::Migration[7.0]
  def change
    create_table :tasks do |t|
      t.string :name
      t.boolean :completed
      t.date :date

      t.timestamps
    end
  end
end

{% endhighlight %}


 <br>
 ii) Run migrations
 
{% highlight terminal %}

$ rails db:migrate

{% endhighlight %} 


<br>
3 . Create the todo_lists controller

{% highlight terminal %} 

$ rails g controller todo_lists

{% endhighlight %} 


<br>
*Thanks for reading, see you in the next one!*
