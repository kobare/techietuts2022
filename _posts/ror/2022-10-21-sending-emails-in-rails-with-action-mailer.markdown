---
layout: post
title:  "Sending Emails in Rails With ActionMailer"
author: Denis Kobare
date:   2022-10-23 07:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Definition
Sending emails from a Ruby on Rails app is really easy. You won't even need a gem to do it!
Unless you are too lazy. 

According to stats from various research findings (which won't be cited), email has higher conversion 
rates than social media and search engine results combined. Hence why in this section, 
we'll create a simple mail app that we can use to send cold sales emails (HTML) 
to multiple prospects at a go and hopefully convert them into clients 🤑️


<br>
### Part A: Creating a Rails 7 Project 

1 . Create a new rails project called <span class="badge">emails_example</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.

<br>
2 . Install bootstrap icons

{% highlight terminal %}

$ npm i bootstrap-icons

{% endhighlight %}


<br>
### Part B: Configuring ActionMailer and Gmail 

Set up <span class="badge">ActionMailer</span> to work with <span class="badge">gmail</span> by following this document: [Configuring ActionMailer and Gmail](/category/frameworks/ruby-on-rails/tutorials/202210/configuring-action-mailer-and-gmail){:target="_blank"}.


<br>
### Part C: Creating the model

1 . Create the <span class="badge">prospect.rb</span> model.

 i). Generate the model
 
{% highlight terminal %}

 $ rails g model prospect
 
{% endhighlight %}
 

 <br>
 ii). Edit the file
  
{% highlight ruby %}
# models/prospect.rb

class Prospect < ApplicationRecord

    validates_presence_of :name, :email
  
end

{% endhighlight %}


<br>
2 . Create migration

 i). Edit the migrations

{% highlight ruby %}
# db/migrate/20221002112836_create_prospects.rb

class CreateProspects < ActiveRecord::Migration[7.0]
  def change
    create_table :prospects do |t|
      t.string :name     
      t.string :email             
      t.integer :email_count       
      t.datetime :last_mailed_at    

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
### Part D: Creating the Controllers 

1 . Create the prospects controller

i). Generate the prospects controller

{% highlight terminal %} 

$ rails g controller prospects

{% endhighlight %} 


<br>
ii). Replace the code in the file with this code

{% highlight ruby %} 
# controllers/prospects_controller.rb

class ProspectsController < ApplicationController

  before_action :set_prospect, only: [:show, :edit, :update, :destroy]


  def index
    if params[:search].present?
    @search = params[:search]
    @prospects = Prospect.where("prospects.name like '%#{@search}%'").page(params[:page]).per(500)
    else
    @prospects = Prospect.page(params[:page]).per(500)
    end
  end


  def show
  
  end


  def new
    @prospect = Prospect.new
  end


  def edit
    @prospect = Prospect.find(params[:id])
  end


  def create
    @prospect = Prospect.new(prospect_params)

    respond_to do |format|
      if @prospect.save
        format.html { redirect_to @prospect, notice: 'Prospect was successfully created.' }
        format.json { render action: 'show', status: :created, location: @prospect }
      else
        format.html { render action: 'new' }
        format.json { render json: @prospect.errors, status: :unprocessable_entity }
      end
    end
  end


  def update
    respond_to do |format|
      if @prospect.update(prospect_params)
        format.html { redirect_to @prospect, notice: 'Prospect was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: 'edit' }
        format.json { render json: @prospect.errors, status: :unprocessable_entity }
      end
    end
  end


  def destroy
    @prospect.destroy
    respond_to do |format|
      format.html { redirect_to prospects_url, notice: 'Prospect was successfully deleted.'  }
      format.json { head :no_content }
    end
  end


  private
    # Use callbacks to share common setup or constraints between actions.
    def set_prospect
      @prospect = Prospect.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def prospect_params
      params.require(:prospect).permit(:name, :email)
    end
end

{% endhighlight %} 
   

<br>
2 . Create the emails controller

i). Generate the emails controller

{% highlight terminal %} 

