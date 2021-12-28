---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---



<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no; maximum-scale=1.0; user-scalable=0;">




</head>
<body>

<div id="z-wrapper">
    <div class="home-banner xxzig-zag-bottom">
      <!-- <h1 style="padding: -50px">This example demonstrates how to hide a navbar when the user starts to scroll the page</h1> -->
      <h1>Let's code in Ruby, JS, CSS, Python and More...</h1>             
    </div>
</div>


<div class="home-content">

  <div class="content">
    <div class="blog-post blog">
      <h2 class="recent-header">Recent posts</h2>                
      <div class="grid-container">
        {% for post in site.posts limit: 20 %}
        <div class="grid-item">
          <div class="grid-image"><img src="{{ post.img }}"></div>
          <div class="grid-title">
            <p style="font-size: 18px; font-weight: 600; text-align: left;"><a href="{{ site_url }}{{ post.url | append: site.baseurl }}" style="color: inherit; text-decoration: none;">{{ post.title }}</a></p>
            <p style="font-size: 12px; font-weight: 600; text-align: left;"><i class="fa fa-user"></i> {{ post.author }} &nbsp; &nbsp;<i class="fa fa-calendar"></i> {{ post.date | date: "%B %Y" }}</p>
          </div>          
        </div>
        {% endfor %}                
      </div>

    </div>  
  </div>
</div>






</body>
</html>

