---
layout: post
title:  "Devise Cheatsheet"
author: Denis Kobare
date:   2022-08-21 08:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails/Devise
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

1 . Redirecting User After Sign Up (Confirmation Pending)

If you want to redirect the user to a specific url after signing up, override the 
<span class="badge">after_inactive_sign_up_path_for</span> in the <span class="badge">registrations_controller</span>.

 <br>
 i). Create a new <span class="badge">registrations_controller.rb</span> in <span class="badge">app/controllers</span> directory.

{% highlight ruby %}
# app/controllers/registrations_controller.rb

class RegistrationsController < ::Devise::RegistrationsController
  layout false
  # the rest is inherited, so it should work
  
  private
  def after_inactive_sign_up_path_for(resource_or_scope)
    if resource_or_scope == :user || resource_or_scope.class == User || resource_or_scope == User
      your_pending_registrations_path
    #elsif resource_or_scope == :admin || resource_or_scope.class == AdminUser  || resource_or_scope == AdminUser
    #  admin_root_path
    else
      super
    end
  end
  
end

{% endhighlight %} 

 <br>
 ii). Override Devise default behaviour in the routes in <span class="badge">config/routes.rb</span>

{% highlight ruby %} 
# config/routes.rb
 devise_for :users, controllers: { registrations: 'registrations' }
 
{% endhighlight %}  


<br>
2 . Redirecting User From The Confirmation Email

You may want to redirect the user to a specific url after they clicked the link 
in the confirmation email. To do that, just override the 
<span class="badge">after_confirmation_path_for</span> in the <span class="badge">confirmations_controller</span>.

 <br>
 i). Create a new <span class="badge">confirmations_controller.rb</span> in <span class="badge">app/controllers</span> directory.

{% highlight ruby %}
# app/controllers/confirmations_controller.rb

class ConfirmationsController < Devise::ConfirmationsController
  private
  def after_confirmation_path_for(resource_name, resource)
    sign_in(resource) # In case you want to sign in the user
    # some_other_new_after_confirmation_path
  end
end
{% endhighlight %} 

 <br>
 ii). Override Devise default behaviour in the routes in <span class="badge">config/routes.rb</span>

{% highlight ruby %} 
# config/routes.rb
 devise_for :users, controllers: { confirmations: 'confirmations' }
{% endhighlight %}  


<br>
3 . Update User email without Sending a Confirmation Email
If you are using the <span class="badge">:reconfirmable</span> attribute and want 
to update a user's email without needing a confirmation email to be sent, you can
use the <span class="badge">skip_reconfirmation!</span> like so:

{% highlight rub %}

user = User.where(name: "techietuts")
user.email = 'techie@techietuts.com'
user.skip_reconfirmation!
user.save!

{% endhighlight %}


<br>
4 . Allowing Unconfirmed Users For A Given Period

You can give unconfirmed users some time to confirm their emails by setting the 
period for <span class="badge">allow_unconfirmed_access_for</span> in the <span class="badge">config/initializers/devise.rb</span> file.

{% highlight ruby %}
# config/initializers/devise.rb

config.allow_unconfirmed_access_for = 30.days

{% endhighlight %} 


<br>
5 . Skip Required Confirmation

You can skip required confirmation even if you've already added <span class="badge">confirmable</span> for devise.

{% highlight ruby %}

# in User.rb
protected

def confirmation_required?
  false
end

{% endhighlight %} 


<br>
6 . Allow Users to Login and Confirm their Emails Later

 i). Enable <span class="badge">reconfirmable</span> and set <span class="badge">allow_unconfirmed_access_for</span> in <span class="badge">config/initializers/devise.rb</span> file.

{% highlight ruby %}
# config/initializers/devise.rb

config.reconfirmable = true
config.allow_unconfirmed_access_for = 30.days

{% endhighlight %} 

 <br>
 ii). Create the column in the users table.

{% highlight ruby %}
# migrations/[...]devise_create_users
# ...

t.string   :unconfirmed_email # Only if using reconfirmable

# ...
{% endhighlight %} 
 
 <br>
 iii). Send users confirmation email like so:
 
{% highlight ruby %}
 
 User.find_each { |user| user.send_confirmation_instructions } 
       
{% endhighlight %} 

 
<br>
*Thanks for reading, see you in the next one!*