$ rails g controller emails

{% endhighlight %} 


<br>
ii). Replace the code in the file with this code

{% highlight ruby %} 
# controllers/emails_controller.rb

class EmailsController < ApplicationController

  before_action :set_prospect_ids, only: [:send_to_prospects]

  def index
    @prospects = Prospect.all
  end
  
  
  def send_to_prospects(prospect_ids = @prospect_ids)
  
    if @prospect_ids.present?
    
      @recipients = Prospect.all.where(id: @prospect_ids)
    
      @recipients.each do |recipient|
        MarketingMailer.new_marketing_email(recipient).deliver_later
        recipient.update(email_count: (recipient.email_count + 1), last_mailed_at: Time.now)
      end
    
      render json: {title: "Emails Sent", message: "Successfully sent emails", code: 200 }    

    else
      render json: {title: "Not Sent", message: "Select at least one recipient", code: 400 }
    end
  end


  private
  
    def set_prospect_ids
      @prospect_ids = params[:prospect_ids]
    end
  
end

{% endhighlight %} 


<br>
3 . Edit the <span class="badge">routes.rb</span> file to ook like this

{% highlight ruby %}
# config/routes.rb

Rails.application.routes.draw do
  root "home#index"
  resources :prospects
  resources :emails do
    collection do
      get :send_to_prospects
    end
  end  
end

{% endhighlight %}


<br>
### Part E: Creating the Views 

1 . Edit the prospects view templates

i). Create the <span class="badge">new.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/prospects/new.html.erb -->

<div class="container mt-3">
  <div class ="row">
    <h1>New Prospect</h1>

    <%= render 'form' %>


    <div class ="col-1"><%=link_to "Back", prospects_path, class: "btn btn-primary btn-sm btn-info float-right" %></div>

  </div>
</div>

{% endhighlight %} 


<br>
ii). Create the <span class="badge">_form.html.erb</span> partial template and add this code

{% highlight html %} 
<!-- views/prospects/_form.html.erb -->

<div class="row col-lg-6">
  <%= simple_form_for([:main, @prospect]) do |f| %>
    <%= f.error_notification %>

    <div class="form-inputs">
      <%= f.input :name %>
      <%= f.input :email %>
    </div>

    <div class="form-actions">
      <%= f.button :submit, class: "btn btn-success", value: "Create Prospect" %>
    </div>
  <% end %>
</div>

{% endhighlight %} 


<br>
iii). Create the <span class="badge">edit.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/prospects/edit.html.erb -->

<div class="container mt-3"><br><br>
  <div class="row">
  <div><%=link_to "View", @prospect, class: "btn btn-primary btn-sm btn-info float-right" %></div>
    <h1 style="text-align: center">Editing Person</h1>
    
    <%= render "form", prospect: @prospect %>

  </div>
</div>

{% endhighlight %} 


<br>
iv). Create the <span class="badge">show.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/prospects/show.html.erb -->

<div class="col-md-4" style="padding-top: 70px; margin: auto">
  <table class="table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Email Count</th>
        <th>Last Mailed At</th>                        
      </tr>
    </thead>
    <tbody class="fields">
     <tr>
      <td><%= @prospect.name %></td>
      <td><%= @prospect.email %></td>
      <td><%= @prospect.email_count %></td>
      <td><%= @prospect.last_mailed_at.strftime("%d-%b-%Y at %I:%M%p")  %></td>      
     <tr>
    </tbody>
  </table>
  
  <div class ="container">
   <div class="pull-right">
   <%=link_to " Edit", edit_main_prospect_path, class: "btn btn-warning btn-sm float-left" %>
  </div>
 </div>
</div>

{% endhighlight %} 


<br>
v). Create the <span class="badge">index.html.erb</span> template and add this code

{% highlight html %} 
<!-- views/prospects/index.html.erb -->

