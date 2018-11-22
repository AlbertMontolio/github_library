# Section 20: Authentication & Route Protection in Angular Apps

# 253. Module Introduction

i want to add authentication

a big chunk of authentication happens on the backend

in firebase its easy to enable authentication

we will focus on authentication

only allow sign in users

# 254. How Authentication Works in Single-Page-Applications

- traditional web app, fullstack app, where your server renders views, which are the html code returned to the client
we also have client and server, but we have a strong connection between the two.
the user sees what the server generated
you use some template engine to render some views, in the end, the client, recieves the finished html code

if we want to authenticate, the client sends the information, the server will validate, and the server will create a session, and store that session on the server
the server is able to communicate with the client, because the server is communicating with the client all the time

therefore, the session is stored in the server, for examle on a db, , and the client gets a session cockie
which mainly containsss the id of thesession, which will be sent with every request, when you want to access 
some sensible information, which may be protetected

server checks, if the id is correct.

this is working, because we have this close connection beteween server and front end





- SPA. we only recieve one page, and the client, angular, its responsible to change the content of the page dynamically

here, we dont have this strong connecction

we may reach out for a server from time to time, but that is not guaranteed


the request is handle by the client entirely

if. list has to be render and the info is already stored in the client

it wont reach out to the backend

the html code will be changed by angular, that is all angular is all about

therefore we dont have this strong connection

besides that, maybe we have a very generic  abckend, exposing a restful api, to which we can connect from serveral web apps and mobile apps

in this case, the server will not require who the client is

if you want to authenticate , you still need to send the auth information

the server will check it in the db

if its valid, it wont create a session

because again, it does not communicaate with the client regularly

so, still, we want to give the client something which allows the client kind of inform the server on future request

that user is logged in

othwersiwe, the user would have to enter the login information on every request he makes, for protectited resources

therefore, the solution is, the server will send back a token, a json

a web token, which encodes some informatin about the authenticated user, nothing sensible

and is hashed. with a certain alogirthy and secret, only known by the server

the client has now this token, if we now want to access some resources on the server which is protected

we would simply attached that token to the request

since the token was generated on the server, and we know the secret &

algorithm, the server is able to validate the token 

therefore, we authenticate via this token on future requests

cuz we attach this token, and server is able to 
check if the token is valid

that is the approach we need to take now, firebase makes this approach very easy

if you are using a nother back end, you need to somehow store the token in the client and attach it to future requests


# 255. More about JWT

Want to learn about the Token which is exchanged? 

The following page should be helpful: https://jwt.io/ - specifically, the introduction: https://jwt.io/introduction/

# 256. Creating a Signup Page and route

create a sign up sign in page

two new components

in the terminal. all in an auth folder

```js
ng g c auth/signup --spec false

ng g c auth/signin --spec false
```


add a form to allow user to sign up sign in

signup.component.html

```html
<div class="row">
  <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offet-1 col-md-offset-2"> 
    <form (ngSubmit)="onSignup(f)" #f="ngForm">
      <div class="form-group">
        <label for="email">Mail</label>
        <input type="email" id="email" name="email" ngModel class="form-control">
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" ngModel class="form-control">
      </div>
      <button class="btn btn-primary" type="submit">Sign up</button>
    </form>
  </div>
</div> 
```

signup.component.ts

```js
onSignup(form: NgForm) {
  const email = form.value.email;
  const password = form.value.password;
}
```

we need to load the form, the component

app-routing.module.ts

```js
{ path: 'signup', component: SignupComponent },
```

working

i want to add a link in the header

```js
<ul class="nav navbar-nav navbar-right">
	<li><a routerLink="/signup">Register</a></li>
```

lets enable authentication on our backend,and learn how to send a request to create a new user

# 257. Setting up the Firebase SDK

firebase bacend, we have an authentication area

right now, is not enabled

we can choose from some providers

enable email/password

now we can start sending this requests, from the frontend

the authentication related things, we will use the firebase sdk

in your app, you will use the normal http service, we use before, to send request with the authentication data
you will need to store your token in the local storage manually

to use firebase with the sdk

we can simply install the firebase package

