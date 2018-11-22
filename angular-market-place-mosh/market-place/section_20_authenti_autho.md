#Authenti author

# 287 Implementing Google Login

authentication paage in firebase

users, sign in method

enable google authetnication

bs-navbar.component.html

```html
<li class="nav-item">
  <a class="nav-link" routerLink="/shopping-cart">Shopping Cart</a>
</li>

<li class="nav-item">
  <a class="nav-link" routerLink="/login">Login</a>
</li>
```

we will show login, or Username

login.component.html

```html
<button 
  class="btn btn-primary"
  (click)="login()"
>
  login with Google
</button>
```

login.component.ts

```js
import { Component } from '@angular/core';
import { AngularFireAuth } from 'angularfire2/auth';
import * as firebase from 'firebase';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {

  constructor(private afAuth: AngularFireAuth) { 
  }

  login() {
    // signInWithRedirect: it redirects to one of the oauth providers, like google, fb
    // we need to pass an instance of the google oauth provider
    this.afAuth.auth.signInWithRedirect(new firebase.auth.GoogleAuthProvider());
    console.log(this.afAuth.idTokenResult);
  }

}
```

we are logged in, we need to hide elements

if we go to firebase, we see the user

lets implement the log out method

# 288 Log out method

bs-navbar.component.html

```html
<a class="dropdown-item" (click)="logout()">Log Out</a>
```

bs.navbar.component.ts

```js
import { Component } from '@angular/core';
import { AngularFireAuth } from 'angularfire2/auth';

@Component({
  selector: 'bs-navbar',
  templateUrl: './bs-navbar.component.html',
  styleUrls: ['./bs-navbar.component.css']
})
export class BsNavbarComponent {

  constructor(private afAuth: AngularFireAuth) { }

  logout() {
    this.afAuth.auth.signOut();
  }
}
```

its working but i dont see it

```js
constructor(private afAuth: AngularFireAuth) {
  // properties of the afAuth object
  // .authState is an observable. we can subscribe
  afAuth.authState.subscribe(x => console.log(x));
}
```

now i see it in the console



# 289 Displaying the current user


bs-navbar.component.ts

afAuth is an obversable of type firebase.User

```js
import * as firebase from 'firebase';
```

```js
export class BsNavbarComponent {
  user: firebase.User;

  constructor(private afAuth: AngularFireAuth) {
    afAuth.authState.subscribe(user => {
      this.user = user;
    })
  }

  logout() {
    this.afAuth.auth.signOut();
  }
  
}
```

bs-navbar.component.html

```html
<li *ngIf="!user" class="nav-item">
  <a class="nav-link" routerLink="/login">Login</a>
</li>
```

# 290 Using the Async Pipe

we should always unsubcsribe from firebase observables

when working with firebase, you are working with asyncronous stream of data, is diffferent of consuming http requests

angular unsubscribes for us, but not for firebase

change type of this user for an Observable

async pipe will unsubscribe when this component is detroyed

```js
import { Component } from '@angular/core';
import { AngularFireAuth } from 'angularfire2/auth';
import * as firebase from 'firebase';
import { Observable } from 'rxjs';

@Component({
  selector: 'bs-navbar',
  templateUrl: './bs-navbar.component.html',
  styleUrls: ['./bs-navbar.component.css']
})
export class BsNavbarComponent {
  user: Observable<firebase.User>;

  constructor(private afAuth: AngularFireAuth) {
    this.user = afAuth.authState;
  }

  logout() {
    this.afAuth.auth.signOut();
  }
  
}
```

bs-navbar.component.html

we need to unwrap our observable

```html
<li ngbDropdown *ngIf="user | async as user" class="nav-item dropdown">
````

but we have two variables with same name, thats why by convention

we add a dollar sign to observables

bs-navbar.component.ts

```js
user$: Observable<firebase.User>;

constructor(private afAuth: AngularFireAuth) {
  this.user$ = afAuth.authState;
}
```

unwrap the observable with async and store it in the variable user

bs-navbar.component.html

```html
<li ngbDropdown *ngIf="user$ | async as user" class="nav-item dropdown">
```

same for the login link

```js
<li *ngIf="!(user$ | async)" class="nav-item">
  <a class="nav-link" routerLink="/login">Login</a>
</li>
```

if the result is undefined or null, we display the link

we can refactor

```html
<ng-template #anonymousUser>
  <li class="nav-item">
    <a class="nav-link" routerLink="/login">Login</a>
  </li>
</ng-template>

<li ngbDropdown *ngIf="user$ | async as user; else anonymousUser" class="nav-item dropdown">
  <a ngbDropdownToggle class="nav-link dropdown-toggle" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    {{ user.displayName }}
