


just ones

```js
npm install ionic cordova -g
```

```js
ionic start firstapp --v2 OBSOLTE
```

use:

```js
ionic start firstapp --type=ionic-angular
```

blank


If you got problems running it, you might be using a newer version of the Ionic CLI. Try 

`npm run ionic:serve`  

after `npm install` in the project folder in such cases!



```js
ionic start ionic2-basics blank --type=ionic-angular
```

or tabs

If your project uses Ionic 3 (check the package.json to see the version number), ionic generate page XY  will give you a slightly different output than shown in the videos:

a) You get an additional .module.ts  file: This file might contain a little bug, make sure it uses IonicPageModule.forChild(...)  and also import { IonicPageModule } from 'ionic-angular'   and NOT IonicModule.forChild(...) !

b) Your page will be named YourName  and not YourNamePage . Refer to it as YourName  in your code then (and not YourNamePage ).

Besides that, it works the same. Make sure to add it to your declarations[] and entryComponents[] array in AppModule and you should be good to go.




create page

```js
ionic generate page users
```

users.html
```js
<ion-header>

  <ion-navbar>
    <ion-title>The users</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>
  <button ion-button>User 'Albert'</button>
  <hr>
  <button ion-button>User 'Max'</button>
</ion-content>

<ion-footer padding>
  <p>The Footer</p>
</ion-footer>
````

home.html

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Ionic 2 Basics
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <button ion-button (click)="onGoToUsers">Users</button>
</ion-content>
```

home.ts

```js
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';
import { UsersPage } from '../users/users';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  constructor(public navCtrl: NavController) {

  }

  onGoToUsers() {
    this.navCtrl.push(UsersPage());
  }

}
```

app.module.ts

```js
import { BrowserModule } from '@angular/platform-browser';
import { ErrorHandler, NgModule } from '@angular/core';
import { IonicApp, IonicErrorHandler, IonicModule } from 'ionic-angular';
import { SplashScreen } from '@ionic-native/splash-screen';
import { StatusBar } from '@ionic-native/status-bar';

import { MyApp } from './app.component';
import { HomePage } from '../pages/home/home';
import { UsersPage } from '../pages/users/users';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    UsersPage
  ],
  imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    HomePage,
    UsersPage
  ],
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}
```

an alternative way of creating pages

manually

a user page

nested in users
user folder
user.ts


```js
import { Component } from "@angular/core";

@Component({
    selector: 'page-user',
    templateUrl: 'user.html'
})

export class UserPage {

}
```


user.html

```html
<ion-header>
    <ion-navbar>
        <ion-title>
            {{ name }}
        </ion-title>
    </ion-navbar>
</ion-header>

<ion-content padding>
    <p>Hi, I'm {{ name }}</p>
</ion-content>
```

user.ts

```js

export class UserPage {
    name: string;
}
```


app.module.ts

register user page

pass data from the users to the user component

users.html

```html

<ion-content padding>
  <button ion-button (click)="onLoadUser('Albert')">User 'Albert'</button>
  <hr>
  <button ion-button (click)="onLoadUser('Victor')">User 'Victor'</button>
</ion-content>
```

users.ts

```js
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { UserPage } from './user/user';

@IonicPage()
@Component({
  selector: 'page-users',
  templateUrl: 'users.html',
})
export class UsersPage {

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  onLoadUser(name: string) {
    // push page on the stack
    // data in the second argument. you can pass an object
    // this.navCtrl.push(UserPage, name);
    this.navCtrl.push(UserPage, {userName: name});
  }
}

```

in the user.ts

```js
import { Component, OnInit } from "@angular/core";
import { NavParams } from "ionic-angular";

@Component({
    selector: 'page-user',
    templateUrl: 'user.html'
})

export class UserPage implements OnInit {
    name: string;

    constructor (private navParams: NavParams) {
        // this allows me to retrieve the data
    }

    ngOnInit() {
        // this.name = this.navParams.data; // that wuold be what you pass, the object
        this.name = this.navParams.get('userName');
    }
}
```



# poping pages, going back

user.html

```html
<ion-content padding>
    <p>Hi, I'm {{ name }}</p>
    <button ion-button (click)="onGoBack()">Confirm</button>
</ion-content>
```

user.ts

```js
import { Component, OnInit } from "@angular/core";
import { NavParams, NavController } from "ionic-angular";

@Component({
    selector: 'page-user',
    templateUrl: 'user.html'
})

export class UserPage implements OnInit {
    name: string;

    constructor (
        private navParams: NavParams,
        private navCtrl: NavController
    ) {}

    ngOnInit() {
        this.name = this.navParams.get('userName');
    }

    onGoBack() {
        this.navCtrl.pop();
    }
}
```

