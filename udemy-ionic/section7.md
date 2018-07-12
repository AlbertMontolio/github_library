# "The extended Recipe Book" App (Auth and Http)

# 143. Module introduction

Impement Authentication
Send http requests and work with responses

we use firebase as a dummy backend

# 144. What we're going to build.

unauthenticated

authenticated

# 146. Generating the required Pages

```js
ionic generate page signup

ionic generate page signin
```

great place to implement the side menu

depending the element we chose, we have a slack or another

app.html

# 147. Adding a Sidemenu

```html
<ion-menu [content]="">

</ion-menu>
```

has this content property we need to bind, we tell it where should you render your content

this will be the ionnav

we give a reference #nav

this menu belongs to this navigation stack


```html
<ion-menu [content]="nav">
    <ion-header>
        <ion-toolbar>
            <ion-title>Menu</ion-title>
        </ion-toolbar>
    </ion-header>
    <ion-content>
        <ion-list>
            <button
                ion-item
                icon-left
                (click)="onLoad(tabsPage)"
            >
                Recipe Book
            </button>
        </ion-list>
    </ion-content>
</ion-menu>

<ion-nav [root]="rootPage" #nav></ion-nav>
```


app.component.ts

```js
export class MyApp {
  tabsPage = TabsPage;
  ```

app.html

```html

<ion-nav [root]="tabsPage" #nav></ion-nav>
```

there is no single root page anymore, it will depend on the authentication state

onLoad(tabsPage)
tabsPage is a property that holds a reference to our tabspage

```html
<ion-list>
    <button
        ion-item
        icon-left
        (click)="onLoad(tabsPage)"
    >Recipe Book</button>
    <button
        ion-item
        icon-left
        (click)="onLoad(signinPage)"
    >Sign in</button>
</ion-list>
```

app.component.ts

```js
import { TabsPage } from '../pages/tabs/tabs';
import { SigninPage } from '../pages/signin/signin';

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  tabsPage = TabsPage;
  signinPage = SigninPage;

```


```html
<ion-list>
    <button
        ion-item
        icon-left
        (click)="onLoad(tabsPage)"
    >
        <ion-icon name="book"></ion-icon>
        Recipe Book
    </button>
    <button
        ion-item
        icon-left
        (click)="onLoad(signinPage)"
    >
        <ion-icon name="log-in"></ion-icon>
        Sign in
    </button>
</ion-list>
```



```html
<button
    ion-item
    icon-left
    (click)="onLoad(signupPage)"
>
    <ion-icon name="person"></ion-icon>
    Sign up
</button>
```

```js
import { SignupPage } from '../pages/signup/signup';

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  tabsPage = TabsPage;
  signinPage = SigninPage;
  signupPage = SignupPage;
```

```html
<button
    ion-item
    icon-left
    (click)="onLogout()"
>
    <ion-icon name="log-out"></ion-icon>
    Logout
</button>
```

app.module.ts

```js
@NgModule({
  declarations: [
    MyApp,
    EditRecipePage,
    RecipePage,
    RecipesPage,
    ShoppingListPage,
    TabsPage,
    SigninPage,
    SignupPage
  ],
```

```js
entryComponents: [
    MyApp,
    EditRecipePage,
    RecipePage,
    RecipesPage,
    ShoppingListPage,
    TabsPage,
    SigninPage,
    SignupPage
  ],
```

onLoad and onLogout are not implemented

app.component.ts

```js
export class MyApp {
  [...]

  onLoad(page: any) {
    
  }

}
```

we can not use the navcontroller in the app.component, because the navigation has not been initializated at the point of time this component has been created

then

we have to get a reference, by using

```js
@ViewChild
```

we have to get a reference to this ion-nav component #nav, which is our template representation of the navcontroller

```js
import { Component, ViewChild } from '@angular/core';
import { Platform, NavController } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';
import { TabsPage } from '../pages/tabs/tabs';
import { SigninPage } from '../pages/signin/signin';
import { SignupPage } from '../pages/signup/signup';

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  tabsPage = TabsPage;
  signinPage = SigninPage;
  signupPage = SignupPage;
  @ViewChild('nav') nav: NavController; 
  ```
we are getting access to this element, store it a property called nav

i can use nav, to set the rootpage, set it to the page that we get as an argument

```js
  onLoad(page: any) {
    this.nav.setRoot(page);

  }
```