<div class="row">
  <h2>List of Prospects</h2>
      <div>
      <%=link_to "New Prospect", new_main_prospect_path, class: "col-3 btn btn-info btn-sm float-right"%>
      </div>
      
      <%= form_tag("/prospects", method: "get") do %>
    <%= label_tag(:range, "Search:") %>
    <%= text_field_tag(:search) %>
  <% end %>

  <%if @search.present?%>
    <h3>Showing results for <%=@search%></h3>
  <%end%>

  <div class="table-responsive">
      <table class="table">
          <thead>
              <tr>
                  <th>#</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Email Count</th>
                  <th>Last Mailed At</th>                  
              </tr>
          </thead>
          <tbody>
            <%@prospects.each do |p|%>
              <tr>
                <td><%=p.id%></td>
                <td><%=p.name%></td>
                <td><%=p.email%></td>
                <td><%=p.email_count%></td>
                <td><%=p.last_mailed_at.strftime("%d-%b-%Y at %I:%M%p") if p.last_mailed_at %></td>
                
                <td><%=link_to "View", p, class: "btn btn-primary btn-sm"%></td>
                <td><%=link_to "Edit", edit_prospect_path(p), class: "btn btn-warning btn-sm" %></td>
                <td><%=link_to "Delete", p, class: "btn btn-danger btn-sm", method: :delete, data: { confirm: 'Are you sure?' } %></td>
              </tr>
            <%end%>
          </tbody>
      </table>
  </div>
</div>

{% endhighlight %} 


<br>
vi). Create the <span class="badge">index.html.erb</span> template in emails directory and add this code

{% highlight html %} 
<!-- views/emails/index.html.erb -->

<div class="row">
  <h2 class="text-center">Marketing Email List</h2>
  <a class="col-2 btn btn-info btn-sm bi bi-ui-checks reduce-icon-font float-right" id="select_all"> Select All</a>
  <div class="table-responsive"><br>
      <table class="table">
          <thead>
              <tr>
                  <th></th>              
                  <th>#</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Email Count</th>
                  <th>Last Mailed At</th>
              </tr>
          </thead>
          <tbody>
            <%@prospects.each do |p|%>
              <tr class="prospect">
                <td><input type="checkbox" value="<%=p.id%>"></td>
                <td><%=p.id%></td>
                <td><%=p.name%></td>
                <td><%=p.email%></td>
                <td><%=p.email_count%></td>
                <td><%=p.last_mailed_at.strftime("%d-%b-%Y at %I:%M%p") if p.last_mailed_at%></td>
              </tr>
            <%end%>
          </tbody>
      </table>
      <a class="btn btn-warning btn-sm bi bi-send-check-fill reduce-icon-font float-right" id="send_emails"> Send Email Blast</a>
      <a class="btn btn-primary btn-sm bi bi-search reduce-icon-font float-right" id="preview_email"> Preview Email</a>
      
  </div> 
</div>

{% endhighlight %} 


<br>
2 . Install simple_form

i). Add <span class="badge">simple_form</span> gem in Gemfile

{% highlight ruby %}
# Gemfile
# ...
 gem simple_form
 
{% endhighlight %} 


<br>
ii). Bundle up

{% highlight terminal %}

$ bundle install
 
{% endhighlight %} 


<br>
iii). Generate simple_form helpers

{% highlight terminal %}

$ rails g simple_form:install
 
{% endhighlight %} 


<br>
### Part F: Creating the JavaScript File 

1 . Create the <span class="badge">prospects.js</span> file in <span class="badge">javascript/src</span>
directory and add this code

