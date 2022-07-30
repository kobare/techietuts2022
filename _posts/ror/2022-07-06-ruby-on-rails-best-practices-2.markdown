---
layout: post
title:  "Ruby On Rails Best Practices #2"
author: Denis Kobare
date:   2022-07-06 16:20:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Introduction

Good programming practices promote readability and maintainability of the code and also reduces complexity.
These coding standards enables a team to have a better overall understanding of a project, which leads to fewer errors and a reduced need for constant fixes.

It is good to learn the best practices early on to make the most out of any programming language. This section describes some of the best practices for the RoR framework.

<br>
### 1. Rescue from <span class="badge">StandardError</span> Rather Than <span class="badge">Exception</span>
Rescuing from Exception broadens the scope rather than narrowing it, and can have catastrophic results and make bug-hunting a pain.
Exception is the root of Ruby's exception hierarchy, so when you rescue Exception you rescue from everything, including subclasses such as <span class="badge">SyntaxError</span>, <span class="badge">LoadError</span>, and <span class="badge">Interrupt</span>.

By default Ruby will rescue from <span class="badge">StandardError</span> 
<br>

{% highlight ruby %}

begin
  # ...
rescue
  # ...
end

{% endhighlight %}  

<br>
If you need a variable with the exception, you could do this:

{% highlight ruby %}

begin
  # ...
rescue => e
  # ...
end


# That's also equivalent to:

begin
  # ...
rescue StandardError => e
  # ...
end

{% endhighlight %}  

<br>
However, it is okay to rescue from <span class="badge">Exception</span> for logging/reporting purposes, in which case you should immediately re-raise the exception:

{% highlight ruby %}

begin
  # ...
rescue Exception => e
  # do some logging
  raise # ...
end

{% endhighlight %}  


<br>
### 2. Don't Violate The Law Of Demeter
In programming the Law Of Demeter is not really a law but a guidline that states: an entity or object should not call methods through another entity or object.

For instance, say you have 3 related models: <span class="badge">Author</span>, <span class="badge">Article</span> and <span class="badge">Comment</span>

{% highlight ruby %}

class Author < ActiveRecord::Base
  has_many :books
end

class Book < ActiveRecord::Base
  belongs_to :author
  has_many :reviews  
end

class Review < ActiveRecord::Base
  belongs_to :book
end

{% endhighlight %} 

<br>
Suppose you want to get information about the author related to a given review? Doing the following would work:

{% highlight erb %}

<%= @review.book.author.name %>

<%= @review.book.author.country %>

{% endhighlight %} 

<br>
But using several dots is a violation of of the Law of Demeter which would have you use only one.
To avoid the multi dot syndrome, Rails provides a <span class="badge">delegate</span> method which helps you shorten your methods. 
To achieve that, you need to delegate <span class="badge">Author</span> methods in <span class="badge">Book</span> model:

{% highlight ruby %}

class Book < ActiveRecord::Base
  belongs_to :author
  has_many :reviews 
  
  delegate :name, :country, to: :author, allow_nil: true
     
end
  
{% endhighlight %} 

<br>
Now <span class="badge">Book</span> can use <span class="badge">Author</span> methods. 
But since you're using the methods from <span class="badge">Review</span> instance, you also need to delegate the methods in <span class="badge">Review</span> model:


<br>
{% highlight ruby %}

class Review < ActiveRecord::Base
  belongs_to :book

  delegate :name, :country, to: :book, prefix: 'book_author', allow_nil: true    
end

{% endhighlight %} 

<br>
If <span class="badge">Book</span> or <span class="badge">Author</span> object is nil, you will have <span class="badge">NoMethodError</span> raised. You avoid that by specifying <span class="badge">allow_nil: true</span> in <span class="badge">delegate</span> method.


<br>
Now you can easily use those methods from the <span class="badge">Review</span> instance:

{% highlight erb %}

<%= @review.book_author_name %>

<%= @review.book_author_country %>

{% endhighlight %} 


<br>
NB: You could also use  <span class="badge">prefix: true</span> which gets class name provided to <span class="badge">:to</span> hash and uses it as a prefix. In this case that will result in:

{% highlight erb %}

<%= @review.book_name %>

<%= @review.book_country %>

{% endhighlight %} 

Not really good for this example hence why <span class="badge">prefix: 'book_author'</span> is preferable.



<br>
In other cases, you would be tempted to access information across models by chaining together methods such as <span class="badge">@author.book.all</span>

Again, 2 dots are too many.

You would need to create a helper method in the <span class="badge">Author</span> class that would achieve this for you.

{% highlight ruby %}

class Author < ActiveRecord::Base
  has_many :books
  
  def all_books
    self.books.all
  end  
end

{% endhighlight %} 


<br>
Now you would be able to adhere to the design priciple by only having to use one dot to access the same information:

{% highlight ruby %}

<%= @author.all_books %>

{% endhighlight %}


<br><br>
### 3. Use <span class="badge">?</span> At the End of Method Name If It Is Returning Boolean Value

{% highlight ruby %}

def all_orders_processed?
  #...
end

{% endhighlight %}


<br><br>
### 4. Declare Instance Variables Inside the Action

Instance variables should not be placed in private methods but rather declared inside the action. 
This increases readability and eliminates confusion.

{% highlight ruby %}

before_filter :get_book

  def show
    @book = get_book
    @reviews = @book.reviews
  end
  
