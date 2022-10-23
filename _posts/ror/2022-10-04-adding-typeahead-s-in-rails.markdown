---
layout: post
title:  "Intergrating Typeahead in Rails"
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


In this example, we will create a form for capturing names of people and their nationality.
The auto complete feature will be applied on the input for selecting a country.

We will have two tables(models): <span class="badge">persons</span> and <span class="badge">countries</span>. Let's get started!


<br>
### Part A: Creating a Rails 7 Project 

Create a new rails project called <span class="badge">typeahead_example</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.


<br>
### Part B: Creating the models

1 . Create the <span class="badge">person.rb</span> and <span class="badge">country.rb</span> models

 i). Generate the models
 
{% highlight terminal %}

 $ rails g model person
 
 $ rails g model country

{% endhighlight %}
 
   
 <br>
 ii). Edit the files
  
{% highlight ruby %}
# models/country.rb

class Country < ApplicationRecord

  has_many :persons
  
end

{% endhighlight %}


<br>
{% highlight ruby %}
# models/person.rb

class Person < ApplicationRecord
  belongs_to :country
  
  self.table_name = "persons"  # override rails naming convention

end
{% endhighlight %}


<br>
2 . Create migrations

 i). Edit the migrations

{% highlight ruby %}
# db/migrate/20221002112836_create_countries.rb

class CreateCountries < ActiveRecord::Migration[7.0]
  def change
    create_table :countries do |t|
      t.string :name

      t.timestamps
    end
  end
end

{% endhighlight %}


<br>
{% highlight ruby %}
# db/migrate/20221002114011_create_persons.rb

class CreatePersons < ActiveRecord::Migration[7.0]
  def change
    create_table :persons do |t|
      t.string :name
      t.boolean :male
      t.date :dob
      t.string :country_name            
      t.integer :country_id      

      t.timestamps
    end
    add_index :persons, :country_id    
  end
end

{% endhighlight %}


 <br>
 ii) Run migrations
 
{% highlight terminal %}

$ rails db:migrate

{% endhighlight %} 


<br>
3 . Create Database Seed Data

 i) Open the <span class="badge">seeds.rb</span> file in the db directory and add this data:
 
{% highlight ruby %}
# db/seeds.rb

Country.create([ 
  {name: 'Afghanistan'}, 
  {name: 'Åland Islands'}, 
  {name: 'Albania'}, 
  {name: 'Algeria'}, 
  {name: 'American Samoa'}, 
  {name: 'AndorrA'}, 
  {name: 'Angola'}, 
  {name: 'Anguilla'}, 
  {name: 'Antarctica'}, 
  {name: 'Antigua and Barbuda'}, 
  {name: 'Argentina'}, 
  {name: 'Armenia'}, 
  {name: 'Aruba'}, 
  {name: 'Australia'}, 
  {name: 'Austria'}, 
  {name: 'Azerbaijan'}, 
  {name: 'Bahamas'}, 
  {name: 'Bahrain'}, 
  {name: 'Bangladesh'}, 
  {name: 'Barbados'}, 
  {name: 'Belarus'}, 
  {name: 'Belgium'}, 
  {name: 'Belize'}, 
  {name: 'Benin'}, 
  {name: 'Bermuda'}, 
  {name: 'Bhutan'}, 
  {name: 'Bolivia'}, 
  {name: 'Bosnia and Herzegovina'}, 
  {name: 'Botswana'}, 
  {name: 'Bouvet Island'}, 
  {name: 'Brazil'}])


{% endhighlight %} 


 <br>
 ii) Seed the database
 
{% highlight terminal %}

$ rails db:seed

{% endhighlight %} 


<br>
### Part C: Creating the Controllers 

1 . Create the persons controller

i). Generate the persons controller

{% highlight terminal %} 

$ rails g controller persons

{% endhighlight %} 


<br>
ii). Replace the code in the file with this code

{% highlight ruby %} 
# controllers/persons_controller.rb