{% highlight javascript %} 
// javascript/src/prospects.js

  toggleSelectAllProspects();
  
  $("#send_emails").click(function(){
  
      
    $.get("/emails/send_to_prospects", {prospect_ids: getSelectedRecipients()}, function(result){

      return swal({
        title: result.title,
        text: result.message,
        type: result.code!=200 ? "error" : "success",
        showCancelButton: false,
        animation: "slide-from-top"
      }, function(inputValue) {}); 
         
    });
  });
  
  
  $("#preview_email").click(function(){
    
    if ($.isNumeric(getSelectedRecipients()[0])){
     window.open("/rails/mailers/marketing_mailer/new_prospect_email.html?locale=en&prospect_id=" + getSelectedRecipients()[0])
    }
    
    else{
    
      return swal({
        title: "No Preview",
        text: "Select at least one recipient",
        type: "error",
        showCancelButton: false,
        animation: "slide-from-top"
      }, function(inputValue) {}); 
    
    }   
  });

  
  function toggleSelectAllProspects(){
    var checked_status = true;
    $('#select_all').click(function(){    
      $('.prospect input:checkbox').prop('checked', checked_status);
      checked_status = !checked_status;
    });
  }


  function getSelectedRecipients(){
    var recipientIDs = [];
    
    $('.prospect input:checked').each(function() {
      recipientIDs.push($(this).val());
    });
    return recipientIDs;
  }

{% endhighlight %} 


<br>
2 . Download these files: 
[Sweetalert (JS and CSS)](https://github.com/kobare/sweetalert){:target="_blank"} 
and save them in the javascript/src/vendor and assets/stylesheets/vendor directories respectively.


<br>
3 . Import the <span class="badge">sweetalert.min.js</span> and 
<span class="badge">prospects.js</span> files in <span class="badge">
javascript/application.js</span>

{% highlight javascript %} 
// javascript/application.js

// ...

import "./src/vendor/sweetalert.min"
import "./src/prospects"

{% endhighlight %} 


<br>
4 . Import the <span class="badge">sweetalert.css</span> file in <span class="badge">
assets/stylesheets/application.bootstrap.scss</span>

{% highlight css %} 
/* assets/stylesheets/application.bootstrap.scss */

/* ... */

@import "./vendor/sweetalert";

{% endhighlight %} 


<br>
### Part G: Creating the Mailer 

1 . Create the Marketing Mailer

i). Generate the <span class="badge">marketing.rb</span> mailer

{% highlight terminal %}
 
$ rails generate mailer MarketingMailer

{% endhighlight %} 


<br>
ii). Edit <span class="badge">application_mailer.rb</span> in app/mailers

{% highlight ruby %}
# app/mailers/application_mailer.rb
 
class ApplicationMailer < ActionMailer::Base
  default from: "your_id@gmail.com"
  layout "mailer"
end

{% endhighlight %} 


<br>
iii). Edit <span class="badge">marketing_mailer.rb</span> in app/mailers

{% highlight ruby %}
# app/mailers/marketing_mailer.rb
 
class MarketingMailer < ApplicationMailer

  def new_marketing_email(recipient)
    @recipient = recipient # We can call this variable in the 
                           # new_marketing_email.html.erb view. We can also 
                           # define more variables for the view. 
    mail(to: @recipient.email, subject: 'Write a catchy marketing title here')
  end
  
end

{% endhighlight %} 


<br>
2 . Create the email template in views


i). Create the <span class="badge">new_marketing_email.html.erb</span> template 
in views/marketing_mailer directory and add this code. Notice the name matches 
the action/method in <span class="badge">MarketingMailer.rb</span> file.

{% highlight html %} 
<!-- views/marketing_mailer/index.html.erb -->

