---
layout: post
title:  "How to Add Navigation Menu to Ionic Apps"
date:   2020-05-16 19:22:06 +0300
img: ionic_side_menu.png
categories: ionic_apps
technology: Ionic/Angular Apps
tutorial_type: How To
---

<div align="justify" style="background-color:#fff"> 
<img srcset="
  https://www.technouz.com/wp-content/uploads/2017/04/ionic-header.jpg 1x,
  https://www.technouz.com/wp-content/uploads/2017/04/ionic-header.jpg 4x
" alt="missing image">
</div>
<br>

The Menu component is a navigation drawer that slides in from the side of the current view. By default, it slides in from the left, but the side can be overridden. <br><br>The menu will be displayed differently based on the mode, however the display type can be changed to any of the available menu types. <br><br>The menu element should be a sibling to the root content element. <br><br>There can be any number of menus attached to the content. These can be controlled from the templates, or programmatically using the MenuController. [Read more from ionic framework docs](https://ionicframework.com/docs/api/menu){:target="_blank"}


Ionic contains a component called ion-menu that enables us to easily create a side menu for navigation for our mobile apps.

Lets begin: 

1). Navigate to the root component of your project, which is the `app.component.ts`

Then, create the `sideMenu()` method in AppComponent class and inside that method add an array of objects that contain the different pages you want to navigate to in your project.

Then call your method inside the constructor as shown below:
  

{% highlight typescript linenos %}

import { Component } from '@angular/core';

import { Platform } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html',
  styleUrls: ['app.component.scss']
})
export class AppComponent {
  navigate : any;
  constructor(
    private platform: Platform,
    private splashScreen: SplashScreen,
    private statusBar: StatusBar
  ) {
    this.initializeApp();
    this.sideMenu();
  }

  initializeApp() {
    this.platform.ready().then(() => {
      this.statusBar.styleDefault();
      this.splashScreen.hide();
    });
  }

  sideMenu()
  {
    this.navigate =
    [
      {
        title : "Home",
        url   : "/select-section",
        icon  : "home"
      },
      {
        title : "Learning",
        url   : "/learning-section",
        icon  : "book"
      },
      {
        title : "Testing",
        url   : "/testing-section",
        icon  : "create"
      },
      {
        title : "Mastered",
        url   : "/mastered-section",
        icon  : "bulb"
      },
    ]
  }

}

{% endhighlight %}



**Note:** To obtain the url, you can navigate to the `app-routing.module.ts` and check the path of each page inside the Routes array. Ion icons name values are available on [ionicons site](https://ionicons.com/){:target="_blank"}.

2). Add the ion-menu component in the `app.component.html` file to create a side menu.

{% highlight typescript linenos %}

<ion-app>
    <ion-menu side="start" menuId="first" contentId="content1">
        <ion-header>
          <ion-toolbar>
            <ion-title>Menu</ion-title>
          </ion-toolbar>
        </ion-header>
        <ion-content>
          <ion-list *ngFor="let pages of navigate">
          <ion-menu-toggle auto-hide="true">
            <ion-item [routerLink]="pages.url" routerDirection="forward">
                <ion-icon [name]="pages.icon" slot="start"></ion-icon>
                   {{pages.title}} 
            </ion-item>
          </ion-menu-toggle>
          </ion-list>
        </ion-content>
      </ion-menu>
  <ion-router-outlet id="content1"></ion-router-outlet>
</ion-app>

{% endhighlight %}

<h5>Explanation:</h5>

The ion-menu with side="start" will create a side menu that starts from left to right, ion-title will create a title for the pages in the side menu. Also, don’t forget to add the attribute contentId which is the id the menu should use. The routerLink will let you navigate to the url specified, and the routerDirection Determines the animation that takes place when the page changes.

Then, we use ngFor to loop inside the array navigate and we use the attribute url to navigate to the specific page when the click event is called.

**NB:** `ion-menu-toggle` is used to open and close the side menu, therefore when you click on a menu item, it will close the side menu automatically.

<h4>Menu Button</h4>

3). Add the open/close humburger button on your main page and/or on all pages you want. To be able to do that you need to use the component `ion-menu-button` in the html of each page, that will create the button to open a menu on the page.

For this tut, we'll add it in the `home.page.html` file:

{% highlight javascript linenos %}

<ion-header>
  <ion-toolbar>
      <ion-buttons slot="start">
      <ion-menu-button></ion-menu-button>
      </ion-buttons>
    <ion-title>
      Home
    </ion-title>
  </ion-toolbar>
</ion-header>

{% endhighlight %}



After adding the above code, you should get a similar menu to the one below:

<img srcset="
  {{baseurl.site}}/assets/img/ionic_side_menu.png 1x, 
  {{baseurl.site}}/assets/img/ionic_side_menu.png 2x
" alt="missing image">

That it's!

*See you on the next tuts*