also iwant to close the menu


app.component.ts

```js
constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen, private menuCtrl: MenuController) {
	```

```js
  onLoad(page: any) {
    this.nav.setRoot(page);
    this.menuCtrl.close();
  }

  onLogout() {
    
  }
  ```

the missing piece is the btn to open the menu

from all major components or pages

recipes page, in the shopping-list page

in signin, and signup

those four pages are the pages important, where we need the button

recipes.html

```html
<ion-header>

  <ion-navbar>
    <ion-nav-buttons start>
      <button ion-button icon-only menuToggle>
        <ion-icon name="menu"></ion-icon>
      </button>
    </ion-nav-buttons>
```

# 148. Creating the Signup Page (and Form)

signup.html

```html
<ion-header>

  <ion-navbar>
    <ion-buttons start>
      <button ion-button icon-only menuToggle>
        <ion-icon name="menu"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>Sign up</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>
  <form #f="ngForm" (ngSubmit)="onSignup(f)">
    <ion-list>
      <ion-item>
        <ion-label fixed>Mail</ion-label>
        <ion-input 
          type="email" 
          ngModel 
          name="email" 
          required></ion-input>
      </ion-item>
      <ion-item>
        <ion-label fixed>Password</ion-label>
        <ion-input 
          type="password" 
          ngModel 
          name="password" 
          required
          [minlength]="6"
          ></ion-input>
      </ion-item>
    </ion-list>
    <button ion-button block type="submit" [disabled]="!f.valid">Signup</button>
  </form>
</ion-content>
```

# 149. Creating the Signin Page

signin.html

```html
<ion-header>

  <ion-navbar>
    <ion-buttons start>
      <button ion-button icon-only menuToggle>
        <ion-icon name="menu"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>Sign in</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>
  <form #f="ngForm" (ngSubmit)="onSignin(f)">
    <ion-list>
      <ion-item>
        <ion-label fixed>Mail</ion-label>
        <ion-input 
          type="email" 
          ngModel 
          name="email" 
          required></ion-input>
      </ion-item>
      <ion-item>
        <ion-label fixed>Password</ion-label>
        <ion-input 
          type="password" 
          ngModel 
          name="password" 
          required
          ></ion-input>
      </ion-item>
    </ion-list>
    <button ion-button block type="submit" [disabled]="!f.valid">Signin</button>
  </form>
</ion-content>
```


signin.ts

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'page-signin',
  templateUrl: 'signin.html',
})
export class SigninPage {

  onSignin(form: NgForm) {
    console.log(form.value);
  }

}
```

# 150. How Authentication Works in an Ionic 2 (Mobile) App

"Normal" Webpage

session-based

Server - Client



Web App / Mobile App

Server - Client (Broswer / Mobile Device)

we dont have a strong connection

if you open your phone, maybe you have no internet, or you open the app in different devices

we dont habe this session based approach

we authenticate ourself, by sending the data to the server
like email and password
on the server we check if we have a user, an a valid password

but not we dont set up a session, stored in a cookie

we generate a token, and send to the device

on each request to a protetected resource, we send again the token to the server

you onle have a token, hej, i authenticate myself, here is the token, you can check if it its valid.

tokens invalid after minutes or hours

firebase makes it very easy, this approach

# 151. Setting up Firebase (as a Development Backend)

firebase, ready to use backend out of the box.

db, authentication

firebase

check the web documentation

angular is a web framework

open your console

create a new project

give a name, a region

add firebase to your web app

on the left, the features you get

authentication & database, 
firebase storage

sdk we will use

start enabling authentication

set up sign in methods

we can use thir party authentication

click on email/password, enable, save

time to add firebase to our project

command line

install sdkk firebase

```
npm install --save firebase
````


click on web setup

dont put it in your index.html file

```
apiKey: "AIzaSyAGalWEXDx6qBAMTedbsxfZazO4txKTZww",
authDomain: "ionic2-recipebook-ddf60.firebaseapp.com",
```

go back to your app.component.ts

we initialize the firebaseapp in our constructor

```js
import firebase from 'firebase';
```

```js
constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen, private menuCtrl: MenuController) {
    firebase.initializeApp({
      apiKey: "AIzaSyAGalWEXDx6qBAMTedbsxfZazO4txKTZww",
      authDomain: "ionic2-recipebook-ddf60.firebaseapp.com"
    });
```

