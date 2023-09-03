---
layout: post
title:  "Mastering GitHub Actions for Continuous Integration and Deployment"
author: Denis Kobare
date:   2024-10-04 16:30:00 +0300
img: /assets/img/svg/git.svg
categories: vcs
sub_category: github
type: concepts
technology: Github/Git
permalink: "category/:categories/github/concepts/:year:month/:title"
---


### Introduction

GitHub Actions is a powerful tool that enables you to automate various tasks in 
your software development workflow. From continuous integration (CI) to 
deployment (CD), GitHub Actions streamlines your processes and boosts 
collaboration among team members. In this comprehensive guide, we'll cover the 
essential concepts of GitHub Actions, providing practical examples and 
explanations to help you set up a robust CI/CD pipeline. We'll also delve into 
advanced topics such as matrix builds, caching, and custom workflows. Let's dive 
in and master GitHub Actions!



<br>
### Introduction to GitHub Actions

GitHub Actions is a feature provided by GitHub that enables you to automate 
workflows directly within your GitHub repository. With this tool, you can define 
custom events and actions to trigger specific tasks whenever those events occur. 
Whether you want to run tests whenever you push code, deploy to a staging server 
when a pull request is merged, or even publish a new release, GitHub Actions has 
you covered.




<br>
### Getting Started with Basic Workflows

To get started with GitHub Actions, follow these steps:

1. Navigate to the "Actions" tab: In your GitHub repository, click on the 
"Actions" tab at the top.

2. Set up a new workflow: Choose a template (e.g., "Set up a new workflow" or 
"Simple workflow") or create your own custom workflow file in the 
.github/workflows directory.

3. Define your workflow: A workflow is defined using YAML syntax. You specify the 
events that trigger the workflow and the jobs that should be executed.

4. Create a workflow file: For example, a basic workflow that runs tests on every 
push to the main branch could look like this:


{% highlight yaml %}

name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

{% endhighlight %}



<br>
### Understanding Continuous Integration (CI)

Continuous Integration (CI) is a development practice where code changes are 
automatically tested and integrated into the main codebase frequently. This 
practice ensures that issues are detected early, reducing the risk of 
integration problems and making the development process smoother.



<br>
### Setting Up a CI Workflow

In the example above, we've already set up a basic CI workflow that triggers on 
every push to the main branch. This workflow performs the following steps:

1. Checkout code: This step checks out the repository code so that the 
subsequent steps can work on it.

2. Set up Node.js: This step sets up the Node.js environment using the specified 
version.

3. Install dependencies: This step installs the project dependencies using npm.

4. Run tests: This step runs the tests using npm.

By setting up a CI workflow, you ensure that tests are executed automatically 
whenever code is pushed to the main branch, providing fast feedback to 
developers.



<br>
### Automating Tests and Quality Checks

CI workflows can include various test and quality check steps to ensure the code 
meets the required standards. For example, you can integrate linters, static 
analysis tools, and unit tests. Here's an extended version of the previous 
workflow that includes linting and static analysis:


{% highlight yaml %}

name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Lint code
      run: npm run lint

    - name: Run tests
      run: npm test

    - name: Static analysis
      run: npm run analyze

{% endhighlight %}



<br>
### Continuous Deployment (CD)

Continuous Deployment (CD) is the practice of automatically deploying code 
changes to a staging or production environment after passing CI tests. GitHub 
Actions can also handle CD by creating workflows that trigger on specific events, 
such as the successful completion of CI tests or the creation of a new release.

For example, a CD workflow might look like this:

{% highlight yaml %}

name: CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Build and deploy
      run: npm run build && npm run deploy

{% endhighlight %}


This CD workflow is triggered on every push to the main branch. It checks out 
the code, sets up Node.js, installs dependencies, and then builds and deploys 
the project.



<br>
### Advanced Concepts

#### Matrix Builds

Matrix builds allow you to test your project against multiple versions of a 
language or framework simultaneously. This is particularly useful when you want 
to ensure compatibility across different environments. Here's an example of a 
matrix build:

{% highlight yaml %}

name: Matrix Build

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12, 14, 16]

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

{% endhighlight %}


This matrix build tests the project against Node.js versions 12, 14, and 16.


<br>
#### Caching Dependencies

Caching dependencies can significantly improve the speed of your workflow by 
storing certain files between workflow runs. For example, you can cache the 
node_modules directory to avoid re-installing dependencies every time. Here's 
how you can set up caching:

{% highlight yaml %}

name: CI with Caching

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Cache Node.js dependencies
      uses: actions/cache@v2
      with:
        path: ~/.npm
        key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-npm-

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

{% endhighlight %}


In this workflow, the Node.js dependencies are cached, and the cache is restored 
using a key based on the package-lock.json file.


<br>
#### Custom Workflows

GitHub Actions supports custom workflows tailored to your project's needs. You 
can create workflows for specific events, such as pull requests or releases, and 
define custom steps to automate complex tasks. Here's an example of a custom 
workflow that runs on every pull request, checking for code formatting and 
running tests:

{% highlight yaml %}

name: PR Checks

on:
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Format code
      run: npm run format -- --check

    - name: Run tests
      run: npm test

{% endhighlight %}


This custom workflow runs on every pull request targeting the main branch, 
checking code formatting and running tests to ensure the quality of the code.



<br>
### Conclusion

By mastering GitHub Actions, you gain the ability to automate your CI/CD 
pipeline, making your development process more efficient and reliable. From 
basic workflows to advanced concepts like matrix builds and caching, GitHub 
Actions offers a wide range of features to suit your project's requirements. 
Experiment with these concepts, create custom workflows tailored to your needs, 
and enjoy the benefits of automated and streamlined software development.



<br>
*Thanks for reading, see you in the next one!*