[...]
```

with ng-template, and else

# 230 extracting a service

in the log out method, and log in, we have type coupling

testability, unit test, we dont want to go to firebase whhile testing, we want to mok this, we want to pass a fake authentication service

other problem, lack of separation of concerns. our component is tight to the authentication with firebase

the component shouldnt care how the authentication is performed

he should call an authetnication class and ask him, hej, authenticat this user, or log out this user

maybe tomorrow we use auth 0 instead of firebase. we should go to all this place and change it

well encapsulate how auth is done and then re use it

when we unit test, we can pass a fake service

generate a new service

```js
ng g s auth
```

app.module.ts

and register it as a provider

```js
providers: [
  AuthService
],
```

auth.service.ts

in the constructor inject object afAuth: AngularFireAuth, as we did before

move the code from the login and logout past methods

```js
import { Injectable } from '@angular/core';
import { AngularFireAuth } from 'angularfire2/auth';
import * as firebase from 'firebase';

@Injectable({
  providedIn: 'root'
})
export class AuthService {

  constructor(private afAuth: AngularFireAuth) { }

  login() {
    this.afAuth.auth.signInWithRedirect(new firebase.auth.GoogleAuthProvider());
  }

  logout() {
    this.afAuth.auth.signOut();
  }
}
```

here, is implementation details. its all about firebase. 

in login, we have no firebase! we delegate it with the thi.auth.login
login.component.ts


```js
import { Component } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {

  constructor(private auth: AuthService) { 
  }

  login() {
    this.auth.login();
  }
}
```

cleaner code with separation of concerns

in bs-navbar.component.ts

// we need to discribe this observable so that we can subscribe from the outside (authState)

```js
constructor(private auth: AuthService) {
  this.user$ = auth.authState;
}
```

in auth.service.ts

```js
import { Injectable } from '@angular/core';
import { AngularFireAuth } from 'angularfire2/auth';
import * as firebase from 'firebase';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  user$: Observable<firebase.User>
  
  constructor(private afAuth: AngularFireAuth) {
    this.user$ = afAuth.authState;
  }
```



in bs-navbar.component.ts

we dont need

```js
user$: Observable<firebase.User>;
```

because we can access auth

```js
constructor(private auth: AuthService) {
}
```

bs-navbar.component.html

```html
<li ngbDropdown *ngIf="auth.user$ | async as user; else anonymousUser" class="nav-item dropdown">
```

we need to change the access modifier for this field (auth) from private to public

when we build for production, they are expected to be public

bs-navbar.component.ts

```js
  constructor(public auth: AuthService) {
  }
```

# 292 Protecting the routes

some of the route sshould be able to anonymous user

protect routes with guard

```js
ng g s auth-guard
```

in appmodule, register this as a provider

```js
providers: [
    AuthService,
    AuthGuardService
  ],
