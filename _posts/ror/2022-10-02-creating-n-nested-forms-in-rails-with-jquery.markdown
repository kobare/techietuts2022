---
layout: post
title:  "Creating Nested Forms in Rails With Jquery"
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
A nested form is a form within a form. We use it to handle multiple models in a single form. 
In this example, we have a todo_list that contains many tasks. A nested form will allow us to
perform CRUD operations on the <span class="badge">todo_list</span> & <span class="badge">tasks</span> models within a single form.


<br>
### Prerequisites

1. Ruby 3.2.0
2. Rails 7.0.3


<br>
### Part A: Creating a Rails 7 Project 

Create a new rails project called <span class="badge">devise_email</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.


<br>
### Part B: Creating the models

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

  has_many :tasks, dependent: :destroy
  
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
      t.integer :todo_list_id      

      t.timestamps
    end
    add_index :tasks, :todo_list_id    
  end
end

{% endhighlight %}


 <br>
 ii) Run migrations
 
{% highlight terminal %}

$ rails db:migrate

{% endhighlight %} 


<br>
### Part C: Creating the Controller 

1 . Create the todo_lists controller

i). Generate the controller

{% highlight terminal %} 

$ rails g controller todo_lists

{% endhighlight %} 


<br>
ii). Replace the code in the file with this code

{% highlight ruby %} 
# controllers/todo_lists_controller.rb

class TodoListsController < ApplicationController

  before_action :set_todo_list, only: [:show, :edit, :update, :destroy]


  def index
    @todo_lists = TodoList.all
  end
  
  
  def new
    @todo_list = TodoList.new
  end


  def edit 
    @todo_list = TodoList.find(params[:id])
  end
 
 
  def update
    respond_to do |format|
      if @todo_list.update(todo_list_params)
        format.html { redirect_to [:main, @todo_list], notice: 'To Do List was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: 'edit' }
        format.json { render json: @todo_list.errors, status: :unprocessable_entity }
      end
    end
  end
  
  
  def create
  
    ActiveRecord::Base.transaction do 
      
      todo_list = params[:todo_list_id] == "new" ? TodoList.new : TodoList.find(params[:todo_list_id])
      todo_list.name = params[:todo_list_name]      
        
      if !params[:tasks].nil?
        todo_list.tasks.destroy_all if todo_list.persisted?
      end
        
      if !params[:tasks].nil?   
      params[:tasks].each do |weekly_attendance|
        atts = CGI::parse weekly_attendance
      
        new_task = todo_list.tasks.build(
          id: atts['task_id'][0],
          name: atts['task_name'][0],
          completed: atts['task_status'][0],
          date: atts['due_date'][0],
          todo_list_id: atts['todo_list_id'][0],          
          created_at: Time.now,
          updated_at: Time.now      
        )
      end
      end

    
      if todo_list.save!
        render json: {id: todo_list.id}
        flash[:notice] = "To Do List successfully saved."
      else
        render json: false
      end
              
    end    
  end


  def destroy
    TodoList.find(params[:id]).destroy
    redirect_to todo_lists_path, notice: "Deleted Successfully."
  end

   
  private 
      # Use callbacks to share common setup or constraints between actions.
    def set_todo_list
      @todo_list = TodoList.find(params[:id])
    end


   def todo_list_params
     params.require(:todo_list).permit(:name, :created_at, :updated_at)
   end
  
end

{% endhighlight %} 


<br>
2 . Edit the <span class="badge">routes.rb</span> file to ook like this

{% highlight ruby %}
# config/routes.rb

Rails.application.routes.draw do
  root "home#index"
  resources :todo_lists
end

{% endhighlight %}


<br>
### Part D: Creating the Views 

1 . Edit the view templates

i). Create the <span class="badge">new.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/todo_lists/new.html.erb -->

<div class="container mt-3">
  <div class ="row">
    <h1>New To Do List</h1>

    <%= render 'form' %>


    <div class ="col-1"><%=link_to "Back", todo_lists_path, class: "btn btn-primary btn-sm btn-info pull-right" %></div>

  </div>
</div>

{% endhighlight %} 


<br>
ii). Create the <span class="badge">_form.html.erb</span> partial template and add this code

{% highlight html %} 
<!-- views/todo_lists/_form.html.erb -->

