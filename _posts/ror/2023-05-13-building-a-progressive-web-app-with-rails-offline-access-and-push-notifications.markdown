---
layout: post
title:  "Building a Progressive Web App (PWA) with Rails: Offline Access and Push Notifications"
author: Denis Kobare
date:   2023-05-13 13:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Introduction

In recent years, Progressive Web Apps (PWAs) have gained significant popularity 
among web developers and users alike. PWAs provide a seamless and engaging user 
experience, combining the best features of web and mobile applications. They can 
be accessed through web browsers and offer capabilities like offline access, push 
notifications, and device integration. In this tutorial, we will explore how to 
build a Progressive Web App with Rails, focusing on implementing PWA features 
such as offline access and push notifications.


<br>
### Benefits of Progressive Web Apps (PWAs)

Before diving into the implementation details, let's understand the benefits of 
building a Progressive Web App:


<br>
#### 1 . Cross-platform compatibility 
PWAs are built with web technologies such as HTML, CSS, and JavaScript, making 
them compatible across various platforms and devices. They work on both desktop 
and mobile devices, eliminating the need to develop separate applications for 
different platforms.


<br>
#### 2 . Enhanced user experience 
PWAs provide a native-like experience to users, with smooth animations, offline 
access, and push notifications. They load quickly and respond instantly, even on 
slower network connections, resulting in higher user engagement and satisfaction.


<br>
#### 3 . Offline access 
One of the standout features of PWAs is the ability to work offline. By utilizing 
a combination of service workers and caching, PWAs can store and display content 
even when the device is not connected to the internet. This ensures that users 
can access and interact with the app regardless of their network status.


<br>
#### 4 . Push notifications 
PWAs can send push notifications to users, allowing them to stay updated with new 
content, alerts, or important information even when the app is not open. Push 
notifications enable re-engagement and can significantly improve user retention 
and conversion rates.


<br>
#### 5 . App-like behavior 
PWAs can be installed on the user's home screen, just like native mobile 
applications, without the need to go through an app store. This provides easy 
access and increases the visibility of your app to users. Additionally, PWAs can 
access device features like cameras, geolocation, and sensors, enhancing their 
functionality and versatility.


Now that we understand the benefits of PWAs, let's move on to implementing PWA 
features in a Rails application.


<br>
### Implementing Offline Access

Offline access is a crucial feature of PWAs, as it allows users to access content 
even when they are not connected to the internet. To implement offline access in 
a Rails application, we'll utilize service workers and caching.


<br>
#### Step 1: Setting up a Service Worker

i). Create a new JavaScript file called service-worker.js in your Rails app's 
public directory. 

ii). Add the following code to the service-worker.js file to register 
the service worker:

<br>
{% highlight javascript %}

self.addEventListener('install', function(event) {
  event.waitUntil(
    caches.open('your-app-cache').then(function(cache) {
      return cache.addAll([
        '/assets/css/styles.css',
        '/assets/js/app.js',
        // Add other assets you want to cache
      ]);
    })
  );
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});

{% endhighlight %}


<br>
#### Step 2: Linking the Service Worker to Your Rails Application

i). In your Rails application layout file (e.g., application.html.erb), add the 
following code inside the <head> tag:


<br>
{% highlight javascript %}

<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/service-worker.js').then(function(registration) {
      console.log('Service Worker registered with scope:', registration.scope);
    }).catch(function(error
    console.log('Service Worker registration failed:', error);
    });
  }
</script>

{% endhighlight %}


<br>
#### Step 3: Configuring Caching

i). Open your Rails application's config/initializers/assets.rb file.
ii). Add the following code to configure caching for your assets:


<br>
{% highlight ruby %}

Rails.application.config.assets.precompile += %w( service-worker.js )

Rails.application.config.assets.configure do |env|
  env.register_transformer 'text/cache-manifest', 'application/x-cache-manifest', Sprockets::DirectiveProcessor
end

{% endhighlight %}


<br>
#### Step 4: Testing Offline Access

i). Start your Rails server by running rails server in your terminal.
ii). Open your application in a web browser.
iii). Use the browser's developer tools to simulate offline mode.
iv). Reload the page and verify that the cached assets are still being served.