private
  def get_book
    Book.find(params[:id])
  end


###### Don't do this:

before_filter :get_book

  def show
    @reviews = @book.reviews
  end
  
private
  def get_book
    @book = Book.find(params[:id])
  end  
  
{% endhighlight %}



<br>
### 5. Make Helper Methods for Views

Avoid filling the views with calculations but when it's unavoidable, do the processing with helpers.

Don't do this:

{% highlight erb %}

<%= f.select( :genre, ['Literary Fiction', 'Mystery', 'Thriller', 'Horror', 'Historical', 'Romance', 'Western'].collect{|genre| [genre, genre]}) %>

{% endhighlight %}

<br>
Do this instead:

{% highlight erb %}

<%= f.select( :genre, genre_names.collect{|genre|  [genre, genre]}) %>

{% endhighlight %}


<br>
And create a helper:
{% highlight ruby %}

  def  genre_names
    ['Literary Fiction', 'Mystery', 'Thriller', 'Horror', 'Historical', 'Romance', 'Western']
  end

{% endhighlight %}


<br>
The same technique can be applied for conditional displays:

Don't do this:

{% highlight erb %}

<% case @filter %>
<% when 'inbox' %>
    <%= render 'inbox'%>
<% when 'sent' %>
    <%= render 'sent' %>
<% when 'draft' %>
    <%= render 'draft' %>
<% when 'trash'%>
    <%= render 'trash' %>
<% end %>

{% endhighlight %}


<br>
Do this instead:

{% highlight erb %}

<%= render filter_templates(@filter) %>

{% endhighlight %}


<br>
And create a helper:
{% highlight ruby %}

  def filter_templates(filter)
    case filter
    when 'inbox'
        render 'inbox'
    when 'sent'
        render 'sent'
    when 'draft'
        render 'draft'
    when 'trash'
        render  'trash'
    end
  end

{% endhighlight %}


<br>
### 6. Keep it simple, stupid (KISS)

Keep it simple, stupid (KISS) is a design principle which states that designs and/or systems should be as simple as possible.
In modern era the principle has evolved to have less derogatory implications and if you so wish you may also refer to is as: Keep It Short and Simple or Keep It Short and Sweet.
 
The easier something is to understand and use, the more likely it is to be adopted and engaged with.
Simplicity should be a primary design goal to reduce complexity. It can be achieved by following the “Single Responsibility Principle” among other techniques.


<br>
### 7. Continuous Refactoring
Continuous improvement of a programs internal structure ensures that the product is being done according to best practice and does not degenerate to a patch work.
The process should be a constant and gradual improvement to your codebase.


<br>
### 8. Prevent SQL Injection
Do not supply user input as a database query without escaping quotes.
If the user passes a single quote in an input text, then the text after the single quote character is considered to be an SQL statement. This means that the text would have direct access to the database, putting the entire database at risk as the user might have entered malicious content.

{% highlight ruby %}

# Dont do this
  Book.where("name = '#{params[:name]}'")
  
  Author.find_by("id = '#{params[:author_id]'")


# Instead, pass it as a parameter: 
  Book.where("name like ?", params[:name])
  
  Author.find_by(id: params[:author_id])
  
{% endhighlight %}


<br>
### 9. Write Tests

Testing is an absolute best practice in software development.
Running your Rails tests you can ensure your code adheres to the desired functionality even after some major code refactoring.

Rails tests can also simulate browser requests and thus you can test your application’s response without having to test it through your browser.
Tests also acts as detailed specifications of a feature or application and as documentation for other engineers, which helps them understand your intent in an implementation.


<br>
### 10. Make Use of Enums

An enum is an attribute where the values map to integers in the database and can be queried by name.

Say you wanted to store the <span class="badge">status</span> attribute of the book object as either completed, draft or published. You may do something like this: 

{% highlight ruby %}

  if book.status == "draft"
    # ...
  elsif book.status == "completed"
    # ...
  elsif book.status == "published"
    # ...
  end

# or you could do

  if book.status == 0 #draft
      # ...
  elsif book.status == 1 #completed
      # ...
  elsif book.status == 2 #published
      # ...
  end

{% endhighlight %}

Which is not very elegant.


A better approach would be to define an enum for the <span class="badge">status</span> attribute of <span class="badge">Book</span> object, where the possible values are draft, completed, or published. After refactoring the code, it would look like this:

{% highlight ruby %}

  enum status: { draft: 0, completed: 1, published: 2 }

  if book.draft?
      # ...
  elsif book.completed?
      # ...
  elsif book.published?
      # ...
  end
  
{% endhighlight %}

Ruby on Rails also provides a helper for updating the enum value. Instead of <span class="badge">Book.update(status: :published)</span>, we can use:
<span class="badge">Book.published!</span> 

You can also generate a scope like so: <span class="badge">Book.draft</span> instead of doing <span class="badge">Book.where(status: 'draft')</span>

Lastly, you could make the scope more intuitive by adding the <span class="badge">_prefix: true</span> or <span class="badge">_suffix: true</span> option:

{% highlight ruby %}

  enum status: { draft: 0, completed: 1, published: 2 }, _prefix: true

  Book.status_completed?  # status == 'completed'
  
  Book.status_published!  # update(status: :published)
  
  Book.status_draft  # Book.where(status: :draft)
  
{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
