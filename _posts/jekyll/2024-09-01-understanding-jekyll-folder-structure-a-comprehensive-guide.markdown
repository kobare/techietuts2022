---
layout: post
title:  "Understanding Jekyll Folder Structure: A Comprehensive Guide"
author: Denis Kobare
date:   2024-09-01 15:30:00 +0300
img: /assets/img/svg/jekyll.svg
categories: frameworks
sub_category: jekyll
type: concepts
technology: Ruby
permalink: "category/:categories/jekyll/concepts/:year:month/:title"
---


### Introduction

Jekyll, a static site generator, has gained popularity among developers and 
content creators for its simplicity and efficiency in building websites. At the 
core of a Jekyll project lies its folder structure, which plays a pivotal role 
in organizing content, templates, and assets. In this section, we will delve 
into the intricacies of the Jekyll folder structure, providing a comprehensive 
explanation of each directory's purpose and how they collectively contribute to 
the creation of a functional and aesthetically appealing website.


<br>
### Introduction to Jekyll

Jekyll is a static site generator that transforms plain text files into fully 
functional websites. It eliminates the need for complex databases and 
server-side processing, making websites faster and more secure. To understand 
how Jekyll works, one must familiarize themselves with its folder structure, as 
it forms the backbone of a Jekyll project.




<br>
### Jekyll Folder Structure

#### _layouts

The _layouts directory houses template files that define the structure of 
different pages on your site. Layouts are essential for maintaining consistency 
across multiple pages. The default.html layout, for instance, might contain the 
header, footer, and navigation common to every page. To create a new layout, add 
an HTML file in this directory and include placeholders for content that will 
vary between pages.

{% highlight html %}

<!-- _layouts/default.html -->
<html>
<head>
   <!-- meta tags, CSS links, etc. -->
</head>
<body>
   % include header.html %
   { content }
   % include footer.html %
</body>
</html>

{% endhighlight %}


<br>
#### _includes

Reusable components, such as navigation menus, headers, and footers, can be 
stored in the _includes folder. These components can then be easily included 
within layout files using Liquid tags.

{% highlight liquid %}

<!-- _layouts/default.html -->
<body>
   % include header.html %
   { content }
   % include footer.html %
</body>

{% endhighlight %}


<br>
#### _posts

The _posts directory contains markdown or HTML files representing individual 
posts. These files follow a specific naming convention: YYYY-MM-DD-title.md. 
Jekyll automatically converts these files into HTML pages and sorts them based 
on their date.


<br>
#### _data

The _data folder lets you store YAML or JSON files that can be accessed using 
Liquid tags. This is useful for separating data from your layout and content 
files, promoting a cleaner and more organized structure.



<br>
#### _sass and assets

The _sass directory stores Sass (Syntactically Awesome Style Sheets) files, 
which allow for more efficient and maintainable CSS. Jekyll will automatically 
convert Sass files into CSS when the site is built. The assets directory, on the 
other hand, contains static files like images, JavaScript, and CSS that will be 
copied over to the generated site.


<br>
#### _config.yml

This YAML configuration file contains various settings for your Jekyll site, 
including site title, description, and plugins. It's the central hub for 
customizing how your site is generated.



<br>
#### index.html and Other Pages

Pages like index.html, about.html, and more, reside in the root directory. These 
pages utilize layouts and include content from _posts, _includes, and _data as 
necessary.



<br>
### Building a Jekyll Site Step-by-Step

#### Installation and Setup

- Install Jekyll using the command: gem install jekyll.

- Create a new Jekyll site: jekyll new mysite.

- Navigate to the project folder: cd mysite.


<br>
#### Creating Layouts

- Inside _layouts, create a new HTML file for your layout (e.g., default.html).

- Define the basic structure of your layout, including placeholders for header, 
footer, and content.

- Save the file.


<br>
#### Writing Posts

- Add markdown or HTML files to _posts, following the naming convention: 
YYYY-MM-DD-title.md.
- Write your post using markdown or HTML syntax.

- Include front matter at the beginning of each post to define metadata like 
title and date.


<br>
#### Styling with Sass

- Utilize the _sass directory to create modular Sass files.

- Import your Sass files into a main styles.scss file.

- Jekyll will compile Sass into CSS during site generation.



<br>
### Conclusion

Understanding the Jekyll folder structure is essential for harnessing the full 
potential of this static site generator. By organizing your content, layouts, 
and assets in a systematic manner, you can create efficient, visually appealing, 
and easily maintainable websites. Remember, Jekyll's simplicity lies not only in 
its functionality but also in its structured approach to web development.



<br>
*Thanks for reading, see you in the next one!*