<br>
Congratulations! You have successfully implemented offline access in your Rails 
application using service workers and caching.


    
<br>
### Implementing Push Notifications

Push notifications are a powerful way to engage and re-engage users with your PWA. 
To implement push notifications in a Rails application, we'll utilize the Web 
Push API and a service worker.


<br>
#### Step 1: Setting up Web Push

i). Generate VAPID keys by running the following command in your terminal:

<br>
{% highlight terminal %}

$ npx web-push generate-vapid-keys

{% endhighlight %}


<br>
ii). Take note of the generated public and private keys.


<br>
#### Step 2: Configuring the Rails Application

i). Install the webpush gem by adding it to your Gemfile and running bundle install.

ii). Create a new file called push_notifications_controller.rb and add the 
following code:


<br>
{% highlight ruby %}

class PushNotificationsController < ApplicationController
  def create
    subscription = params[:subscription]

    payload = {
      notification: {
        title: 'New Notification',
        body: 'This is a push notification from your PWA!',
        icon: '/assets/notification-icon.png'
      }
    }

    Webpush.payload_send(
      endpoint: subscription[:endpoint],
      message: JSON.generate(payload),
      p256dh: subscription[:keys][:p256dh],
      auth: subscription[:keys][:auth],
      vapid: {
        subject: 'mailto:your@email.com',
        public_key: 'YOUR_VAPID_PUBLIC_KEY',
        private_key: 'YOUR_VAPID_PRIVATE_KEY'
      }
    )
  end
end

{% endhighlight %}


<br>
Replace <span class="badge">YOUR_VAPID_PUBLIC_KEY</span> and 
<span class="badge">YOUR_VAPID_PRIVATE_KEY</span> with the respective keys 
generated in Step 1.


<br>
#### Step 3: Adding Push Notification UI

i). Create a view file called subscribe.html.erb and add the following code:
    
    
<br>
{% highlight html %}

<h1>Subscribe to Push Notifications</h1>

<button id="subscribe-button">Subscribe</button>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    var subscribeButton = document.getElementById('subscribe-button');

    subscribeButton.addEventListener('click', function() {
      if ('serviceWorker' in navigator && 'PushManager' in window) {
        navigator.serviceWorker.ready.then(function(serviceWorkerRegistration) {
          serviceWorkerRegistration.pushManager.subscribe({
            userVisibleOnly: true,
            applicationServerKey: 'YOUR_VAPID_PUBLIC_KEY'
          }).then(function(subscription) {
            fetch('/push_notifications', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'X-CSRF-Token': '<%= form_authenticity_token %>'
              },
              body: JSON.stringify({ subscription: subscription })
            });
          }).catch(function(error) {
            console.error('Error subscribing to push notifications:', error);
          });
        });
      }
    });
    });
  });
</script>
 
{% endhighlight %}


<br>
#### Step 4: Routing and CSRF Protection

i). Open your config/routes.rb file and add the following route:

<br>
{% highlight ruby %}

post '/push_notifications', to: 'push_notifications#create'

{% endhighlight %}


<br>
ii). Make sure you have the protect_from_forgery method enabled in your 
ApplicationController to provide CSRF protection.


<br>
#### Step 5: Testing Push Notifications

i). Start your Rails server by running rails server in your terminal.
ii). Open your application in a web browser.
iii). Navigate to the subscribe page (e.g., http://localhost:3000/subscribe).
iv). Click the "Subscribe" button to subscribe to push notifications.
v). Verify that the subscription details are being sent to the server and a push 
notification is triggered.

<br>
Congratulations! You have successfully implemented push notifications in your 
Rails application.


<br>
### Conclusion

In this tutorial, we explored the benefits of building a Progressive Web App with 
Rails and learned how to implement PWA features such as offline access and push 
notifications. By leveraging service workers, caching, and the Web Push API, we 
were able to provide an enhanced user experience with offline access and real-time 
push notifications. PWAs offer a powerful combination of web and mobile app 
capabilities, providing cross-platform compatibility and engaging user experiences. 
With the knowledge gained from this tutorial, you are now equipped to build your 
own Progressive Web App with Rails and take advantage of the benefits it offers. Happy coding!


<br>
*Thanks for reading, see you in the next one!*