we pass the config options, you coud pass the full object from web setup

we do this in the beginning of the app,in the constructor of the app, we can use the features of firebase


# 152. Implementing the Signup Process

start working on our app

add new services

auth.ts

```js
import firebase from 'firebase';

export class AuthService {
    signup(email: string, password: string) {
        return firebase.auth().createUserWithEmailAndPassword(email, password);
    }
}
```

you could send an http request, to your own restful api

basically, fib does the same.

creates a user on the server

the result will be an error if the address is already taken

i want to send this request from the singup page

in order to use the auth service, we need to provide it in app.module.ts

```js
providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    ShoppingListService,
    RecipesService,
    AuthService
  ]
  ```


signup.ts

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { AuthService } from '../../services/auth';

@Component({
  selector: 'page-signup',
  templateUrl: 'signup.html',
})
export class SignupPage {
  constructor(private authService: AuthService) {}

  onSignup(form: NgForm) {
    this.authService.signup(form.value.email, form.value.password);
  }
}
```

the result is a promise!

resolved if succes, rejected if not

then, if it success

```js
  onSignup(form: NgForm) {
    this.authService.signup(form.value.email, form.value.password)
      .then(data => console.log(data))
      .catch(error => console.log(error));
  }
```

show some loading screen

# 153. Showing a Loader (Spinner) and Error Alert

spinner, signup.ts

ionic2 has a component for this, its called loadingController


```js
constructor(
    private authService: AuthService,
    private loadingCtrl: LoadingController
  ) {}