class PersonsController < ApplicationController

  before_action :set_person, only: [:show, :edit, :update, :destroy]


  def index
    @persons = Person.all
  end
  
  
  def new
    @person = Person.new
  end


  def edit 
    @person = Person.find(params[:id])
  end
 
 
  def update
    respond_to do |format|
      if @person.update(person_params)
        format.html { redirect_to [:main, @person], notice: 'Person was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: 'edit' }
        format.json { render json: @person.errors, status: :unprocessable_entity }
      end
    end
  end
  
  
  def create
  
    ActiveRecord::Base.transaction do 
      
      person = params[:person_id] == "new" ? Person.new : Person.find(params[:person_id])
      person.name = params[:person_name]
      person.male = params[:gender]      
      person.dob = params[:dob] 
      person.country_id = params[:country_id]
      person.country_name = params[:country_name]  
    
      if person.save!
        render json: {id: person.id}
        flash[:notice] = "Person successfully saved."
      else
        render json: false
      end
              
    end    
  end


  def destroy
    Person.find(params[:id]).destroy
    redirect_to persons_path, notice: "Deleted Successfully."
  end

   
  private 
      # Use callbacks to share common setup or constraints between actions.
    def set_person
      @person = Person.find(params[:id])
    end


   def person_params
     params.require(:person).permit(:name, :male, :dob, :country_id)
   end
  
end

{% endhighlight %} 


<br>
2 . Create the countries controller

i). Generate the countries controller

{% highlight terminal %} 

$ rails g controller countries

{% endhighlight %} 


<br>
ii). Replace the code in the file with this code

{% highlight ruby %} 
# controllers/countries_controller.rb

class CountriesController < ApplicationController


  def index
    @countries = Country.where('name like ?', "%#{params[:search]}%")
    json = @countries.map { |c| {name: c.name, id: c.id}}
    respond_to do |format|
      format.html
      format.json { render json: json.to_json }
    end
  end


  def show
    @country = Country.find(params[:id])
    respond_to do |format|
      format.html
      format.json { render json: @country.to_json }
    end
  end

  
end

{% endhighlight %} 


<br>
3 . Edit the <span class="badge">routes.rb</span> file to ook like this

{% highlight ruby %}
# config/routes.rb

Rails.application.routes.draw do
  root "home#index"
  resources :persons
  resources :countries  
end

{% endhighlight %}


<br>
### Part D: Creating the Views 

1 . Edit the persons view templates

i). Create the <span class="badge">new.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/persons/new.html.erb -->

<div class="container mt-3">
  <div class ="row">
    <h1>New Person</h1>

    <%= render 'form' %>


    <div class ="col-1"><%=link_to "Back", persons_path, class: "btn btn-primary btn-sm btn-info pull-right" %></div>

  </div>
</div>

{% endhighlight %} 


<br>
ii). Create the <span class="badge">_form.html.erb</span> partial template and add this code

{% highlight html %} 
<!-- views/persons/_form.html.erb -->

<div class="col-md-6" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Gender</th>
        <th>DoB</th>  
        <th>Country</th>                      
      </tr>
    </thead>
    <tbody class="fields">
     <tr class="line-item">
      <% if @person.persisted? %>
      <td>
       <input type="hidden" id="person_id" value="<%= @person.id %>">
       <input type="text" id="person_name" value="<%= @person.name %>" class="form-control">   
      </td>
      <td>
       <select id="gender" class="form-control">
        <option value="<%= @person.male %>"><%= @person.male? ? "Male" : "Female" %></option>
        <option value="<%= @person.male? ? 0 : 1 %>"><%= @person.male? ? "Female" : "Male" %></option>        
       </select>
      </td>          
      <td>
       <input type="date" id="dob" value="<%= @person.dob %>" class="form-control">   
      </td>
      <td>
       <input type="hidden" id="country_id" value="<%= @person.country_id %>">         
       <input type="text" id="country_name" autocomplete="off" value="<%= @person.country_name %>" class="form-control">   
      </td>     
      <% else %>
      <td>
       <input type="hidden" id="person_id" value="new">
       <input type="text" id="person_name" class="form-control">
      </td>
      <td>
       <select id="gender" class="form-control">
        <option value="1">Male</option>
        <option value="0">Female</option>        
       </select>
      </td>          
      <td>
       <input type="date" id="dob" class="form-control">   
      </td>      
      <td>
       <input type="hidden" id="country_id">         
       <input type="text" id="country_name" class="form-control" autocomplete="off">   
      </td> 
      <% end %>
     <tr>
    </tbody>
  </table>

 <div class="mt-4">
  <button type="button" class="btn btn-success btn-sm person float-right" id="save_person">Save</button> 
 </div>
 </div>

