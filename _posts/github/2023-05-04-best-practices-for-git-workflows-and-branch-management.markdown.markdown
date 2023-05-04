---
layout: post
title:  "Best Practices for Git Workflows and Branch Management"
author: Denis Kobare
date:   2023-05-04 16:30:00 +0300
img: /assets/img/svg/git.svg
categories: vcs
sub_category: github
type: concepts
technology: Github/Git
permalink: "category/:categories/github/concepts/:year:month/:title"
---


### Introduction

Git is a popular version control system used by Rails developers to manage 
changes, collaborate with others, and maintain code quality. However, using Git 
effectively requires more than just basic knowledge of its commands. In this 
article, we'll explore some best practices for Git workflows and branch 
management in Rails apps, with practical examples to illustrate each point.


<br>
### Use a Branching Model

A branching model is a strategy for organizing your Git branches based on the 
workflow of your team. There are several popular branching models, but one that 
works well for Rails apps is the Gitflow model.

Gitflow separates branches into two main types: long-lived branches and 
short-lived branches. Long-lived branches are used to represent the overall state 
of your codebase, such as production or staging environments. Short-lived 
branches are used for feature development, bug fixes, and other changes that are 
not yet ready for release.

Using Gitflow, you can create a "develop" branch to represent the current state 
of your development work. When a feature or bug fix is ready for testing, it can 
be merged into the "develop" branch. Once the "develop" branch has accumulated 
enough changes, it can be merged into a "release" branch for final testing and 
then into the "master" branch for production release.

Here's an example of how to create a new feature branch using Gitflow:

<br>
{% highlight terminal %}

$ git flow feature start new-feature

{% endhighlight %}


<br>
This command creates a new branch named "feature/new-feature" based on the 
current state of the "develop" branch. You can then make changes to this branch 
and commit them as usual.

When you're done with the feature, you can use the following command to merge it 
back into the "develop" branch:


<br>
{% highlight terminal %}

$ git flow feature finish new-feature

{% endhighlight %}


<br>
This command merges the changes from the "feature/new-feature" branch into the 
"develop" branch and deletes the "feature/new-feature" branch.



<br>
### Keep Branches Small and Focused

When working on a feature or bug fix, it's tempting to create a large branch that 
includes multiple changes. However, this can make it difficult to review and 
merge your code later. Instead, it's best to keep your branches small and focused 
on a single change.

Using small, focused branches makes it easier to review and merge changes because 
the changes are easier to understand. Additionally, small branches can be merged 
more quickly, reducing the time between when a change is completed and when it's 
released.

Here's an example of how to create a new branch for a small change:

<br>
{% highlight terminal %}

$ git checkout -b fix-bug

{% endhighlight %}


<br>
This command creates a new branch named "fix-bug" based on the current state of 
the branch you're currently on. You can then make the necessary changes and 
commit them as usual.

When you're done with the change, you can use the following command to merge it 
back into the original branch:


<br>
{% highlight terminal %}

$ git checkout original-branch
$ git merge fix-bug

{% endhighlight %}


<br>
This command merges the changes from the "fix-bug" branch into the 
"original-branch" branch.



<br>
### Use Pull Requests for Code Review

Code review is an important part of maintaining code quality in your Rails app. 
Pull requests (PRs) are a Git feature that allows you to submit your changes for 
review before they are merged into a branch.

Using PRs for code review has several benefits. First, it allows other members 
of your team to review your changes and provide feedback before they are merged. 
This can help catch bugs and improve the overall quality of your code. Second, 
PRs can be used to track of changes over time, making it easier to understand 
why certain decisions were made.


To use PRs for code review, you can create a branch for your changes and then 
submit a pull request to the branch you want to merge your changes into. Other 
members of your team can then review your changes and provide feedback before 
the changes are merged.

Here's an example of how to create a pull request using GitHub:

1 . Create a new branch for your changes:

<br>
{% highlight terminal %}

$ git checkout -b my-feature

{% endhighlight %}


<br>
2 . Push the branch to GitHub:

<br>
{% highlight terminal %}

$ git push origin my-feature

{% endhighlight %}


<br>
3 . Create a pull request on GitHub:

 - Go to the repository on GitHub.
 - Click on the "Pull requests" tab.
 - Click on the "New pull request" button.
 - Select the branch you want to merge your changes into (e.g. "develop").
 - Select the branch that contains your changes (e.g. "my-feature").
 - Provide a description of the changes you made.
 - Submit the pull request.
 
 
<br>
Once the pull request is submitted, other members of your team can review the 
changes and provide feedback. You can make additional changes based on the 
feedback and push them to the same branch. GitHub will automatically update the 
pull request with the new changes.

When the pull request is approved, you can merge the changes into the target 
branch. GitHub provides an option to merge the changes using a merge commit, 
which creates a new commit that represents the merge. This commit includes a 
message that summarizes the changes that were merged, making it easier to 
understand the history of the code.



<br>
### Use Rebase to Keep Branches Up-to-Date

When working on a long-lived branch, such as the "develop" branch, it's important 
to keep it up-to-date with the latest changes from other branches. This can be 
done using the "merge" command, which combines the changes from two branches into 
a new commit.

However, merging can create a messy history, with multiple merge commits that 
make it difficult to understand the changes over time. To avoid this, you can use 
the "rebase" command instead.

Rebasing takes the changes from one branch and applies them on top of another 
branch. This creates a linear history, with a single branch representing the 
changes over time. Rebasing can make it easier to understand the history of your 
code and to identify the cause of bugs or issues.

Here's an example of how to use rebase to keep your branch up-to-date:

1 . Checkout the branch you want to rebase:

<br>
{% highlight terminal %}

$ git checkout my-feature

{% endhighlight %}


<br>
2 . Fetch the latest changes from the remote repository:

<br>
{% highlight terminal %}

$ git fetch origin

{% endhighlight %}


<br>
3 . Rebase your changes on top of the latest changes:

<br>
{% highlight terminal %}

$ git rebase origin/develop

{% endhighlight %}



<br>
This command takes the changes in your "my-feature" branch and applies them on 
top of the latest changes in the "develop" branch.

After the rebase is complete, you may need to resolve any conflicts that arise. 
Once the conflicts are resolved, you can push your changes to the remote 
repository using the "push" command.



<br>
### Conclusion

Using Git effectively is essential for maintaining code quality and collaborating 
with others on your Rails app. By following these best practices for Git workflows 
and branch management, you can ensure that your code is well-organized, easy to 
understand, and ready for release.

Remember to use a branching model, keep branches small and focused, use pull 
requests for code review, and use rebase to keep your branches up-to-date. With 
these practices in place, you'll be well on your way to developing high-quality 
Rails apps that are easy to maintain and extend.



<br>
*Thanks for reading, see you in the next one!*