```

```js
onSignup(form: NgForm) {
  const loading = this.loadingCtrl.create({
    content: 'Signing you up...'
  });
```

we wanna dismiss it in the then and in the catch

```js
this.authService.signup(form.value.email, form.value.password)
  .then(data => {
    loading.dismiss();
  })
```

now for the error, we want to show also an alert

```js
export class SignupPage {
  constructor(
    private authService: AuthService,
    private loadingCtrl: LoadingController,
    private alertCtrl: AlertController
  ) {}
```

```js
.catch(error => {
  loading.dismiss();
  const alert = this.alertCtrl.create({
    title: 'Signup failed',
    message: error.message,
    buttons: ['Ok']
  });
  alert.present();
});
```

# 154. Implementing the Signin Process

start on the auth.ts service

```js
import firebase from 'firebase';

export class AuthService {
    signup(email: string, password: string) {
        return firebase.auth().createUserWithEmailAndPassword(email, password);
    }

    signin(email: string, password: string) {
        return firebase.auth().signInWithEmailAndPassword(email, password);
    }
}
```

signin.ts

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { AuthService } from '../../services/auth';

@Component({
  selector: 'page-signin',
  templateUrl: 'signin.html',
})
export class SigninPage {
  constructor(private authService: AuthService) {}

  onSignin(form: NgForm) {
    this.authService.signin(form.value.email, form.value.password)
      .then(data => {
        console.log(data);
      })
      .catch(error => {
        console.log(error);
      });
  }
}
```

# 155. Refining the Signin Page

signin.ts

```js
export class SigninPage {
  constructor(private authService: AuthService, 
    private loadingCtrl: LoadingController,
    private alertCtrl: AlertController
  ) {}
```

```js
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { AuthService } from '../../services/auth';
import { LoadingController, AlertController } from 'ionic-angular';

@Component({
  selector: 'page-signin',
  templateUrl: 'signin.html',
})
export class SigninPage {
  constructor(private authService: AuthService, 
    private loadingCtrl: LoadingController,
    private alertCtrl: AlertController
  ) {}

  onSignin(form: NgForm) {
    const loading = this.loadingCtrl.create({
      content: 'Signing you in...'
    });
    loading.present();
    this.authService.signin(form.value.email, form.value.password)
      .then(data => {
        loading.dismiss();
      })
      .catch(error => {
        loading.dismiss();
        const alert = this.alertCtrl.create({
          title: 'Signin failed',
          message: error.message,
          buttons: ['Ok']
        });
        alert.present();
      });
  }
}

```

next step, manage the auth state, determine if we are auth or not

split up our app into separete pieces


# 156. Managing the User State

change the page we are viewing, depending if we are authed or not

app.component.ts

here we set our root page

we have the menu which changes our root page

we add a new code to the constructor

```js
constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen, private menuCtrl: MenuController) {
    firebase.initializeApp({
      apiKey: "AIzaSyAGalWEXDx6qBAMTedbsxfZazO4txKTZww",
      authDomain: "ionic2-recipebook-ddf60.firebaseapp.com"
    });
    firebase.auth().onAuthStateChanged();
```

when we switch to auth or nonauth. it gives us the user or null, if we are not authed

```js
export class MyApp {
  tabsPage = TabsPage;
  signinPage = SigninPage;
  signupPage = SignupPage;
  isAuthenticated = false;
  ```

```js
firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.isAuthenticated  =true;
      }
    });
```

```js
firebase.auth().onAuthStateChanged(user => {
  if (user) {
    this.isAuthenticated  =true;
    /// i want to navigate to tabspage
    this.nav.setRoot(this.tabsPage);
  } else {
    this.isAuthenticated = false;
    this.nav.setRoot(this.signinPage);
  }
});
```

ill use isAuthenticated in my app.html, in the menu

show and hide the buttons

```html
<button
    ion-item
    icon-left
    (click)="onLoad(tabsPage)"
    *ngIf="isAuthenticated"
>
    <ion-icon name="book"></ion-icon>
    Recipe Book
</button>
```

```html
<button
    ion-item
    icon-left
    (click)="onLoad(signinPage)"
    *ngIf="!isAuthenticated"
>
    <ion-icon name="log-in"></ion-icon>
    Sign in
</button>
```


```html
<button
    ion-item
    icon-left
    (click)="onLoad(signupPage)"
    *ngIf="!isAuthenticated"
>
    <ion-icon name="person"></ion-icon>
    Sign up
</button>
```


```html
<button
    ion-item
    icon-left
    (click)="onLogout()"
    *ngIf="isAuthenticated"
>
    <ion-icon name="log-out"></ion-icon>
    Logout
</button>
````

implement logout button

# 157. Logging Users Out & Fixing a Bug

auth.ts

```js
logout() {
    firebase.auth().signOut();
    // deletes our token
}
```

app.component.ts

```js
constructor(
    platform: Platform, 
    statusBar: StatusBar, 
    splashScreen: SplashScreen, 
    private menuCtrl: MenuController,
    private authService: AuthService
  ) {
```

```js
  onLogout() {
    this.authService.logout();
  }
```

something is wrong

app.component.ts

when we watch the onAuthStateChanged

we call this nav.setroot

nav grabs the navcontroller from our template

we are in our constructor, onauthstatechange fires the event asynchronoiulsy

its very fast

our template, has not been initialized
our navigation stack has not been initiliazed

viewchild will not have grade our navigation from the template

change tabspage for rootPage: any

```js
export class MyApp {
  rootPage: any = TabsPage;
```

app.html

```html
<ion-nav [root]="rootPage" #nav></ion-nav>
```

```html
<button
    ion-item
    icon-left
    (click)="onLoad(rootPage)"
    *ngIf="isAuthenticated"
>
    <ion-icon name="book"></ion-icon>
    Recipe Book
</button>
```

initalie that will hold the tabsPage

i will change dynamically rootPage

app.component.ts

```js

firebase.auth().onAuthStateChanged(user => {
  if (user) {
    this.isAuthenticated = true;
    /// i want to navigate to tabspage
    // this.nav.setRoot(this.tabsPage);
    this.rootPage = TabsPage;
  } else {
    this.isAuthenticated = false;
    this.nav.setRoot(this.signinPage);
    this.rootPage = SigninPage;
  }
});
```

it wont navigate us there

it will set it to this page behind the scence
this runs for the first time

it makes sure that our navigation stack is set with the first page

# 158. How Firebase stores the Token

fib stores the token in the local storage of our browser


application

local storeage

localhost8100

important note:

cordova will compile the localstorage access code such that it picks an appropiate storage on the device

i dont see the token!!!

redirection

changing the rootpage alone, it does not trigger change detection

any page transition

in order to navigate away

```js
onLogout() {
  this.authService.logout();
  this.menuCtrl.close();
  this.nav.setRoot(SigninPage);
}
```

# 159. Adding a popover Component

save a shopping list

dropdown menu to chose if we load or save the data

ionic docu
components
popover

we need a page which serves as popover

like the modal, it has a page behind the scence

i will create ths page in a subfolder, to the shopping list folder

sl-options folder
sl-options.ts


```js
import { Component } from "@angular/core";
import { ViewController } from "ionic-angular";

@Component({
    selector: 'page-sl-options',
    template: `
        <ion-grid text-center>
            <ion-row>
                <ion-col>
                    <h3>Store & load</h3>
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    <button ion-button outline (click)="onAction('load')">Load List</button>
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    <button ion-button outline (click)="onAction('store')">Save List</button>
                </ion-col>
            </ion-row>
        </ion-grid>
    `
})

export class SLOptionsPage {
    constructor(private viewCtrl: ViewController) {}
    onAction(action: string) {
        this.viewCtrl.dismiss({action: action});
    }
}
```

responsible for executing the correct action

i want to dismiss the popover

dismiss a popover is like a modal. its simlpe an overlay to the current page
we dismiss, but we pass our action

we can't see the popover

shopping-list.html

i want a button that opens this popover

```html
<ion-navbar>
  <ion-buttons start>
    <button ion-button icon-only menuToggle>
      <ion-icon name="menu"></ion-icon>
    </button>
  </ion-buttons>
  <ion-buttons end>
    <button ion-button icon-only (click)="onShowOptions()">
      <ion-icon name="more"></ion-icon>
    </button>
  </ion-buttons>
  <ion-title>Shopping List</ion-title>
</ion-navbar>
```

shopping-list.ts

```js
  onShowOptions() {
    // iwant to present the popover
  }
  ```

we need a controller for the popover

```js
constructor(
  private slService: ShoppingListService,
  private popoverCtrl: PopoverController
) {}
```

```js
  onShowOptions() {
    // we pass the page we wanna load
    const popover = this.popoverCtrl.create(SLOptionsPage);
  }
```


add page in declarations array

and entry components

we are not loading the page through the selector or angular2 router

now we present it

```js
onShowOptions() {
  // we pass the page we wanna load
  const popover = this.popoverCtrl.create(SLOptionsPage);
  popover.present();
}
```

it does not look like a popover, just like a dialog

the popover does not know, where is the button that triggered the ppover

shopping-list.html

```html
<ion-buttons end>
  <button ion-button icon-only (click)="onShowOptions($event)">
    <ion-icon name="more"></ion-icon>
  </button>
</ion-buttons>
```

on the present method i need to pass some arguments

```js
onShowOptions() {
  // we pass the page we wanna load
  const popover = this.popoverCtrl.create(SLOptionsPage);
  popover.present();
}
```


this allows the popover to place in the btn, because the mouseevent has teh coordinates

```js
onShowOptions(event: MouseEvent) {
  // we pass the page we wanna load
  const popover = this.popoverCtrl.create(SLOptionsPage);
  popover.present({ev: event});
}
```

# 160. Fetching the Token

in firebase, in the database

```
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

store our data in server!

we will need the token, to auth the user with fib

in firebase, database, just authenticated users are allowed to write data

we need to get our token and send it with the http request

this user is extracted in the app.component.ts

in onAuthStateChanged

we also can get the user with a helper method from firebase

auth.ts

```js
getActiveUser() {
    return firebase.auth().currentUser;
}
```

shopping-list.ts

in order to handle store click

i need to listen the data i get when we dismiss the popover. a string, load or store

if its store, i need to get my current user and get the token

inject the authService

```js
constructor(
  private slService: ShoppingListService,
  private popoverCtrl: PopoverController,
  private authService: AuthService
) {}
```

```js
onShowOptions(event: MouseEvent) {
  // we pass the page we wanna load
  const popover = this.popoverCtrl.create(SLOptionsPage);
  popover.present({ev: event});
  popover.onDidDismiss(data => {
    if (data.action == 'load') {

    } else {
      // returns a promise
      // it checks if the token has expired. if so, it refreshes
      this.authService.getActiveUser().getIdToken()
        .then(
          (token: string) => {
            // to be done
          }
        );
    }
  });
}
```

getToken returns a promise, get token has a look at the local storage an retrieve the token. it checks if the token is still valid. if it expires, it fetches a new one.

storing shopping list data, sounds like a job for the service/shopping-list.ts

there we will implement the http request

# 161

Ionic 3 and Http
Section 7, Lecture 161
If you're using Ionic 3 (check the package.json  file to find out which version you're using), you'll need to add the HttpModule  to your imports[]  array in the AppModule  to get Http functionality to work.

Don't forget to also add the import statement to the top of the AppModule file:

import { HttpModule } from '@angular/http'; 

You can stop at this point if you're using Angular up to version 5 (the latest version until now).

-------

When using Angular 5+, you can also use the new HttpClient  it offers (this will be mandatory once Angular 6 is released).

Here's how you use it:

import { HttpClientModule } from '@angular/common/http'  

instead of

import { HttpModule } from '@angular/http'; 

Hence, you should add HttpClientModule  to imports[]  in your AppModule  (instead of HttpModule  as described above).

In the files where you inject Http , you should now inject HttpClient .

To get access to this type/ class, you have to change the import from

import { Http } from '@angular/http'; 

to

import { HttpClient } from '@angular/common/http'; 

Generally, you use HttpClient  in exactly the same way, though one change is required: HttpClient  extracts the data you get with the response automatically. Hence you don't need to map()  it manually anymore.

So the following code

this.http.get('my-url').map(res => res.json()) 

will become

this.http.get('my-url') 

That's it. With these steps, you're using the new HttpClient and everything should work just as shown in the videos!

# 162. Sending a PUT request with the Auth Token

```js
  storeList(token: string) {
    
  }
```

```js
import { Injectable } from "@angular/core";

@Injectable()
```

```js
constructor(private http: Http) {}
```

```js
import { Http } from "@angular/http";
```

in the browser, to know the version of ionic

```
Ionic.version
"3.9.2"
```

if we have ionic > 3 and angular > 5

app.module.ts

```js
imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp),
    HttpModule
  ],
```

```js
import { HttpModule } from '@angular/http';
```

now, in storelist, i will use the service to put some data in the database
i want to overwrite the old data

we need to have the address of our database

in our database, data, the link

https://ionic2-recipebook-ddf60.firebaseio.com/


i want to store the data in a folder in the current user


we have a db like mongo, we have like js objects

each user has its own node

```js
  storeList(token: string) {
    this.http.put('https://ionic2-recipebook-ddf60.firebaseio.com/', );
  }
  ```

lets inject the AuthService

```js
constructor(private http: Http, private authService: AuthService) {}
```

store the user id

we need to use .json. you can name the file whatever

as a body, we send this.ingredients

```js
storeList(token: string) {
  const userId = this.authService.getActiveUser().uid;
  this.http.put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/shopping-list.json', this.ingredients);
}
```

we got an observable back

```js
  storeList(token: string) {
    const userId = this.authService.getActiveUser().uid;
    return this.http
      .put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/shopping-list.json', this.ingredients);
  }
  ```

we need to subscribe

i do wanna map any potential response
Response is imported from 

```js
import { Http, Response } from "@angular/http";
```

we will take care in the page where we are returning this

```js
  storeList(token: string) {
    const userId = this.authService.getActiveUser().uid;
    return this.http
      .put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/shopping-list.json', this.ingredients)
      .map((response: Response) => {
        return response.json();
      })
  }
```

if i want to use map, i need to unlock it 

```js
import 'rxjs/Rx'
```

we are not using the token!

we are just using the userId

we should inform dfirebase that we have a token

```js
storeList(token: string) {
    const userId = this.authService.getActiveUser().uid;
    return this.http
      .put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/shopping-list.json?auth=' + token, this.ingredients)
      .map((response: Response) => {
        return response.json();
      })
  }
```

now we can subscribe to the obserbale this method returns

in a success case, i want to log success, in error, log the error

shopping-list.ts

```js
this.authService.getActiveUser().getIdToken()
.then(
  (token: string) => {
    this.slService.storeList(token)
      .subscribe(
        () => console.log('Success!'),
        error => {
          console.log(error);
        }
      );
  }
);
```

the new node is the userid

# Section 7, Lecture 163
The token we use here is a JWT => JSON Web Token. You learn more about this technology in the following article: https://jwt.io/

During the compilation process, Cordova will convert our code such that it actually is not stored in localStorage  (we don't have that on a mobile device) but in an appropriate other space, preferably a SQLite database.


# 164. Sending a GET Request to load Data

we want to get this data back

services/shopping-list.ts

i wanna add the map method, to transform the data a bit, i will get the response, which is type response
simply extract the body of this response and turn it into a js object

do operator, will simply run on the result of this observable.

```js
fetchList(token: string) {
  const userId = this.authService.getActiveUser().uid;    
  return this.http.get('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/shopping-list.json?auth=' + token)
    .map((response: Response) => {
      return response.json();
    })
    .do((data) => {
      this.ingredients = data
    });
}
```

shopping-list.ts

```js
if (data.action == 'load') {
  this.authService.getActiveUser().getIdToken()
    .then(
      (token: string) => {
        this.slService.fetchList(token)
          .subscribe(
            (list: Ingredient[]) => {
              if (list) {
                this.listItems = list;
              } else {
                this.listItems = [];
              }
            },
            error => {
              console.log(error);
            }
          );
      }
    );
}
```

we are updating the listItems, but we are not triggering the pageChanges

# 165. Polishing the App (Adding a Spinner and Error Handling)

show a loader while we are loading

shopping-list.ts

inject `LoadingController` in constructor


```js
onShowOptions(event: MouseEvent) {
  const loading = this.loadingCtrl.create({
    content: "Please wait..."
  });
```

```js
if (data.action == 'load') {
  loading.present();
```

```js
} else if (data.action == 'store') {
  loading.present();
```

i want to dismiss the loader in the success case

```js
.subscribe(
  (list: Ingredient[]) => {
    loading.dismiss();
    if (list) {
      this.listItems = list;
    } else {
      this.listItems = [];
    }
  },
  error => {
    loading.dismiss();
    console.log(error);
  }
);
```

```js
else if (data.action == 'store') {
  loading.present();
  this.authService.getActiveUser().getIdToken()
    .then(
      (token: string) => {
        this.slService.storeList(token)
          .subscribe(
            () => loading.dismiss(),
            error => {
              loading.dismiss();;
            }
          );
      }
    );
```


what happens if we get an error

i want to show an alert

we create a new method

```js
private handleError(errorMessage: string) {
  
}
```

i need an AlertController


```js
private handleError(errorMessage: string) {
  const alert = this.alertCtrl.create({
    title: 'An error occured!',
    message: errorMessage,
    buttons: ['Ok']
  });
  alert.present();
}
```

this is how i want to handle my errors

in the error callbacks, i call the method

```js
this.slService.storeList(token)
.subscribe(
  () => loading.dismiss(),
  error => {
    loading.dismiss();
    this.handleError(error.message);
  }
);
```

```js
this.slService.fetchList(token)
.subscribe(
  (list: Ingredient[]) => {
    loading.dismiss();
    if (list) {
      this.listItems = list;
    } else {
      this.listItems = [];
    }
  },
  error => {
    loading.dismiss();
    this.handleError(error.message);
  }
);
```

the dismiss, it triggers the page change


# 166. Fixing the error handler

in my case it worked... i dont know why, but

Unfortunately, the error handler won't correctly in its current state.

Right now, we try to extract the error message like this:

.subscribe(
  ...,
  error => {
    loading.dismiss();
    this.handleError(error.message);
  }
);
But this actually has two problems:

error  still is the original Response  object - we first have to extract the body  (and convert it to a JS object) via json() .
The actual message is not stored in a property named message  but in a property named error .
The correct code therefore looks like this:

.subscribe(
  ...
  ,
  error => {
    loading.dismiss();
    this.handleError(error.json().error);
  }
);
Replace the code in the shopping-list.ts and the recipes.ts file and you should see the actual error messages.


# 167. Storing and Fetching Recipes

sl-options inside pages directly

we want to reuse the component

we could implemenent different components

rename folder to database-options

rename the file to database-options.ts

rename the page itself

database-options.ts

```js
export class DatabaseOptionsPage {
    constructor(private viewCtrl: ViewController) {}
    onAction(action: string) {
        this.viewCtrl.dismiss({action: action});
    }
}
```

in app.module.ts, also rename

in shopping list page, we were also using it

```js
import { DatabaseOptionsPage } from '../database-options/database-options';
```

```js
const popover = this.popoverCtrl.create(DatabaseOptionsPage);
```

now we can reuse it in our recipes

copy from shopping-list.ts

the onShowOptions, loadItems and handleError methods

to recipes.ts

delete the loadItems method

we should put all of this in a service

```js
constructor (private navCtrl: NavController,
  private recipesService: RecipesService,
  private popoverCtrl: PopoverController,
  private loadingCtrl: LoadingController,
  private alertCtrl: AlertController,
  private authService: AuthService
) {}
```

now we can use this tools to create our popover

dont forget

```js
import { DatabaseOptionsPage } from '../database-options/database-options';
```

show the dropdown, the popover in the template


```html
<ion-buttons end>
  <button ion-button icon-only (click)="onNewRecipe()">
    <ion-icon name="add"></ion-icon>
  </button>
  <button ion-button icon-only (click)="onShowOptions($event)">
    <ion-icon name="more"></ion-icon>
  </button>
</ion-buttons>

```

recipes service

```js
@Injectable()
export class RecipesService {
```

```js
constructor(private http: Http, private authService: AuthService) {}
```

```js
storeList(token: string) {
  const userId = this.authService.getActiveUser().uid;
  return this.http.put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/recipes.json?auth=' + token, this.recipes)
}
```

```js
fetchList(token: string) {
    const userId = this.authService.getActiveUser().uid;
    return this.http.get('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/recipes.json?auth=' + token)   ;     
}
```

unlock some operators

```
import 'rxjs/Rx'
```

```js
import { Http, Response } from "@angular/http";

```

```js
fetchList(token: string) {
    const userId = this.authService.getActiveUser().uid;
    return this.http.get('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/recipes.json?auth=' + token)
        .map((response: Response) => {
            return response.json();
        })
        .do((recipes: Recipe[]) => {
            if (recipes) {
                this.recipes = recipes;
            } else {
                this.recipes = [];
            }
        });    
}
```

in services/shopping-list.ts

```js
.do((ingredients: Ingredient[]) => {
if (ingredients) {
  this.ingredients = ingredients
} else {
  this.ingredients = []          
}
```

services/recipes.ts

```js
storeList(token: string) {
  const userId = this.authService.getActiveUser().uid;
  return this.http.put('https://ionic2-recipebook-ddf60.firebaseio.com/' + userId + '/recipes.json?auth=' + token, this.recipes)
      .map((response: Response) => response.json());
}
```

recipes.ts page

```js
onShowOptions(event: MouseEvent) {
  const loading = this.loadingCtrl.create({
    content: "Please wait..."
  });
  const popover = this.popoverCtrl.create(DatabaseOptionsPage);
  popover.present({ev: event});
  popover.onDidDismiss(data => {
    if (data.action == 'load') {
      loading.present();
      this.authService.getActiveUser().getIdToken()
        .then(
          (token: string) => {
            this.recipesService.fetchList(token)
              .subscribe(
                (list: Recipe[]) => {
                  loading.dismiss();
                  if (list) {
                    this.recipes = list;
                  } else {
                    this.recipes = [];
                  }
                },
                error => {
                  loading.dismiss();
                  // this.handleError(error.message);
                  this.handleError(error.json().error);
                }
              );
          }
        );
    } else if (data.action == 'store') {
      loading.present();
      this.authService.getActiveUser().getIdToken()
        .then(
          (token: string) => {
            this.recipesService.storeList(token)
              .subscribe(
                () => loading.dismiss(),
                error => {
                  loading.dismiss();
                  // this.handleError(error.message);
                  this.handleError(error.json().error);
                }
              );
          }
        );
    }
  });
}
```

if we go to recipes we get an error

our firebase data

it was saved with no ingredients

we dont have the array. its ok if its array

if we open the popover, and we click somehwere else,also an error


# 168. Fixing Errors

if we store a recipe withoug ingredients, and we fetch it, error

adjust the data we get back

check if any recipe dont have the ingredients property


services/recipes.ts

```js
.map((response: Response) => {
    const recipes: Recipe[] = response.json() ? response.json() : [];
    for (let item of recipes) {
        if (!item.hasOwnProperty('ingredients')) {
            item.ingredients = [];
        }
    }
    return recipes;
})
```

recipes.ts

```js
onShowOptions(event: MouseEvent) {
    const loading = this.loadingCtrl.create({
      content: "Please wait..."
    });
    const popover = this.popoverCtrl.create(DatabaseOptionsPage);
    popover.present({ev: event});
    popover.onDidDismiss(data => {
      if (!data) {
        return;
      }
```


shopping-list.ts

```js
onShowOptions(event: MouseEvent) {
const loading = this.loadingCtrl.create({
  content: "Please wait..."
});
const popover = this.popoverCtrl.create(DatabaseOptionsPage);
popover.present({ev: event});
popover.onDidDismiss(data => {
  if (!data) {
    return;
  }




























































































































































































