---
layout: post
title:  "Building Command-Line Applications with Ruby: A Step-by-Step Tutorial"
author: Denis Kobare
date:   2023-09-03 14:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: tutorial
technology: Ruby
permalink: "category/:categories/ruby/tutorial/:year:month/:title"
---


### Introduction

Command-line applications are powerful tools that allow users to interact with 
software through the terminal, making them an essential part of a developer's 
toolbox. Ruby, a dynamic and versatile programming language, provides a great 
platform for building command-line applications due to its elegant syntax and 
rich ecosystem of gems. In this section, we'll walk you through the process of 
creating robust command-line applications in Ruby, covering essential topics 
such as argument parsing, interactive prompts, error handling, and best practices.



<br>
### Prerequisites

Before we begin, make sure you have Ruby installed on your system. You can check 
if Ruby is installed by opening your terminal and running:

<br>
{% highlight terminal %}

ruby -v

{% endhighlight %}

<br>
If Ruby is not installed, visit the official Ruby website to download and 
install it.



<br>
### Step 1: Setting Up the Project

Let's start by setting up a basic project structure. Create a directory for your 
project and navigate into it:

<br>
{% highlight terminal %}

mkdir my_cli_app
cd my_cli_app

{% endhighlight %}

<br>
Now, let's create a file named <span class="badge">cli_app.rb</span>, which will 
be the entry point for our command-line application:

<br>
{% highlight ruby %}

# cli_app.rb

# Your application code will go here

{% endhighlight %}



<br>
### Step 2: Argument Parsing

To create a flexible command-line application, you'll need to parse the 
arguments passed to the application. The OptionParser class in Ruby's standard 
library makes argument parsing a breeze. Let's create a simple example where our 
application accepts a <span class="badge">--name</span> argument to greet the 
user:

<br>
{% highlight ruby %}

# cli_app.rb

require 'optparse'

# Initialize the options hash
options = {}

# Define the OptionParser
OptionParser.new do |opts|
  opts.banner = "Usage: cli_app.rb [options]"

  opts.on("--name NAME", "Your name") do |name|
    options[:name] = name
  end

  opts.on("-h", "--help", "Prints this help") do
    puts opts
    exit
  end
end.parse!

# Display the greeting
puts "Hello, #{options[:name]}!" if options[:name]

{% endhighlight %}

<br>
You can run the application with:

<br>
{% highlight terminal %}

ruby cli_app.rb --name John

{% endhighlight %}


<br>
This should output:

Hello, John!



<br>
### Step 3: Interactive Prompts

Many command-line applications require user input. We can use the highline gem 
to create interactive prompts that guide the user through the input process. 
Let's install the gem first:

{% highlight terminal %}

gem install highline

{% endhighlight %}

<br>
Now, let's create a more interactive version of our application that asks the 
user for their name if it's not provided as an argument:

<br>
{% highlight ruby %}

# cli_app.rb

require 'optparse'
require 'highline/import'

# Initialize the options hash
options = {}

# Define the OptionParser
OptionParser.new do |opts|
  opts.banner = "Usage: cli_app.rb [options]"

  opts.on("--name NAME", "Your name") do |name|
    options[:name] = name
  end

  opts.on("-h", "--help", "Prints this help") do
    puts opts
    exit
  end
end.parse!

# Ask for the name if not provided
unless options[:name]
  options[:name] = ask("What's your name? ") { |q| q.default = "John" }
end

# Display the greeting
puts "Hello, #{options[:name]}!"

{% endhighlight %}

<br>
Now, when you run the application without the <span class="badge">--name</span> 
argument:

<br>
{% highlight terminal %}

ruby cli_app.rb

{% endhighlight %}

<br>
It will ask you for your name:

<br>
{% highlight terminal %}

What's your name? John
Hello, John!

{% endhighlight %}



<br>
### Step 4: Error Handling

Robust command-line applications should handle errors gracefully. Let's modify 
our application to handle cases where the user provides an invalid name or no 
name at all:

<br>
{% highlight ruby %}


# cli_app.rb

require 'optparse'
require 'highline/import'

# Initialize the options hash
options = {}

# Define the OptionParser
OptionParser.new do |opts|
  opts.banner = "Usage: cli_app.rb [options]"

  opts.on("--name NAME", "Your name") do |name|
    options[:name] = name
  end

  opts.on("-h", "--help", "Prints this help") do
    puts opts
    exit
  end
end.parse!

# Ask for the name if not provided
begin
  unless options[:name]
    options[:name] = ask("What's your name? ") { |q| q.default = "John" }
  end

  # Validate the name
  if options[:name].empty?
    raise "Name cannot be empty"
  end

  # Display the greeting
  puts "Hello, #{options[:name]}!"
rescue StandardError => e
  puts "An error occurred: #{e.message}"
end

{% endhighlight %}

<br>
Now, if the user provides an empty name:

<br>
{% highlight terminal %}

ruby cli_app.rb --name ""

{% endhighlight %}

<br>
The application will handle the error gracefully:

<br>
{% highlight terminal %}

An error occurred: Name cannot be empty

{% endhighlight %}



<br>
### Step 5: Best Practices

As you continue to build more complex command-line applications, keep the 
following best practices in mind:

- Modularize your code: Divide your application into smaller, manageable modules 
and classes. This makes your code easier to maintain and test.

- Use configuration files: Consider using configuration files (e.g., YAML, JSON) 
to allow users to customize the behavior of your application.

- Document your application: Provide clear documentation on how to use your 
command-line application, including a detailed description of available options, 
usage examples, and troubleshooting tips.

- Test your application: Write automated tests to ensure that your command-line 
application behaves as expected. Tools like RSpec can be incredibly helpful for 
testing.

- Handle user input: Ensure that your application handles user input gracefully, 
providing helpful error messages when necessary.

- Keep it simple: Strive for simplicity in both the user interface and the 
codebase. Complex command-line interfaces can be confusing for users.



<br>
### Conclusion

In this section, we've covered the basics of building robust command-line 
applications with Ruby. We explored argument parsing, interactive prompts, error 
handling, and best practices. Armed with this knowledge, you can create powerful 
and user-friendly command-line tools that enhance your productivity and delight 
your users. Happy coding!



<br>
*Thanks for reading, see you in the next one!*
