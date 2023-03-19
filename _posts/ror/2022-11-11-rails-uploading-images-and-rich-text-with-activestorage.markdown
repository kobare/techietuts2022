---
layout: post
title:  "Rails Uploading Images And Rich Text With ActiveStorage"
author: Denis Kobare
date:   2022-11-11 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---


### Introduction

Image upload in Rails is easy to implement with ActiveStorage.
We can also implement action_text for image uploading but also as a toolkit for 
handling rich text content & editing and file attachments in general.

In this section, we are going to be setting up ActiveStorage and action_text to 
upload images to our Rails application in simple steps.


<br>
### Prerequisites

<span class="badge">Rails 7.0.4</span><br>
<span class="badge">Ruby 3.2.0</span> 


<br>
### Part A: Install and Configure ActiveStorage

#### 1. Install active_storage and action_text

{% highlight terminal %}

$ rails active_storage:install;

$ rails g action_text:install;

$ rails db:migrate

{% endhighlight %}


  <br>
  ii). Uncomment image_processing gem in Gemfile 
  
{% highlight ruby %} 
#...

# Use Active Storage variants [https://guides.rubyonrails.org/active_storage_overview.html#transforming-images]
gem "image_processing", "~> 1.2"
  
{% endhighlight %}


  <br>
  iii). Bundle up
  
{% highlight terminal %} 

$ bundle install

{% endhighlight %}


<br>
#### 2. Install ImageMagick 

Active storage will require image processing to resize the images.
vips is the default image processor but we'll use imagemagick.

  <br>
  i) Switch image processing from vips to image_magick. Add this code in 
     <span class="badge">/config/environments/development.rb</span>


{% highlight ruby %}

#...

Rails.application.configure do
#...
      
  config.active_storage.variant_processor = :mini_magick 

end
     
{% endhighlight %}
 
  
  <br>
  ii) Install ImageMagick

The chances that ImageMagick has already been installed on your Ubuntu system 
are pretty high. Confirm this by checking its version:

{% highlight terminal %}
 
$ convert --version

{% endhighlight %}


<br>
You'll get an error message if it is not installed. Install it with the following command:

{% highlight terminal %}

$ sudo apt install imagemagick

{% endhighlight %}



<br>
### Part B: Usage Example

#### 1. Create a person scaffold

  i). Generate the scaffold

{% highlight terminal %}
   
$ rails g scaffold person name

{% endhighlight %}



  <br>
  ii). Add the route

{% highlight ruby %}
# config/routes.rb
#...

resources person

{% endhighlight %}


<br>
#### 2. Associating the Model With ActiveStorage

  i). Set up image upload for the <span class="badge">person.rb</span> model

{% highlight ruby %}
# models/person.rb
#...

  has_one_attached :image # if you want the object to have a single image
  has_many_attached :pictures # if you want the object to have multiple images
  has_rich_text :body # for rich texts that can also include images e.g blog posts

{% endhighlight %}


  <br>
  ii). White-list the image params in the <span class="badge">persons_controller.rb</span>:

{% highlight ruby %}
# controllers/persons_controller.rb
#...

   def person_params
     params.require(:person).permit(:name, :image, :body, pictures: [])

{% endhighlight %}


  <br>
  iii). Resizing The Images

Create a method for resizing the images in person.rb 

{% highlight ruby %}

# ...

  def resize_image_to_thumbnail
    return unless image.content_type.in?(%[image/jpeg image/png])
    image.variant(resize_to_limit: [300, 300]).processed
  end

  def resize_picture_to_thumbnail(pic)
    return unless pic.content_type.in?(%[image/jpeg image/png])
    pic.variant(resize_to_limit: [250, 250]).processed
  end

{% endhighlight %}


  <br>
  iv). Create the form

{% highlight html %} 

<%= simple_form_for(@person do |f| %>
 <%= f.error_notification %>
 
  <div>
   <%= f.label :name, style: "display: block" %>
   <%= f.text_field :name %>
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

<% end %>

{% endhighlight %}


  <br>
  v). Edit <span class="badge">show.html.erb</span> to resemble this:

{% highlight html %} 
<h2>Rich Text Area</h2>
<%= @question.body %>

<br>
<h2>Single Image</h2>
<% if @person.image.content_type.in?(%[image/jpeg image/png]) %>
  <%= link_to image_tag(@person.resize_image_to_thumbnail), person.image if person.image.attached? %>
<% end %>


<br>
<h2>Multiple Images</h2>
<% person.resize_picture_to_thumbnail.each do |picture| %>
<%= link_to image_tag(@person.resize_picture_to_thumbnail(picture)), picture %>
<% end %>

{% endhighlight %}



  <br>
  vi). Install simple_form gem
  
  Use [these](/category/frameworks/ruby-on-rails/tutorials/202211/rails-simple-form-gem-installation-and-usage){:target="_blank"} instructions to install simple_form

<br>  
That's all!


<br>
*Thanks for reading, see you in the next one!*