<!doctype html>
<html lang="en-US">
 <head>
  <meta content="text/html; charset=utf-8" http-equiv="Content-Type" />
  <title>Cold Sales Email Template</title>
  <meta name="description" content="New Account Email Template.">
  <style type="text/css">
   a:hover {
    text-decoration: underline !important;
   }
  </style>
 </head>
 <body marginheight="0" topmargin="0" marginwidth="0" style="margin: 0px; background-color: #f2f3f8;" leftmargin="0">
  <!-- 100% body table -->
  <table cellspacing="0" border="0" cellpadding="0" width="100%" bgcolor="#f2f3f8" style="@import url(https://fonts.googleapis.com/css?family=Rubik:300,400,500,700|Open+Sans:300,400,600,700); font-family: 'Open Sans', sans-serif;">
   <tr>
    <td>
     <table style="background-color: #f2f3f8; max-width:670px; margin:0 auto;" width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
      <tr>
       <td style="height:80px;">&nbsp;</td>
      </tr>
      <tr>
       <td style="text-align:center;">
        <a href="https://techietuts.com" title="logo" target="_blank">
         <img style="border-radius: 20px;" width="300" src="https://techietuts.com/assets/img/logo_01.png" title="logo" alt="logo">
        </a>
       </td>
      </tr>
      <tr>
       <td style="height:20px;">&nbsp;</td>
      </tr>
      <tr>
       <td>
        <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0" style="max-width:670px; background:#fff; border-radius:3px; text-align:left;-webkit-box-shadow:0 6px 18px 0 rgba(0,0,0,.06);-moz-box-shadow:0 6px 18px 0 rgba(0,0,0,.06);box-shadow:0 6px 18px 0 rgba(0,0,0,.06);">
         <tr>
          <td style="height:40px;">&nbsp;</td>
         </tr>
         <tr>
          <td style="padding:0 35px;">
           <h1 style="color:#1e1e2d; font-weight:500; margin:0;font-size:32px;font-family:'Rubik',sans-serif;">Hi <%= @recipient.name %>, <br>
            <br>
           </h1>
           <p style="font-size:15px; color:#455056; margin:8px 0 0; line-height:24px;"> My name is [my name] and I head up business development efforts with [my company]. We recently launched a new platform that [one-sentence pitch]. <br>
            <br> I’d like to speak with someone from <%= @recipient.name %> who is responsible for [handling something that's relevant to my product]. <br>
            <br> I am taking an educated stab in the dark here, however, based on your online profile, you may be the right person to connect with or can at least point me in the right direction. <br>
            <br> If that’s you, are you open to a fifteen-minute call on [time and date] to discuss ways the [company name] platform can specifically help your business? If not you, can you please put me in touch with the right person? <br>
            <br> I appreciate the help! <br>
            <br> Best, <br>
            <br> [your name]
          </td>
         </tr>
         <tr>
          <td style="height:40px;">&nbsp;</td>
         </tr>
        </table>
       </td>
      </tr>
      <tr>
       <td style="height:20px;">&nbsp;</td>
      </tr>
      <tr>
       <td style="text-align:center;">
        <p style="font-size:14px; color:rgba(69, 80, 86, 0.7411764705882353); line-height:18px; margin:0 0 0;">&copy; <strong>www.techietuts.com</strong>
        </p>
       </td>
      </tr>
      <tr>
       <td style="height:80px;">&nbsp;</td>
      </tr>
     </table>
    </td>
   </tr>
  </table>
  <!--/100% body table-->
 </body>
</html>

{% endhighlight %} 


*NB: As a best practice, you can create a text version of the email in case the 
receiver doesn't use HTML email. This goes in the same folder and has the same 
file name but uses the <span class="badge">text.erb</span> extension instead of 
<span class="badge">html.erb</span>. But for this tutorial, we will rely on the HTML email.*


<br>
3 . Set up the email preview

i). Open the <span class="badge">marketing_mailer_preview.rb</span> file located 
in tests/mailer/previews and replace the code with this:  


{% highlight ruby %} 
# tests/mailer/previews/marketing_mailer_preview.rb
# Preview all emails at http://localhost:3000/rails/mailers/marketing_mailer

class MarketingMailerPreview < ActionMailer::Preview

  def new_prospect_email
    @recipient = Prospect.find(params[:prospect_id])
    
    MarketingMailer.new_marketing_email(@recipient)  
  end


end

{% endhighlight %} 


<br>
### Final Results
Navigate to http://localhost:3000/prospects/new and create new prospects. Then 
to http://localhost:3000/emails to send the emails.

<img class="zoom-on-hover mobile-image" srcset="
  {{site.baseurl}}/assets/img/posts/email_list.png 1.7x
" alt="missing image">

<br>
<img class="zoom-on-hover mobile-image" srcset="
  {{site.baseurl}}/assets/img/posts/cold_sales_email.png 2.7x
" alt="missing image">


<br>
*Thanks for reading, see you in the next one!*