```js
npm install --save firebase
```


in the auth file, we need some extra service

auth/auth.service.ts

the method i want to call to sign a user up


```js
export class AuthService {
  signupUser(email: string, password: string) {

  }
}
```

in order to use firebase sdk

we need to configure it, when our app starts

in the app.comonent.ts, in the OnInit hook

and import firebase

```js
import * as firebase from 'firebase'
```

```js
ngOnInit() {
  firebase.initializeApp();
}
```

expects to get a js object as an argument

this object, we can get it from the backend

in the web setup, top right corner

we just need the apiKey and the authDomain

```js
ngOnInit() {
  firebase.initializeApp({
    apiKey: "Aaaaaaaaaaaaaa-sY",
    authDomain: "naaaaaaaaaaaafirebaseapp.com",
  });
}
```


now lets implement the sign up method

# 258. Use getIdToken() instead of getToken()

Important note: If you're using Firebase 5.x or higher (you can check the package.json  file to find out), you should use getIdToken()  for obtaining the token, NOT getToken()  as shown in the next lectures.

# 259. Signing Users Up

lets allow the user to sign up

auth.service.ts

```js
import * as firebase from 'firebase'

export class AuthService {
  signupUser(email: string, password: string) {
		
  }
}
```

auth, its a method, that gives us access to important methods

```js
import * as firebase from 'firebase'

export class AuthService {
  signupUser(email: string, password: string) {
		firebase.auth().createUserWithEmailAndPassword(email, password)
  }
}
```

this is a promise, we can listen to the response in the then block.

i want to see the errors

```js
firebase.auth().createUserWithEmailAndPassword(email, password)
	.catch(
		error => console.log(error)
	)
```

we are almost there, we need to call signUpUser, whenever we click the sign up button

we need to provide the AuthService

app.component.ts

```js
providers: [ShoppingListService, RecipeService, DataStorageService, AuthService],
```

signup.component.ts

where we have the onSignup method

we inject the auth service

```js
constructor(
  private authService: AuthService
) { }
```


we can simply reach out to it, 

```js
onSignup(form: NgForm) {
  const email = form.value.email;
  const password = form.value.password;
  this.authService.signupUser(email, password);
}
```

working, we see the user

firebase requires 6 characters, you can add some validation in the form

we can create new users

lets then add the sign in page and allow users to sign in

# 260. Signin Users in

copy the form from my signup component, to the signin component


```js
<div class="row">
  <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offet-1 col-md-offset-2"> 
    <form (ngSubmit)="onSignin(f)" #f="ngForm">
      <div class="form-group">
        <label for="email">Mail</label>
        <input type="email" id="email" name="email" ngModel class="form-control">
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" ngModel class="form-control">
      </div>
      <button class="btn btn-primary" type="submit">Sign in</button>
    </form>
  </div>
</div> 
```

we need a method in the auth.service.ts

a signinuser method

we recieve an email and a password

firebase.auth().signInWithEmailAndPassword

```js
signinUser(email: string, password: string) {
  firebase.auth().signInWithEmailAndPassword(email, password);
}
```

we could return the promsie to handle it in the component, or handle it here with the catch, to see errors

now, lets use then.
what we get back in the then block

```js

signinUser(email: string, password: string) {
  firebase.auth().signInWithEmailAndPassword(email, password)
    .then(
      response => console.log(response)
    )
    .catch(
      error => console.log(error)
    );
}
```

signin.compoment.ts

```js
onSignIn(form: NgForm) {
  const email = form.value.email;
  const password = form.value.password;
  this.authService.signinUser(email, password)
}
```

we need a route to load the signin component

app-routing.module.ts

```js
{ path: 'signin', component: SigninComponent }
```

header.component.html

```html
<li><a routerLink="/signin">Sign in</a></li>
```



it works, the response holds the token with which we are able to identify ourselves

this token, could be store by ourselfs manually, but we dont need to do that, cuz firebase automatically stores the token for us

when we call the sign in method

application >> local storage

we see the token, we see the expiration time, accessToken we see also

we can access our local storage if we want

we will use this token to authenticate ourselfes

# 261. Requiring a token (on the backend)

