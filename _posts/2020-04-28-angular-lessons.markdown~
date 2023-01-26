---
layout: post
title:  "Angular Links"
date:   2020-04-28 15:44:06 +0300
img: pic08.jpg
categories: ruby-on-rails
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


declarations
declarations specifies the components that are defined in this module. This is an important idea
in Angular:
You have to declare components in a NgModule before you can use them in your templates.
You can think of an NgModule a bit like a “package” and declarations states what components are
“owned by” this module.
You may have noticed that when we used ng generate , the tool automatically added our compo-
nents to this declarations list! The idea is that when we generated a new component, the ng tool
assumed we wanted it to belong to the current NgModule .
imports
imports describes which dependencies this module has. We’re creating a browser app, so we want
to import the BrowserModule .
If your module depends on other modules, you list them here.
import vs. imports ?
You might be asking the question, “What’s the difference between import ing a class at the
top of the file and putting a module in imports ?”
The short answer is that you put something in your NgModule ’s imports if you’re going
to be using it in your templates or with dependency injection. We haven’t talked about
dependency injection, but rest assured, we will.
providers
providers is used for dependency injection. So to make a service available to be injected throughout
our application, we will add it here.
Learn more about this in the section on Dependency Injection .
bootstrap
bootstrap tells Angular that when this module is used to bootstrap an app, we need to load the
AppComponent component as the top-level component.

We’re going to make two components in this app:
1. The overall application, which contains the form used to submit new articles (marked in
magenta in the picture).
2. Each article (marked in mint green).Writing Your First Angular Web Application
31
In a larger application, the form for submitting articles would probably become its own
component. However, having the form be its own component makes the data passing more
complex, so we’re going to simplify in this chapter and have only two components.
For now two components will work fine, but we’ll learn how to deal with more sophisti-
cated data architectures in later chapters of this book.
But first thing’s first, let’s generate a new application by running the same ng new command we
ran before to create a new application passing it the name of the app we want to create (here, we’ll
create an application called angular-reddit ):
1
ng new angular-reddit
We’ve provide a completed version of our angular-reddit in the example code download.
If you ever need more context, be sure to check it out to see how everything fits together.
Adding CSS
First thing we want to do is add some CSS styling so that our app isn’t completely unstyled.
If you’re building your app from scratch, you’ll want to copy over a few files from our
completed example in the first_app/angular-reddit folder.
Copy:
•
•
•
•
src/index.html
src/styles.css
src/app/vendor
src/assets/images
into your application’s folder.
For this project we’re going to be using Semantic-UI21 to help with the styling. Semantic-UI
is a CSS framework, similar to Zurb Foundation22 or Twitter Bootstrap23. We’ve included
it in the sample code download so all you need to do is copy over the files specified above.
21 http://semantic-ui.com/
22 http://foundation.zurb.com
23 http://getbootstrap.comWriting Your First Angular Web Application
32
The Application Component
Let’s now build a new component which will:
1. store our current list of articles
2. contain the form for submitting new articles.
We can find the main application component on the src/app/app.component.ts file. Let’s open this
file. Again, we’ll see the same initial contents we saw previously.
code/first-app/angular-reddit/src/app/app.component.ts
1
2
3
4
5
6
7
8
9
10
import { Component } from '@angular/core';
@Component({
selector: 'app-root',
templateUrl: './app.component.html',
styleUrls: ['./app.component.css']
})
export class AppComponent {
title = 'app works!';
}
Notice that the title property was automatically generated for us on the AppComponent .
Remove that line, because we aren’t using the component title .
Below we’re going to be submitting new links that have a ‘title’, which could be confused
with the AppComponent title that was auto-generated by Angular CLI. When we add a ‘title’
to the new links we submit below the form title is a separate form field.
Let’s change the template a bit to include a form for adding links. We’ll use a bit of styling from the
semantic-ui package to make the form look a bit nicer:Writing Your First Angular Web Application
33
code/first-app/angular-reddit/src/app/app.component.html
1
2
3
4
5
6
7
8
9
10
11
12
<form class="ui large form segment">
<h3 class="ui header">Add a Link</h3>
<div class="field">
<label for="title">Title:</label>
<input name="title">
</div>
<div class="field">
<label for="link">Link:</label>
<input name="link">
</div>
</form>
We’re creating a template that defines two input tags: one for the title of the article and the other
for the link URL.
When we load the browser you should see the rendered form:



Adding Interaction
Now we have the form with input tags but we don’t have any way to submit the data. Let’s add
some interaction by adding a submit button to our form.
When the form is submitted, we’ll want to call a function to create and add a link. We can do this
by adding an interaction event on the <button /> element.
We tell Angular we want to respond to an event by surrounding the event name in parenthesis () .
For instance, to add a function call to the <button /> onClick event, we can pass it through like so:
1
2
3
4
<button (click)="addArticle()"
class="ui positive right floated button">
Submit link
</button>
Now, when the button is clicked, it will call a function called addArticle() , which we need to define
on the AppComponent class. Let’s do that now:Writing Your First Angular Web Application
35
code/first-app/angular-reddit/src/app/app.component.ts
8
9
10
11
12
13
export class AppComponent {
addArticle(title: HTMLInputElement, link: HTMLInputElement): boolean {
console.log(`Adding article title: ${title.value} and link: ${link.value}`);
return false;
}
}
With the addArticle() function added to the AppComponent and the (click) event added to
the <button /> element, this function will be called when the button is clicked. Notice that the
addArticle() function can accept two arguments: the title and the link arguments. We need to
change our template button to pass those into the call to the addArticle() .
We do this by populating a template variable by adding a special syntax to the input elements on
our form. Here’s what our template will look like:
code/first-app/angular-reddit/src/app/app.component.html
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
<form class="ui large form segment">
<h3 class="ui header">Add a Link</h3>
<div class="field">
<label for="title">Title:</label>
<input name="title" #newtitle> <!-- changed -->
</div>
<div class="field">
<label for="link">Link:</label>
<input name="link" #newlink> <!-- changed -->
</div>
<!-- added this button -->
<button (click)="addArticle(newtitle, newlink)"
class="ui positive right floated button">
Submit link
</button>
</form>
Notice that in the input tags we used the # (hash) to tell Angular to assign those tags to a local
variable. By adding the #newtitle and #newlink to the appropriate <input /> elements, we can
pass them as variables into the addArticle() function on the button!
To recap what we’ve done, we’ve made four changes:Writing Your First Angular Web Application
36
1. Created a button tag in our markup that shows the user where to click
2. We created a function named addArticle that defines what we want to do when the button
is clicked
3. We added a (click) attribute on the button that says “call the function addArticle when
this button is pressed”.
4. We added the attribute #newtitle and #newlink to the <input> tags
Let’s cover each one of these steps in reverse order:
Binding input s to values
Notice in our first input tag we have the following:
1
<input name="title" #newtitle>
This markup tells Angular to bind this <input> to the variable newtitle . The #newtitle syntax
is called a resolve. The effect is that this makes the variable newtitle available to the expressions
within this view.
newtitle is now an object that represents this input DOM element (specifically, the type is
HTMLInputElement ). Because newtitle is an object, that means we get the value of the input tag
using newtitle.value .
Similarly we add #newlink to the other <input> tag, so that we’ll be able to extract the value from
it as well.
Binding actions to events
On our button tag we add the attribute (click) to define what should happen when the button is
clicked on. When the (click) event happens we call addArticle with two arguments: newtitle
and newlink . Where did this function and two arguments come from?
1. addArticle is a function on our component definition class AppComponent
2. newtitle comes from the resolve ( #newtitle ) on our <input> tag named title
3. newlink comes from the resolve ( #newlink ) on our <input> tag named link
All together:Writing Your First Angular Web Application
1
2
3
4
37
<button (click)="addArticle(newtitle, newlink)"
class="ui positive right floated button">
Submit link
</button>
The markup class="ui positive right floated button" comes from Semantic UI and
it gives the button the pleasant green color.


Defining the Action Logic
On our class AppComponent we define a new function called addArticle . It takes two arguments:
title and link . Again, it’s important to realize that title and link are both objects of type
HTMLInputElement and not the input values directly. To get the value from the input we have to
call title.value . For now, we’re just going to console.log out those arguments.
code/first-app/angular-reddit/src/app/app.component.ts
9
10
11
12
addArticle(title: HTMLInputElement, link: HTMLInputElement): boolean {
console.log(`Adding article title: ${title.value} and link: ${link.value}`);
return false;
}
Notice that we’re using backtick strings again. This is a really handy feature of ES6: backtick
strings will expand template variables!
Here we’re putting ${title.value} in the string and this will be replaced with the value
of title.value in the string.
Try it out!
Now when you click the submit button, you can see that the message is printed on the console:

39
ng generate component article
We have three parts to defining this new component:
1. Define the ArticleComponent view in the template
2. Define the ArticleComponent properties by annotating the class with @Component
3. Define a component-definition class ( ArticleComponent ) which houses our component logic
Let’s talk through each part in detail:
Creating the ArticleComponent template
We define the template using the file article.component.html :
code/first-app/angular-reddit/src/app/article/article.component.html
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
<div class="four wide column center aligned votes">
<div class="ui statistic">
<div class="value">
{{ votes }}
</div>
<div class="label">
Points
</div>
</div>
</div>
<div class="twelve wide column">
<a class="ui large header" href="{{ link }}">
{{ title }}
</a>
<ul class="ui big horizontal list voters">
<li class="item">
<a href (click)="voteUp()">
<i class="arrow up icon"></i>
upvote
</a>
</li>
<li class="item">
<a href (click)="voteDown()">
<i class="arrow down icon"></i>
downvote
</a>40
Writing Your First Angular Web Application
27
28
29
</li>
</ul>
</div>
There’s a lot of markup here, so let’s break it down :
A Single reddit-article Row
We have two columns:
1. the number of votes on the left and
2. the article information on the right.
We specify these columns with the CSS classes four wide column and twelve wide column
respectively (remember that these come from SemanticUI’s CSS).
We’re showing votes and the title with the template expansion strings {{ votes }} and {{ title
}} . The values come from the value of votes and title property of the ArticleComponent class,
which we’ll define in a minute.
Notice that we can use template strings in attribute values, as in the href of the a tag: href="{{
link }}" . In this case, the value of the href will be dynamically populated with the value of link
from the component class
On our upvote/downvote links we have an action. We use (click) to bind voteUp() / voteDown() to
their respective buttons. When the upvote button is pressed, the voteUp() function will be called on
the ArticleComponent class (similarly with downvote and voteDown() ).
Creating the ArticleComponentWriting Your First Angular Web Application
41
code/first-app/angular-reddit/src/app/article/article.component.ts
7
8
9
10
11
@Component({
selector: 'app-article',
templateUrl: './article.component.html',
styleUrls: ['./article.component.css'],
})
First, we define a new Component with @Component . The selector says that this component is
placed on the page by using the tag <app-article> (i.e. the selector is a tag name).
So the most essential way to use this component would be to place the following tag in our markup:
1
2
<app-article>
</app-article>
These tags will remain in our view when the page is rendered.
Creating the ArticleComponent Definition Class
Finally, we create the ArticleComponent definition class:
code/first-app/angular-reddit/src/app/article/article.component.ts