</div>

{% endhighlight %} 


<br>
iii). Create the <span class="badge">edit.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/persons/edit.html.erb -->

<div class="container mt-3"><br><br>
  <div class="row">
  <div><%=link_to "View", @person, class: "btn btn-primary btn-sm btn-info pull-right" %></div>
    <h1 style="text-align: center">Editing Person</h1>
    
    <%= render "form", person: @person %>

  </div>
</div>

{% endhighlight %} 


<br>
iv). Create the <span class="badge">show.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/persons/show.html.erb -->

<div class="col-md-4" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Gender</th>
        <th>DoB</th>
        <th>Country</th>                        
      </tr>
    </thead>
    <tbody class="fields">
     <tr>
      <td><%= @person.name %></td>
      <td><%= @person.male? ? "Male" : "Female" %></td>
      <td><%= @person.dob.strftime('%d-%b-%Y') %></td>
      <td><%= @person.country_name %></td>      
     <tr>
    </tbody>
  </table>
  
  <div class ="container">
   <div class="pull-right">
  <a class="btn btn-primary person add-row pull-left" href="/persons/<%=@person.id%>/edit">
    <span class="icon"><i class="fa fa-pen"></i></span>
    <span>Edit</span>
  </a>
  </div>

 <div class="mt-4">
  <button type="button" class="btn btn-success btn-sm person float-right" id="save_person">Save</button> 
 </div>
 </div>
</div>

{% endhighlight %} 


<br>
v). Create the <span class="badge">index.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/persons/index.html.erb -->

<div class="col-md-4" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Gender</th>
        <th>DoB</th>
        <th>Country</th>
        <th></th>
        <th></th>
        <th></th>                                                                                                                        
      </tr>
    </thead>
    <tbody class="fields">
     <% @persons.each do |p| %>
     <tr>
      <td><%= p.name %></td>
      <td><%= p.male? ? "Male" : "Female" %></td>
      <td><%= p.dob.strftime('%d-%b-%Y') %></td>
      <td><%= p.country_name %></td>
      <td><%=link_to "View", [p], class: "btn btn-primary btn-xs", style: "background: #2c3e50;" %></td>
      <td><%=link_to "Edit", edit_person_path(p), class: "btn btn-default btn-xs" %></td>
      <td><%=link_to "Delete", [p], class: "btn btn-danger btn-xs", style: "background: #c0392b;", method: :delete, data: { confirm: 'Are you sure?' } %></td>
     <tr>
     <% end %>
    </tbody>
  </table>
  
</div>

{% endhighlight %} 


<br>
2 . Add fontawesome in the head tag of <span class="badge">application.html.erb</span>
 
{% highlight html %} 
<head>
<!-- views/application.html.erb -->

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css" integrity="sha512-xh6O/CkQoPOWDdYTDqeRdPCVd1SpvCA9XXcUnZS2FmJNp1coAFzvtCN9BmamE+4aHK8yyUHUSCcJHgXloTyT2A==" crossorigin="anonymous" referrerpolicy="no-referrer" />

<!-- ... -->    
</head>
<!-- ... -->

{% endhighlight %} 

<br>
### Part E: Creating the JavaScript File 

1 . Create the <span class="badge">persons.js</span> file in <span class="badge">javascript/src</span>
directory and add this code

{% highlight javascript %} 
// javascript/src/persons.js