user.ts

```js
import { Component, OnInit } from "@angular/core";
import { NavParams, NavController } from "ionic-angular";

@Component({
    selector: 'page-user',
    templateUrl: 'user.html'
})

export class UserPage implements OnInit {
    name: string;

    constructor (
        private navParams: NavParams,
        private navCtrl: NavController
    ) {}

    ngOnInit() {
        this.name = this.navParams.get('userName');
    }

    onGoBack() {
        // this.navCtrl.pop(); // go to last one
        this.navCtrl.popToRoot();
    }
}
```


# 29. saving time with helpful navigation directives

shortcut

home.js

```js
import { Component } from '@angular/core';
import { UsersPage } from '../users/users';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  usrPage = UsersPage; // i reference the class
}
```
home.html

```html
<ion-content padding>
  <button ion-button [navPush]="usrPage">Users</button>
</ion-content>
```

still working

though you can add [navParams]="your-data" in such a case


users.html

```html
<ion-content padding>
  <button ion-button (click)="onLoadUser('Albert')">User 'Albert'</button>
  <hr>
  <button ion-button (click)="onLoadUser('Victor')">User 'Victor'</button>
  <hr>
  <button ion-button navPop>Go Back</button>
</ion-content>
```



Configuring Page Transitions
Section 2, Lecture 30
Besides the page you want to go to and data you want to pass along, you can pass a third argument to push() (and a first argument to pop()): Navigation Options

These options allow you to configure the page transition. You do set your own configuration by passing a JS object where you may set the following properties:

animate (boolean): Whether or not the transition should animate.
animation (string): What kind of animation should be used.
direction (string): The conceptual direction the user is navigating. For example, is the user navigating forward, or back?
duration (number): The length in milliseconds the animation should take.
easing (string): The easing for the animation.
Example:

this.navCtrl.push(NewPage, {}, {
    direction: 'back', // default for push is 'forward'
    duration: 2000, // 2 seconds
    easing: 'ease-out'
});


# 31. lifecycle of a page

ionViewCanEnter			NavigationGuard => Should the Page be loaded?
protect a page

ionViewDidLoad			Page has loaded; not fired when cached => Setup Code for Page
only if the page is created

ionViewWillEnter		Page is about to enter and become active Page

ionViewDidEnter			Page has fully entered and now is active Page; Also fired when cached

ionViewCanLeave 		Navigation Guard => May the page be left?

ionViewWillLeave		page is about to leave and become inactive

ionViewDidLeave			Page finished leaving and is no inactive

ionViewWillUnload		Page is about to be destroyed (not cached anymore)



just for components that are pages, that are access through the navigation

users.js

```js
export class UsersPage {

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  onLoadUser(name: string) {
    this.navCtrl.push(UserPage, {userName: name});
  }

  // this is a hook
  ionViewCanEnter(): boolean | Promise<boolean> {
    console.log('ionViewCanEnter');
    return true; // or return Promise<void>
  }
}
```

home.html

```html
<ion-content padding>
  <button ion-button (click)="onGoToUsers()">Users</button>
</ion-content>
```

home.ts

```js
import { Component } from '@angular/core';
import { UsersPage } from '../users/users';
import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  usrPage = UsersPage; // i reference the class

  constructor (private navCtrl: NavController) {}

  onGoToUsers() {
    this.navCtrl.push(this.usrPage);
  }
}
```

sometimes you dont see did load, because the page is cached

# 33. How to use the Ionic 2 Documentation

ionicframework.com/docs/

documentation

API
all the classes ionic ships with

interesting, the nav related


components section: rich library

ionicons

# 34. Styling the App and Setting a Theme

users.scss

```css
page-users {
    p {
        background-color: red;
        padding: 10px;
    }
}
```

```html
  <p>The users</p>
```

we didn't have the styleUrl, like in Angular

in Ionic you use the sass approach

all the .scss are bundled together

if you put the p outside page-users, you are targetting all the p in the app

you have the general theme

the variable for the scss file

official documentation: theming

overwrite the default colors

the btns have by default the primary color

# 35. Utility Attributes

like padding




Section 2, Lecture 37
You can find the source code of this section attached to this lecture!

If you got problems running it, you might be using a newer version of the Ionic CLI. Try npm run ionic:serve  after npm install in the project folder in such cases!

Definitely check out the official docs if you want to dive deeper into one of the topics taught in this section: https://ionicframework.com/docs

Especially interesting might be the articles about the NavigationController, Components, Ionicons, Theming and the Utility Attributes.



