<div class="col-md-6" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
      </tr>
    </thead>
    <tbody class="fields">
     <tr>
      <% if @todo_list.persisted? %>
      <td><input type="hidden" id="todo_list_id" value="<%= @todo_list.id %>"><input type="text" id="todo_list_name" value="<%= @todo_list.name %>"></td>
      <% else %>
      <td><input type="hidden" id="todo_list_id" value="new"><input type="text" id="todo_list_name"></td>      
      <% end %>
     <tr>
    </tbody>
  </table>


  <table class="table tasks">
    <thead>
      <tr>
        <th></th>                    
        <th>Task</th>              
        <th>Status</th>
        <th>Due</th>
      </tr>
    </thead>
    <tbody class="fields">
      <% @todo_list.tasks.each do |t| %>    
     <tr class="task-table-row">
      <td><center><button type="button" class="btn btn-danger btn-sm remove-row pull-left blockable"> <i class="fa fa-minus-circle"></i></button></center></td>      
      <td><input type="text" class="form-control" name="task_name" value="<%= t.name %>"></td>
      <td>
       <select readonly name="task_status" class="form-control">
        <option value="<%= t.completed %>"><%= t.completed? ? "Completed" : "Pending"%></option>
        <option value='<%= t.completed? ? 0 : 1 %>'><%= t.completed ? "Pending" : "Completed"%></option>
       </select>
      </td> 
      <td><input type="date" class="form-control" name="due_date" value="<%= t.date %>"></td>                                      
     </tr>
      <% end %>
    </tbody>
  </table>
  
  <div class ="container">
   <div class="pull-right">
  <a class="btn btn-primary task add-row pull-left">
    <span class="icon"><i class="fa fa-plus"></i></span>
    <span>Add Task</span>
  </a>
  </div>

 <div class="mt-4">
  <button type="button" class="btn btn-success btn-sm todo-list float-right" id="save_todo_list">Save</button> 
 </div>
 </div>

</div>

{% endhighlight %} 


<br>
iii). Create the <span class="badge">edit.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/todo_lists/edit.html.erb -->

<div class="container mt-3"><br><br>
  <div class="row">
  <div><%=link_to "View", [@todo_list ], class: "btn btn-primary btn-sm btn-info pull-right" %></div>
    <h1 style="text-align: center">Editing To Do List</h1>
    
    <%= render "form", todo_list: @todo_list %>

  </div>
</div>

{% endhighlight %} 


<br>
iv). Create the <span class="badge">show.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/todo_lists/show.html.erb -->

<div class="col-md-4" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
      </tr>
    </thead>
    <tbody class="fields">
     <tr>
      <td><%= @todo_list.name %></td>
     <tr>
    </tbody>
  </table>


  <table class="table tasks">
    <thead>
      <tr>
        <th>Task</th>              
        <th>Status</th>
        <th>Due</th>
      </tr>
    </thead>
    <tbody class="fields">
      <% @todo_list.tasks.each do |t| %>    
     <tr class="task-table-row">
      <td><%= t.name %></td>
      <td><%= t.completed? ? "Completed" : "Pending" %></td> 
      <td><%= t.date %></td>                                      
     </tr>
      <% end %>
    </tbody>
  </table>
  
  <div class ="container">
   <div class="pull-right">
  <a class="btn btn-primary task add-row pull-left" href="/todo_lists/<%=@todo_list.id%>/edit">
    <span class="icon"><i class="fa fa-pen"></i></span>
    <span>Edit</span>
  </a>
  </div>

 <div class="mt-4">
  <button type="button" class="btn btn-success btn-sm todo-list float-right" id="save_todo_list">Save</button> 
 </div>
 </div>
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

1 . Create the <span class="badge">todo_lists.js</span> file in <span class="badge">javascript/src</span>
directory and add this code

{% highlight javascript %} 
// javascript/todo_lists.js


  $("button#save_todo_list").click(function(){

  var tasksData = [];

  todoListID = $("#todo_list_id").val();
  todoListName = $("#todo_list_name").val();
  
  $(".task-table-row").each(function(index){
  
    var serialized_data = $(this).find('.form-control').serialize();
  
    tasksData.push(serialized_data);

  });

      $.post("/todo_lists",
              {tasks: tasksData, 
               todo_list_id: todoListID,
               todo_list_name: todoListName                 
               }, 
             function(result){
               console.log("Server Result: " + JSON.stringify(result));
             
               if(result == false){
                 window.location.href = "/todo_lists/" 
               }
               else{
               window.location.href = "/todo_lists/" + result.id + "/edit"
               };

      });
            
  });


    $(".task.add-row").click(function () {

      tableBody = $('table.tasks tbody');

      taskRow = "<tr class='task-table-row'><td><center><button type='button' class='btn btn-danger btn-sm remove-row pull-left'> <i class='fa fa-minus-circle'></i></button></center></td><td><input name='task_id' type='hidden' class='form-control'></input><input name='todo_list_id' type='hidden' class='form-control'></input><input type='text' name='task_name' class='form-control'></td><td><select name='task_status' class='form-control'><option value='1'>Completed</option><option value='0'>Pending</option></select></td><td><input type='date' name='due_date' class='form-control'></td></tr>";
    
      $(taskRow).appendTo(tableBody)
    });        


    $(document).on('click','.remove-row',function(){
        $(this).parents('tr').remove();
    });

{% endhighlight %} 


<br>
2 . Import the <span class="badge">todo_list.js</span> in <span class="badge">javascript/application.js</span>

{% highlight javascript %} 
// javascript/application.js

// ...

import "./src/todo_lists"


{% endhighlight %} 


<br>
### Final Results
Navigate to http://localhost:3000/todo_lists/new and test it.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/jquery_nested_forms_2.png 2.3x,
  {{site.baseurl}}/assets/img/posts/jquery_nested_forms_2.png 2.5x
" alt="missing image">

<br>
<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/jquery_nested_forms_1.gif 2.4x,
  {{site.baseurl}}/assets/img/posts/jquery_nested_forms_1.gif 3x
" alt="missing image">

<br>
*Thanks for reading, see you in the next one!*