jQuery(function() {

  var auto_complete_country;

  $("button#save_person").click(function(){

  personID = $("#person_id").val();
  personName = $("#person_name").val();
  gender = $("#gender").val();
  dob = $("#dob").val();
  countryID = $("#country_id").val();
  countryName = $("#country_name").val();
  

      $.post("/persons",
              {person_id: personID,
               person_name: personName,
               gender: gender,
               dob: dob,
               country_name: countryName,
               country_id: countryID,                 
               }, 
             function(result){
               console.log("Server Result: " + JSON.stringify(result));
             
               if(result == false){
                 window.location.href = "/persons/" 
               }
               else{
               window.location.href = "/persons/" + result.id + "/edit"
               };

      });
            
  });


    auto_complete_country = function(td) {
      td.find('#country_name').typeahead({
        name: "countries",
        remote: "/countries.json?search=%QUERY",
        valueKey: 'name',
        template: '<p><strong>{{name}}</strong></p>',
        engine: Hogan,
        limit: 20
      });
      
      return td.find('#country_name').on("typeahead:selected", function(obj, country) {
        td.find('#country_id').val(country.id);
      });     
      
    };

   auto_complete_country($('.fields').find('tr').find('td').next()); 
   
});    

{% endhighlight %} 


<br>
2 . Download these files: [Typeahead & Hogan](https://github.com/kobare/typeahead){:target="_blank"} and save them in the javascript/src/vendor directory.


<br>
3 . Import the <span class="badge">persons.js</span>, <span class="badge">hogan.js</span> and <span class="badge">typeahead.min.js</span> in <span class="badge">javascript/application.js</span>

{% highlight javascript %} 
// javascript/application.js

// ...

import typeahead from "./src/vendor/typeahead.min"
window.typeahead = typeahead

import Hogan from "./src/vendor/hogan.js"
window.Hogan = Hogan;

import "./src/persons"

{% endhighlight %} 



<br>
### Part E: Add Styling For Typeahead 

1 . Create the <span class="badge">typeahead.css</span> file in <span class="badge">app/assets/stylesheets</span>
directory and add this code

{% highlight css %} 

/* app/assets/stylesheets/typeahead.css */

.twitter-typeahead{
width:100%;
}

.twitter-typeahead .tt-query,
.twitter-typeahead .tt-hint {
  margin-bottom: 0;
}
.tt-dropdown-menu {
  min-width: 160px;
  margin-top: 2px;
  padding: 5px 0;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0,0,0,.2);
  *border-right-width: 2px;
  *border-bottom-width: 2px;
  -webkit-border-radius: 6px;
     -moz-border-radius: 6px;
          border-radius: 6px;
  -webkit-box-shadow: 0 5px 10px rgba(0,0,0,.2);
     -moz-box-shadow: 0 5px 10px rgba(0,0,0,.2);
          box-shadow: 0 5px 10px rgba(0,0,0,.2);
  -webkit-background-clip: padding-box;
     -moz-background-clip: padding;
          background-clip: padding-box;
  width:100%;        
}

.tt-suggestion {
  display: block;
  padding: 3px 20px;
}

.tt-suggestion.tt-is-under-cursor {
  color: #fff;
  background-color: #0081c2;
  background-image: -moz-linear-gradient(top, #0088cc, #0077b3);
  background-image: -webkit-gradient(linear, 0 0, 0 100%, from(#0088cc), to(#0077b3));
  background-image: -webkit-linear-gradient(top, #0088cc, #0077b3);
  background-image: -o-linear-gradient(top, #0088cc, #0077b3);
  background-image: linear-gradient(to bottom, #0088cc, #0077b3);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0088cc', endColorstr='#ff0077b3', GradientType=0)
}

.tt-suggestion.tt-is-under-cursor a {
  color: #fff;
}

.tt-suggestion p {
  margin: 0;
}


input.tt-hint {
  width: 115px !important;
}

{% endhighlight %} 


<br>
2 . Import the <span class="badge">typeahead.css</span> file in <span class="badge">app/assets/application.bootstrap.scss</span>

{% highlight javascript %} 
/* app/assets/stylesheets/application.bootstrap.scss */

@import 'typeahead';

{% endhighlight %} 



<br>
### Final Results
Navigate to http://localhost:3000/persons/new and test it.

<img class="zoom-on-hover mobile-image" srcset="
  {{site.baseurl}}/assets/img/posts/typeahead.png 1.1x
" alt="missing image">

<br>
<img class="zoom-on-hover mobile-image" srcset="
  {{site.baseurl}}/assets/img/posts/typeahead.gif 1.2x
" alt="missing image">


<br>
*Thanks for reading, see you in the next one!*
