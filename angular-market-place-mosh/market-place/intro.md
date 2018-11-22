# using bootsrap
# hosted in firebase

Source code

https://github.com/mosh-hamedani/organic-shop

# 278 Creating a new project

```js
ng new oshop
```

if this does not work, run 

```js
npm install
```

to install all the dependencies

installed:

```js
@angular/cli@6.0.8
```

```js
ng serve
```

go to localhost:42000

angular project is ready

lets add firebase

create new firebase proejct

oshop

add firebase to your web app

```js
<script src="https://www.gstatic.com/firebasejs/5.1.0/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "AdfasfafasFhMfsasdfaf8zh6c2Y",
    authDomain: "oshop-a5b59.firebaseapp.com",
    databaseURL: "https://oshop-a5b59.firebaseio.com",
    projectId: "oshop-a5b59",
    storageBucket: "",
    messagingSenderId: "412999473771"
  };
  firebase.initializeApp(config);
</script>
```

environment.ts

```js
export const environment = {
  production: false,
  firebase: {
    apiKey: "AIzaSyBkFMAUmCInU-3kFhMuogPiGXX18zh6c2Y",
    authDomain: "oshop-a5b59.firebaseapp.com",
    databaseURL: "https://oshop-a5b59.firebaseio.com",
    projectId: "oshop-a5b59",
    storageBucket: "",
    messagingSenderId: "412999473771"
  }
};
```

same in environment.prod.ts

install packages

```js
npm i --save firebase@4.2.0 angularfire2@4.0.0-rc.1
```

```js
+ angularfire2@4.0.0-rc.1
+ firebase@4.2.0
```

failed to compile

```js
Failed to compile.

./node_modules/angularfire2/auth/auth.js
Module not found: Error: Can't resolve 'rxjs/Observable' in '/Users/albertmontolio/Local_Documents/CodingArea/tutorials/angular/organic-shop-codium/oshop/node_modules/angularfire2/auth'
```

package.json

```js
  "dependencies": {
    "@angular/animations": "^6.0.0",
    "@angular/common": "^6.0.0",
    "@angular/compiler": "^6.0.0",
    "@angular/core": "^6.0.0",
    "@angular/forms": "^6.0.0",
    "@angular/http": "^6.0.0",
    "@angular/platform-browser": "^6.0.0",
    "@angular/platform-browser-dynamic": "^6.0.0",
    "@angular/router": "^6.0.0",
    "angularfire2": "^4.0.0-rc.1",
    "core-js": "^2.5.4",
    "firebase": "^4.2.0",
    "rxjs": "^6.0.0",
    "zone.js": "^0.8.26"
  },
```


In angularfire2 v5.0.0-rc.3 is using rxjs 5.x. You have two way.

1: Upgrade AngularFire2 Libary.

```js
npm install --save angularfire2@latest
```

or,

2: Install rxjs-compat

```js
npm install --save rxjs-compat
```

i do

```js
npm install --save angularfire2@latest
```

result

```js
+ angularfire2@5.0.0-rc.11
```

in package.json

```js
    "angularfire2": "^5.0.0-rc.11",
```

```js
npm install --save firebase@latest
```

result 

```js
+ firebase@5.1.0
```

package.json

```js
  "firebase": "^5.1.0",
```

and now it works...

# 279 Installing Bootstrap 4

```js
npm i --save bootstrap
```


```js
+ bootstrap@4.1.1
```

package.json

```js
    "bootstrap": "^4.1.1",
```

the next unofficial released (beta versions)

npm i --save bootstrap@next


style.css

path to the bootstrap css related form the node modules folder

```js
/* You can add global styles to this file, and also import other style files */
@import "~bootstrap/dist/css/bootstrap.css"
```

go to bootstrap four, template

examples, starter template

right click, view page source

copy this in app.component.html

```html
<nav class="navbar navbar-expand-md navbar-dark bg-dark fixed-top">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExampleDefault" aria-controls="navbarsExampleDefault" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarsExampleDefault">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#">Disabled</a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="http://example.com" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
        <div class="dropdown-menu" aria-labelledby="dropdown01">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </li>
    </ul>
    <form class="form-inline my-2 my-lg-0">
      <input class="form-control mr-sm-2" type="text" placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
    </form>
  </div>
</nav>

<main role="main" class="container">

  <div class="starter-template">
    <h1>Bootstrap starter template</h1>
    <p class="lead">Use this document as a way to quickly start any new project.<br> All you get is this text and a mostly barebones HTML document.</p>
  </div>

</main><!-- /.container -->

```

we basically have two components

styles.css

```css
/* You can add global styles to this file, and also import other style files */
@import "~bootstrap/dist/css/bootstrap.css";

body {
    padding-top: 80px;
}
```

app.component.html

```html
<nav class="navbar navbar-expand-md navbar-light bg-light fixed-top">
```

you can change classes for changing the color of the navbar

# 280 Extracting NavBar component

in the terminal, generate new component

```js
ng g c bs-navbar
```

result

```js
CREATE src/app/bs-navbar/bs-navbar.component.css (0 bytes)
CREATE src/app/bs-navbar/bs-navbar.component.html (28 bytes)
CREATE src/app/bs-navbar/bs-navbar.component.spec.ts (643 bytes)
CREATE src/app/bs-navbar/bs-navbar.component.ts (280 bytes)
UPDATE src/app/app.module.ts (760 bytes)
```

move navbar to bs.navbar.component.html

change the selector for this component

bs-navbar.component.ts

```js
@Component({
  selector: 'bs-navbar',
```

app.component.html

```html
<bs-navbar></bs-navbar>
```

define routes for our application

# 281 Defining the routes

generate a bunch of components.
public area

