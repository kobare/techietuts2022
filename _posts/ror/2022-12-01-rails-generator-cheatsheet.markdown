---
layout: post
title:  "Rails Generator Cheatsheet"
author: Denis Kobare
date:   2022-12-01 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: cheatsheet
technology: Ruby
permalink: "category/:categories/ruby-on-rails/cheatsheet/:year:month/:title"
---

<link rel="stylesheet" href="/assets/css/table.css">

### Definition
Simple Form is a custom Rails form builder that consistently structures forms 
with a rich set of features. It is rather flexible and consequently easy to 
override layout options in a particular form or field.


<br>
### Installation

1 . Add gem to Gemfile

{% highlight ruby %}

#...

  gem simple_form

{% endhighlight  %}


<br>
2 . Bundle up

{% highlight terminal %}

$ bundle install

{% endhighlight  %}


<br>
3 . Generate helpers

{% highlight terminal %}

$ rails g simple_form:install

{% endhighlight  %}


<br>
### Usage Example

See this controller for context:

<br>
{% highlight ruby %}

class Lecturer::QuestionsController < Lecturer::ApplicationController
  
  
  def create
    @question = Question.new(question_params)

    respond_to do |format|
      if @question.save
        format.html { redirect_to [:lecturer, @question], notice: 'Question was successfully created.' }
        format.json { render action: 'show', status: :created, location: @question }
      else
        format.html { render action: 'new' }
        format.json { render json: @question.errors, status: :unprocessable_entity }
      end
    end
  end
  
  
  private
     
   def question_params
     params.require(:question).permit(:question_statement, :body, :image, pictures: [])
   end
  
end

{% endhighlight  %}


<br> 
{% highlight ruby %}

<%= simple_form_for([:lecturer, @question]) do |f| %>
 <%= f.error_notification %>
 
  <div>
   <%= f.label :question_statement, style: "display: block" %>
   <%= f.text_field :question_statement %>
  </div>
  
  <br>
  <div>
   <%= f.rich_text_area :body %>
  </div>

  <div>
   <%= f.label :image %>
   <%= f.file_field :image %>
  </div>

  <div>
   <%= f.label :pictures %>
   <%= f.file_field :pictures, multiple: true %>
  </div>

  <div class="form-actions">
    <%= f.button :submit, class: "btn btn-success" %>
  </div>


<!-- Note that a single line of code can define all of the form input field and its label. e.g: -->

   <%= f.input :question_statement, style: "display: block" %>

<% end %>

{% endhighlight %}
 

<br>
*Thanks for reading, see you in the next one!*