```

implement canactivate interface 

auth-guard.service.ts

```js
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuardService implements CanActivate {

  constructor(private auth: AuthService, private router: Router) { }

  canActivate() {
    // is the user log in? we need our oauth service
    this.auth.user$.subscribe(user => {
      // if no user, we want to navigate the user to the root page, we need Router
      if (user) return true;

      this.router.navigate(['/login']);
      return false;
    });
  }
}
```


we can not unsubscribe, we are not implementing a component, we odnt have the candestroy interface

in canActivate we need to return a boolean. we are returning a subscription. sometime in the future we will return the true or the false

https://angular.io/api/router/CanActivate

we can return with canActivate

```js
Observable<boolean> | Promise<boolean> | boolean
```

we can return an observable of boolean!

instead of subscribing to this observable, we call the map operator
transform this observable from the user object into a boolean



angular will internally subscribe to this observable, and then, remove the subscription later

with map, we are mapping an observable of firebase user, to an observable of user

this is how you implement an oauth guard

```js
return this.auth.user$.map(user => {
```

```js
import 'rxjs/add/operator/map'
```

in angular 6

```js
import { map } from 'rxjs/operators';
```

```js
return this.auth.user$.pipe(map(user => {
```

appmodule, apply this guard to a few routes

```js
{ path: 'check-out', component: CheckOutComponent, canActivate: [AuthGuardService]},
```


now, if we go to check-out url, we are redirect it to login page

you put the canActivate: [AuthGuardService] to the routes you wanna protect

protect order-success, my/orders, admin/products, admin/orders

# 293 Redirecting user after log in


in auth-guard.service.ts

```js
this.router.navigate(['/login']);
```

in auth-guard, pass a query parameter that determines the return url

canActivate takes two parameters, route and state

```js
canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
  return this.auth.user$.pipe(map(user => {
    if (user) return true;

    this.router.navigate(['/login'], { queryParams: { returnUrl: state.url }});
    return false;
  }));
}
```


```js
import { CanActivate, Router, RouterStateSnapshot, ActivatedRouteSnapshot } from '@angular/router';
```


if we try checkout, and we are not log in

```js
http://localhost:4200/login?returnUrl=%2Fcheck-out
```

this was the first part of the puzzle

when we click login, we lose the query, because we go to google.
we want to store the query in local storage, before we log in, so 

auth.service.ts

```js
constructor(private afAuth: AngularFireAuth, private route: ActivatedRoute) {
```

with this, we can get the current route, and extract the return url aprameter


```js
import { ActivatedRoute } from '@angular/router';
```

```js
login() {
  let returnUrl = this.route.snapshot.queryParamMap.get('returnUrl') || '/';
  localStorage.setItem('returnUrl', returnUrl);
  this.afAuth.auth.signInWithRedirect(new firebase.auth.GoogleAuthProvider());
}
```

last part, when we come from google, we want to read the url from local storage and redirect the user

since 

```js
this.afAuth.auth.signInWithRedirect(new firebase.auth.GoogleAuthProvider());
```

is a promise, we should be able to use then, but somehow does not work

in app.component.ts

we have the root component

we can subscribe to the user observable of our authentication service 

when the user logs in, we can read our local storage and redirect them

```js
import { Component } from '@angular/core';
import { AuthService } from './auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  constructor(private auth: AuthService, router: Router) {
    // subscribe to auth.user, at this point we have a user object
    auth.user$.subscribe(user => {
      if (user) {
        let returnUrl = localStorage.getItem('returnUrl');
        router.navigateByUrl(returnUrl);
      }
    })
  }
}
```

every time the user logs in and logs out, this observable will emit a new value.
if they log out, we dont have a user object, we recieve null

we have subscribed to this observable, but there is no unsubscribe

this subscription will work all the etime, until we log out

# 294 Storing users in Firebase

time to implement authorization, user should not access pages that he can not

in order to give roles to users, we need to store users in our database of firebase

everytime we work with firebase, we need to encapsulate the code, in services

```js
ng g s user
```

service to work with user objects in our db

app module, register this service as a provider

```js
  providers: [
    AuthService,
    AuthGuardService,
    UserService
  ],
```

in user.service.ts

```js
import { Injectable } from '@angular/core';
import { AngularFireDatabase } from 'angularfire2/database';
import * as firebase from 'firebase';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  constructor(private db: AngularFireDatabase) { }

  save(user: firebase.User) {
    this.db.object('/users/' + user.uid).update({
      name: user.displayName,
      email: user.email
    })
  }
}
```

in app.component.ts

```js
auth.user$.subscribe(user => {
  if (user) {
    userService.save(user);
```



now define roles for our users

# 295 Defining Admins

different approach

add a property, isAdmin: true

this approach does not scale well, if we want more roles
isStoreManager

2nd approach
high level node, roles, under it, admins, all the user with this node, then the id, then true or false

roles
  admins
    id true
    id true
  storeManagers


3rd approach who can perform what

operations
  create product
    id true
    id true
    id false
  delete a product
    id true
    id false


first approach

who set the admin? we manually

to albertmontolio, the first user, isAdmin: true, in firebase db

# 296 Protecting the admin routes

we used an AuthGuard to protect routes from anonimous users
now simiilar aproach, but for admins

```js
ng g s admin-auth-guard
```

```js
providers: [
  AuthService,
  AuthGuardService,
  UserService,
  AdminAuthGuardService
],
```

admin-auth-guard.service.ts

```js
import { Injectable } from '@angular/core';
import { CanActivate } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AdminAuthGuardService implements CanActivate {

  constructor(private auth: AuthService) { 
    canActivate() {
      this.auth.user$
        // we get a user object, its a firebase.User
        // is the user represented by firebase, cuz authentication, not the one in the db.
        // it does not have the isAmdin property
        .map(user => {
          
        });
    }
  }
}
```

we need to read the actual user in db

in user.service.ts, lets create this method

```js
get(uid: string) {
  // any is the object user of our db
  return this.db.object('/users/' + uid);
}
```

lets create a user interface

in app folder, folder, models, new model
app-user.ts

```js
export interface AppUser {
    name: string;
    email: string;
    isAdmin: boolean;
}
```


in user.service.ts

```js
get(uid: string): FirebaseObjectObservable<AppUser> {
```

new docu:

```js
get(uid: string): AngularFireObject<AppUser> {
```

admin-auth-guard.service.ts


```js
canActivate() {
  this.auth.user$
    .map(user => {
      // we get a firebase user object, we need to map it to an appuser object
      return this.userService.get(user.uid);
      // we need to return sth
    })
    .subscribe(x => console.log(x));
}
```

we dont get the app user

we switch to an observable of the type that we want




































































































































