```js
ng g c home
ng g c products
ng g c shopping-cart
ng g c check-out
ng g c order-success
```

when the user logs in

```js
ng g c my-orders
```

if user is admin under a folder called admin

```js
ng g c admin/admin-products
ng g c admin/admin-orders
```

```js
ng g c login
```

now lets go to app.module and register our routes

app.modules.ts

import router module

```js
import { RouterModule } from '@angular/router';
```

in imports array

```js
RouterModule.forRoot([
      { path: '', component: HomeComponent }
    ])
```

in forRoot, multiple route objects. each rout object should have two properties, path, and component

```js
RouterModule.forRoot([
  { path: '', component: HomeComponent },
  { path: 'products', component: ProductsComponent },
  { path: 'shopping-cart', component: ShoppingCartComponent },
  { path: 'check-out', component: CheckOutComponent },
  { path: 'order-success', component: OrderSuccessComponent },
  { path: 'login', component: LoginComponent },
  { path: 'admin/products', component: AdminProductsComponent },
  { path: 'admin/orders', component: AdminOrdersComponent },
])
```

add router outlet in the template of our app component

app.component.html

```html
<bs-navbar></bs-navbar>

<div class="container">
  <router-outlet></router-outlet>
</div>
```

add some links in navbar

bs-navbar.component.html

```html
<li class="nav-item active">
  <a class="nav-link" routerLink="/">Home <span class="sr-only">(current)</span></a>
</li>
<li class="nav-item">
  <a class="nav-link" routerLink="/shopping-cart">Shopping Cart</a>
</li>
```

# 282 Adding a drop-down menu

if we click, we go to example.com

we need the javascript library bootstrap

we dont want to use bootstrap.js, because it relies on jquery. jquery manipulates the dom. we want angular to manipulate the dom

angular adds an abstraction to the dom

there is a library ng-bootstrap.github.io

https://ng-bootstrap.github.io/#/home

install node package

```js
npm i --save @ng-bootstrap/ng-bootstrap
```

```js
+ @ng-bootstrap/ng-bootstrap@2.1.2
```

Once installed you need to import our main module.

```js
import {NgbModule} from '@ng-bootstrap/ng-bootstrap';
```

in imports array

```js
NgbModule.forRoot()
```

we need to apply directives in bs-navbar.component.html

```html
<li ngbDropdown class="nav-item dropdown">
  <a ngbDropdownToggle class="nav-link dropdown-toggle" href="#" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
  <div ngbDropdownMenu class="dropdown-menu" aria-labelledby="dropdown01">
    <a class="dropdown-item" href="#">Action</a>
    <a class="dropdown-item" href="#">Another action</a>
    <a class="dropdown-item" href="#">Something else here</a>
  </div>
</li>
```

# 283 cleaning up the navbar

bs-navbar.component.html

```html
<li ngbDropdown class="nav-item dropdown">
  <a ngbDropdownToggle class="nav-link dropdown-toggle" href="#" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Username</a>
  <div ngbDropdownMenu class="dropdown-menu" aria-labelledby="dropdown01">
    <a class="dropdown-item" routerLink="/my/orders">My Orders</a>
    <a class="dropdown-item" routerLink="/admin/orders">Manage Orders</a>
    <a class="dropdown-item" routerLink="/admin/products">Manage Products</a>
    <a class="dropdown-item">Log Out</a>
  </div>
</li>
```


# 284 fixing a few minor issues

we have a # in the url when we click the dropdown

delete the href="#" in 

```html
<a ngbDropdownToggle class="nav-link dropdown-toggle" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Username</a>
```

icon of the cursor

bs-navbar.component.scss

```js
.dropdown-toggle {
    cursor: pointer;
}
```


```html
<nav class="navbar navbar-expand-md navbar-light bg-light fixed-top">
  <a class="navbar-brand" routerLink="/">oShop</a>
```

my orders link does not work

app.module.ts in RouterModule.forRoot

```js
{ path: 'my/orders', component: MyOrdersComponent },
```

if we type a url that does not exist, we go to the home page



# 285 Deploy to firebase

you have to have firebase tools

in the terminal

firebase --version
mosh has 3.9.2

i had, 3.18.5

if you dont have it or its old, install

```js
npm i g firebase-tools
```

```js
+ firebase-tools@3.19.1
+ g@2.0.1
```



first we need to login

```js
firebase login
```

```js
Already logged in as albert.montolio@gmail.com
```



```js
firebase init
```

select hosting

select your project (oShop)


```js
? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? No
✔  Wrote public/404.html
✔  Wrote public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

firebase.json

```js
{
  "hosting": {
    "public": "dist", //OLD: folder that includes our compiled application. when we compile with cli, the result is stored in this folder
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ]
  }
}
```

in angular 6

```js
"public": "dist/oshop",
```

now, build application for production

```js
ng build --prod
```

```js
chunk {0} runtime.a66f828dca56eeb90e02.js (runtime) 1.05 kB [entry] [rendered]
chunk {1} styles.e3d736209321efe6d945.css (styles) 128 kB [initial] [rendered]
chunk {2} polyfills.2f4a59095805af02bd79.js (polyfills) 59.6 kB [initial] [rendered]
chunk {3} main.80c9e70a1ec6868c0661.js (main) 745 kB [initial] [rendered]
```

lets deploy this to firebase

```js
firebase deploy
```

```js
Deploying to 'oshop-a5b59'...

i  deploying hosting
i  hosting: preparing dist directory for upload...
⚠  Warning: Public directory does not contain index.html
✔  hosting: 7 files uploaded successfully

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/oshop-a5b59/overview
Hosting URL: https://oshop-a5b59.firebaseapp.com
```

put the hosting uRL in the browser











































































































































