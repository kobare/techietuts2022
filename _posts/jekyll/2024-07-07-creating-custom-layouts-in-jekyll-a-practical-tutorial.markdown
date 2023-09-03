---
layout: post
title:  "Creating Custom Layouts in Jekyll: A Practical Tutorial"
author: Denis Kobare
date:   2024-07-07 15:30:00 +0300
img: /assets/img/svg/jekyll.svg
categories: frameworks
sub_category: jekyll
type: concepts
technology: Ruby
permalink: "category/:categories/jekyll/concepts/:year:month/:title"
---


### Introduction

Jekyll is a popular static site generator that allows you to build simple, fast, 
and easy-to-maintain websites. One of its powerful features is the ability to 
create custom layouts, which helps maintain a consistent design across your site 
while keeping your code organized and DRY (Don't Repeat Yourself). In this 
section, we'll dive deeper into creating custom layouts in Jekyll, exploring the 
layout inheritance system, creating reusable layouts, and applying them to 
different pages. Let's get started!



<br>
### Understanding Layouts in Jekyll

In Jekyll, layouts are templates that define the structure of a page. By using 
layouts, you can separate the content of your site from its presentation, making 
your code cleaner and more maintainable. Jekyll uses the Liquid template engine, 
which allows you to include dynamic content in your layouts.



<br>
### Creating a Basic Layout

To create a custom layout in Jekyll, follow these steps:

- Navigate to the _layouts directory: Inside your Jekyll project, you'll find a 
directory called _layouts. This is where your custom layout files will reside.

- Create a new layout file: Let's say you want to create a layout for blog posts. 
Create a new file in the _layouts directory, and name it post.html. This layout 
will be used for individual blog posts.

- Define the layout: In the post.html file, you'll define the structure of your 
layout. Here's a basic example:

{% highlight html %}

<!DOCTYPE html>
<html>
<head>
  <title>{ page.title }</title>
</head>
<body>
  <header>
    <h1>{ site.title }</h1>
  </header>
  <main>
    { content }
  </main>
  <footer>
    <p>&copy; { site.author } { "now" | date: "%Y" }</p>
  </footer>
</body>
</html>

{% endhighlight %}


In this example, we're using Liquid tags to insert dynamic content. 
{{ page.title }} represents the title of the page, {{ site.title }} represents 
the title of your site, and { content } will be replaced with the content of 
each individual blog post.

- Apply the layout: To use this layout for your blog posts, you need to specify 
it in the front matter of each blog post file. Here's an example:


{% highlight html %}

---
 layout: post
 title: "My Awesome Blog Post"
---


This is the content of my blog post.

{% endhighlight %}


The layout: post line in the front matter tells Jekyll to use the post.html 
layout for this specific blog post.


<br>
### Layout Inheritance

Jekyll's layout inheritance system allows you to create a hierarchy of layouts, 
making it easy to reuse common elements across different pages. Let's extend our 
previous example by creating a layout for the homepage.

- Create a new layout file: In the _layouts directory, create a file named 
default.html. This will be the base layout that other layouts can inherit from. 
Here's an example:

{% highlight html %}

<!DOCTYPE html>
<html>
<head>
  <title>{ page.title }</title>
</head>
<body>
  <header>
    <h1>{ site.title }</h1>
  </header>
  <main>
    { content }
  </main>
  <footer>
    <p>&copy; { site.author } { "now" | date: "%Y" }</p>
  </footer>
</body>
</html>

{% endhighlight %}


- Modify the post layout: Update the post.html layout to inherit from the 
default.html layout. Here's the modified post.html:


{% highlight html %}

---
layout: default
---

<article>
  <h2>{ page.title }</h2>
  { content }
</article>

{% endhighlight %}


Now, the post.html layout inherits the structure from default.html, and you only 
need to define the unique elements for blog posts.

- Apply the layouts: When you create a new blog post, you don't need to specify 
the layout as post anymore. The inheritance system takes care of it. Here's an 
example of a blog post file without layout specification:


{% highlight markdown %}

---
title: "My Awesome Blog Post"
---

This is the content of my blog post.

{% endhighlight %}



<br>
### Conclusion

Creating custom layouts in Jekyll is a powerful way to maintain a consistent 
design across your site and keep your code organized. By understanding layout 
inheritance and using reusable layouts, you can streamline your development 
process and focus on creating great content. Start experimenting with custom 
layouts in Jekyll, and you'll discover how it enhances the flexibility and 
maintainability of your static site. Happy coding!



<br>
*Thanks for reading, see you in the next one!*