last lecture, how to sign a user in, we get a response with the token

now i want to use the token

i want to make sure, that saving and fetching data, its only possible if i send the token to the backend

on the backend i want to check if the user is authenticate

rules on the backend

in the rules

```js
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```


only authenticate users are allowed to access our content

if we click save or fetch data, we get an unauthorized exception

but, are we not authorize? we just did logged in, and we did store the token

but we are not sending the token with the request

hej, server, we get a token, if you like it, give me access

we need to change the way we send our request

we need to get access to that token

# 262. sending the token

we eed to retrieve the token

in the auth.service.ts

i want to return sth that firebase gives me back

auth(). we have a current user object, we can call getIdToken

this doesnt give us the token synchronously but asynchronoulsy

why? firebase, behind the scenes, not only retrieve the token from the local storage

but it will automatically check if the token is valid (if it expired), it will give us an ew one

we return a promise. 

```js
getToken() {
  return firebase.auth().currentUser.getIdToken();
}
```

bu tlets use it

in the data-storage-ts

we can inject serviceses

inject the authsERVICE, we need the token

```js
export class DataStorageService {
  constructor(
    private http: Http,
    private recipeService: RecipeService,
    private authService: AuthService
  ) {}
```

when getting recipes, we need to attach this token

first, we need to take the token

the token, is  a stirng.
the issue, 

```js
getRecipes() {
  let tk = '';
  this.authService.getToken()
    .then(
      (token: string) => {
        tk = token;
      }
    );
```

if we use the token after this call
that wont work. the then work is not executed synchronusly, that would happen only when the data is returned

the token will not be immediately available once we execute the get request

we should execute this get request insead of this callback

but that, gives us the problem, if we want to return this observable, like we did in store recipes, we cand do it, because the observable is inside of a callback

what can we do?

of way of solving this issue, go wack to the auth.service.ts

```js
token: string;
```

here i want to set the token

i will do this right at the beginning, when we sign the user in

```js
signinUser(email: string, password: string) {
    console.log(email, password)
    firebase.auth().signInWithEmailAndPassword(email, password)
      .then(
        response => console.log(response)
      )
```

to 

```js
signinUser(email: string, password: string) {
  console.log(email, password)
  firebase.auth().signInWithEmailAndPassword(email, password)
    .then(
      response => {
        firebase.auth().currentUser.getIdToken()
          .then(
            (token: string) => this.token = token
          )
      }
    )
```


with that, i ensure that i have a token here

and if we call get token, i reset the token

but i wll not wait for that to finish, i will return the token



```js
getToken() {
  return firebase.auth().currentUser.getIdToken();
}
```

to

```js
getToken() {
  firebase.auth().currentUser.getIdToken()
    .then(
      (token: string) => this.token = token
    );
  return this.token;
}
```

yes there is the danger of returning an expired token. then, some logic to the user, please try it again

when we sign in, we store the token

when we fetch for recipes, we fetch the token, if it takes too long, we return the this.token

data-storage.ts

```js
getRecipes() {
  let tk = '';
  this.authService.getToken()
    .then(
      (token: string) => {
        tk = token;
      }
    );
```

to

```js
getRecipes() {
  const token = this.authService.getToken();
```


to authenticate ourselfs to the backend, we need to add a query parameter 

tht query parameter firebase will recognize, is auth

```js
getRecipes() {
    const token = this.authService.getToken();
      
    this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
      .pipe(
```

i will log in again, to get a new token, and fetch recipes

its working

lets add the same for storeRecipes

data-storage.service.ts

```js
storeRecipes() {
  const token = this.authService.getToken();

  return this.http.put(
    'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
    this.recipeService.getRecipes()
  );
}
```

# 263. Checking and Using Authentication Status

i want to make sure, that we only see the manage button if we are logged in

i only want to allow access to the new recipe, or the edit recipe route, if we are logged in to
we will add guards


auth.service.ts

i wnat to add a method which ehlps me to find out wheather i am authenticataed or not

```js

isAuthenticated() {
  return this.token != null;
}
```

header.component.ts