{% highlight ruby %}
export class ArticleComponent implements OnInit {
@HostBinding('attr.class') cssClass = 'row';
votes: number;
title: string;
link: string;
constructor() {
this.title = 'Angular 2';
this.link = 'http://angular.io';
this.votes = 10;
}
voteUp() {
this.votes += 1;
}
voteDown() {
this.votes -= 1;
}Writing Your First Angular Web Application
ngOnInit() {
}
}
{% endhighlight %}
Here we create four properties on ArticleComponent :

cssClass - the CSS class we want to apply to the “host” of this component
votes - a number representing the sum of all upvotes, minus the downvotes
title - a string holding the title of the article
link - a string holding the URL of the article
We want each app-article to be on its own row. We’re using Semantic UI, and Semantic provides
a CSS class for rows24 called row .
In Angular, a component host is the element this component is attached to. We can set properties
on the host element by using the @HostBinding() decorator. In this case, we’re asking Angular to
keep the value of the host elements class to be in sync with the property cssClass .
We import HostBinding from the package @angular/core . For instance we can add
HostBinding like this:
1
import { Component, HostBinding } from '@angular/core';
By using @HostBinding() the host element (the app-article tag) we want to set the class attribute
to have “ row ”.
Using the @HostBinding() is nice because it means we can encapsulate the app-article
markup within our component. That is, we don’t have to both use a app-article tag
and require a class="row" in the markup of the parent view. By using the @HostBinding
decorator, we’re able to configure our host element from within the component.
In the constructor() we set some default attributes:
24 http://semantic-ui.com/collections/grid.htmlWriting Your First Angular Web Application
43
code/first-app/angular-reddit/src/app/article/article.component.ts
18
19
20
21
22
constructor() {
this.title = 'Angular 2';
this.link = 'http://angular.io';
this.votes = 10;
}
And we define two functions for voting, one for voting up voteUp and one for voting down voteDown :
code/first-app/angular-reddit/src/app/article/article.component.ts
24
25
26
27
28
29
30
voteUp() {
this.votes += 1;
}
voteDown() {
this.votes -= 1;
}
In voteUp we increment this.votes by one. Similarly we decrement for voteDown .
Using the app-article Component
In order to use this component and make the data visible, we have to add a <app-article></app-
article> tag somewhere in our markup.
In this case, we want the AppComponent to render this new component, so let’s update the code in
that component. Add the <app-article> tag to the AppComponent ’s template right after the closing
</form> tag:
1
2
3
4
5
6
7
8
9
10
<button (click)="addArticle(newtitle, newlink)"
class="ui positive right floated button">
Submit link
</button>
</form>
<div class="ui grid posts">
<app-article>
</app-article>
</div>Writing Your First Angular Web Application
44
If we generated the ArticleComponent using Angular CLI (via ng generate component ), by default
it should have “told” Angular about our app-article tag (more on that below). However, if we
created this component “by hand” and we reload the browser now, we might see that the <app-
article> tag wasn’t compiled. Oh no!
Whenever hitting a problem like this, the first thing to do is open up your browser’s developer
console. If we inspect our markup (see screenshot below), we can see that the app-article tag is on
our page, but it hasn’t been compiled into markup. Why not?
