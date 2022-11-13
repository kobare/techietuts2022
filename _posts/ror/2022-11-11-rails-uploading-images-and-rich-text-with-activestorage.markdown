---
layout: post
title:  "Rails Uploading Images And Rich Text With ActiveStorage"
author: Denis Kobare
date:   2024-11-11 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

<link rel="stylesheet" href="/assets/css/table.css">

### Definition
Callbacks are methods that are called at some points of an object's lifecycle.
They are basically code that will execute before we can respond to a request.
Callbacks are always called before particular controller actions (usually listed on top of the file). 
The reason for this is that you need to find an object from the database and assign 
it to an instance variable in all those actions. 

Callbacks are private because we dont want to access these actions outside the class.

A callback method is meant to keep the code DRY (Do not Repeat Yourself), because if you 
don't have a private method for this, you would have to copy/paste the line inside 
the method for each controller action. Which is fine as long as you don't need to 
change it EVER. That is why it is good practice to use callbacks for these kinds of methods.


<br>
### Example
 
{% highlight ruby %}

class ClientsController < ApplicationController

  before_action :set_client, only: [:show, :edit, :update, :destroy]


  def show
  
  end


  def edit

  end


  def update
    respond_to do |format|
      if @client.update(client_params)
        format.html { redirect_to @client, notice: 'Client was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: 'edit' }
        format.json { render json: @client.errors, status: :unprocessable_entity }
      end
    end
  end


  def destroy
    @client.destroy
    respond_to do |format|
      format.html { redirect_to clients_url, notice: 'Client was successfully deleted.'  }
      format.json { head :no_content }
    end
  end


  private
    # Use callbacks to share common setup or constraints between actions.
    def set_client
      @client = Client.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def client_params
      params.require(:client).permit(:name, :address, :phone, :email, :is_closed, :notes, :client_category_id, :sales_channel_id)
    end
end

{% endhighlight %}
 
   
<br>
### Callback Types

<div class="grid-container-x">
  <div class="grid-item-x item5">
   <h4 class="text-center grey-h">On Object Initialized</h4>
  </div>
  <div class="grid-item-x item2">
   <ul>
    <li>after_initialize</li>
   </ul>
  </div>  
  <div class="grid-item-x item1">
   <h4 class="text-center grey-h">Before Transaction is Committed</h4>  
  </div>
  <div class="grid-item-x item3">
   <ul>
    <li>before_validation</li>
    <li>after_validation</li>
    <li>before_save</li>
    <li>around_save</li>
   </ul>   
  </div>  
  <div class="grid-item-x item4">
   <ul>
    <li>after_save</li>   
    <li>before_create</li>
    <li>around_create</li>
    <li>after_create</li>
   </ul>  
  </div>
  <div class="grid-item-x item6">
   <h4 class="text-center grey-h">On Rollback</h4>
  </div> 
  <div class="grid-item-x item7">
   <ul>  
    <li>after_rollback</li>
   </ul>     
  </div>  
  <div class="grid-item-x item8">
   <h4 class="text-center grey-h">After Transaction is Committed Successfully</h4>
  </div>  
  <div class="grid-item-x item9">
   <ul>
    <li>after_create_commit</li>
    <li>after_commit</li>
   </ul>   
  </div>
  <div class="grid-item-x item9">
   <!--
   <ul>
    <li></li>   
   </ul>
    -->   
  </div>      
</div>



<br>
### Order of Operations

<div class="grid-container-x">
  <div class="grid-item-x item5">
   <h4 class="text-center grey-h">Creating an Object</h4>
  </div>
  <div class="grid-item-x item2">
   <ul>
    <li>before_validation</li>
    <li>after_validation</li>
    <li>before_save</li>    
    <li>around_save</li>                
    <li>before_create</li>
    <li>around_create</li>
    <li>after_create</li>    
    <li>after_save</li>                
    <li>after_commit/after_rollback</li>                
   </ul>
  </div>  

  <div class="grid-item-x item1">
   <h4 class="text-center grey-h">Updating an Object</h4>  
  </div>
  <div class="grid-item-x item3">
   <ul>
    <li>before_validation</li>
    <li>after_validation</li>
    <li>before_save</li>
    <li>around_save</li>
    <li>before_update</li>       
   </ul>   
  </div>  
  <div class="grid-item-x item4">
   <ul>
    <li>around_update</li>
    <li>after_update</li>
    <li>after_save</li>
    <li>after_commit/<br>after_rollback</li>    
   </ul>  
  </div>
  <div class="grid-item-x item6">
   <h4 class="text-center grey-h">Destroying an Object</h4>
  </div> 
  <div class="grid-item-x item7">
   <ul>  
    <li>before_destroy</li>
    <li>around_destroy</li>
    <li>after_destroy</li>    
    <li>after_commit/<br>after_rollback</li>    
   </ul>     
  </div>  
  <div class="grid-item-x item8">
   <h4 class="text-center grey-h"></h4>
  </div>  
  <div class="grid-item-x item9">
   <!--
   <ul>
    <li></li>
   </ul>
   -->   
  </div>
  <div class="grid-item-x item9">
   <!--
   <ul>
    <li></li>   
   </ul>
    -->   
  </div>      
</div>


<br>
*Thanks for reading, see you in the next one!*