```js
export class HeaderComponent {
  constructor(
    private dataStorageService: DataStorageService,
    private authService : AuthService
  ) {}
```

header.component.html

i will wrap my li items in a ng-template

i dont want to wrap them in a div, because that would destroy the layout

ngIf without the star, not using the nicer sintax, because here i want to wrap multiple element sin one ngif condition

i want to check if authservice.isauthenticataed is false


```js
<ul class="nav navbar-nav navbar-right">
  <ng-template [ngIf]="!authService.isAuthenticated()">
    <li><a routerLink="/signup">Register</a></li>
    <li><a routerLink="/signin">Sign in</a></li>
  </ng-template>
```

```js
<li class="dropdown" appDropdown *ngIf="authService.isAuthenticated()">
```

if we reload the app, we need to log in again

its ok

we need a log out button, which destroy the token

# 264. Adding a Logout Button

leave the app

auth.service.ts

firebase.auth().sim


```js
logout() {
  firebase.auth().signOut();
  this.token = null;
}
```

log out should be called by the header component

header.component.html

```html
<ul class="nav navbar-nav navbar-right">
  <ng-template [ngIf]="!authService.isAuthenticated()">
    <li><a routerLink="/signup">Register</a></li>
    <li><a routerLink="/signin">Sign in</a></li>
  </ng-template>

  <li><a style="cursor: pointer;" (click)="onLogout()"></a></li>
```

header.component.ts

```js
onLogout() {
  this.authService.logout();
}
```

only show this link if we are authenticated

```html
<li>
  <a style="cursor: pointer;" (click)="onLogout()" *ngIf="authService.isAuthenticated()">
    Logout
  </a>
</li>
```

i want to make sure that the routing is inline with the sign out status

if i log in, i want to be redirected and not stay on this page

# 265. Route protection and redirection example

if we sign in, i want to redirect the user

signin.component.ts

redirect, just if the signin was successful

auth.service.ts

we have the signinuser method

i want to inject the router

```html
constructor(private router: Router) {}
```

we need @Injectable

in the response of the signinuSER method, we know that we are signed in

so its here, where i want to redirect the user

```js
signinUser(email: string, password: string) {
  console.log(email, password)
  firebase.auth().signInWithEmailAndPassword(email, password)
    .then(
      response => {
        this.router.navigate(['/']);
        firebase.auth().currentUser.getIdToken()
          .then(
            (token: string) => this.token = token
          )
      }
    )
    .catch(
      error => console.log(error)
    );
}
```

lets try this out

now, i want to
make sure, that i cant visit the new reicpe, or edit recipe page, if i am not logged in

we will create a new guard

auth-guard.service.ts in the auth folder

we need to return an observbal,e a promise or a boolean

i will return a boolean value, because i plan on injecting the authservice here

```js
@Injectable()
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthService) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {

  }
}
```

return

```js
  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
    return this.authService.isAuthenticated();
  }
```

and this is a boolean

remember?

in auth.service.ts

```js

isAuthenticated() {
  return this.token != null;
}
```

now i want to apply this guard, to the new route, and the idedit route

app-routing.module.ts

```js
{ path: 'new', component: RecipeEditComponent, canActivate: [AuthGuard] },
```

```js
{ path: ':id/edit', component: RecipeEditComponent, canActivate: [AuthGuard] }
```

you have to provide this guard service in the app module

app.module.ts

```js
providers: [ShoppingListService, RecipeService, DataStorageService, AuthService, AuthGuard],
```

lets see if this works

i lcick new recipe and nothing happens

i click edit recipe, nothing

we could redirect in case of a failure in a guard

# 266. Wrap up

firebase is just a dummy solution

# 267. Possible improvements

You can of course improve this app even more. Some ideas:

- Check if a token is present at application startup (check the localStorage manually or use the Firebase SDK to do so - just make sure that you somehow wait for the SDK to finish its initialization)

- Redirect the user if he want to access a protected route (right now, nothing happens) - inject the router and call this.router.navigate(...) to do so

- Redirect the user on logout so that he's not able to stay on pages which are reserved for authenticated users - you can simply inject the router and call this.router.navigate(...) in the logout() method





























































