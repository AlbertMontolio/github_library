# 11 Routing

we built a single page, angular

but we were all the time in the same page

we want /users, for example

we dont want more index.html

angular gives a router, 

for the user, it seems a new page but behind the scenes, its just that the js ins injected and getting rid of parts

# 114. why do we need router

our router needs to know which routes our angular application has

# 115. understanding the example project

app.component.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="nav nav-tabs">
        <li role="presentation" class="active"><a href="#">Home</a></li>
        <li role="presentation"><a href="#">Servers</a></li>
        <li role="presentation"><a href="#">Users</a></li>
      </ul>
    </div>
  </div>
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <app-home></app-home>
    </div>
  </div>
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <app-users></app-users>
    </div>
  </div>
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <app-servers></app-servers>
    </div>
  </div>
</div>
```

we have the links

this routes, are responsible for our whole app

app.module.ts

we configure our app, add our components
also inform angular about the routes our appplication has

above ngmodule

add new constant, appRoutes. should be of type Routes ,needs to be imported

add import { Routes } from '@angular/router'; not needed but good practice

what should this constant hold? an array. we have multiple routes

each route, is a javascript object in this array
we need a path, what gets entered in the url, after the domain

{ path: 'users'} 

that corresponds

localhost:4200/users

dont put the / !

we also need, what should happen when this path is reached.
the action is a component typically. once this path is reached, a certain component should be loaded. this c would be the page

{ path: 'users', component: UsersComponent }


```js
import { Routes } from '@angular/router'

const appRoutes: Routes = [
  { path: 'users', component: UsersComponent }
];
```


```js
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'servers', component: ServersComponent }
];
```


how does angular knows about this constant?

the name apprOUTES, is up to you. this will ignore by angular

we need to register this routes in our app

in imports, we need to add the RouterModule, imported from @angular/router

```js
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule
  ],
```

```js
import { Routes, RouterModule } from '@angular/router'
```


we are adding the routing functinality to our app, but our routes not exists

RouterModule.forRoot()

register some routes for our main app

recevie our appRoutes constant as an argument

```js
imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(appRoutes)
  ],
```

missing piece, we still add our components with their selector

angular does not know where to load the components

instead of selector, we use a directive, router-outlet

it looks like a component, but its just a directive

we delete the 3 components selector with their cols, and we leave one

app.component.html

```html
<div class="row">
  <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
    <router-outlet></router-outlet>
  </div>
</div>
```

this marks the place in our docu where we want the angular router to load the component of the currently selected route


now we just see the homepage

if we type in url /users, we see the users!

# 116. Navigating with Router Links

some working links in the navigation!

add some links

app.component.html

```html
 	<li role="presentation" class="active"><a href="/">Home</a></li>
	<li role="presentation"><a href="/servers">Servers</a></li>
	<li role="presentation"><a href="/users">Users</a></li>
```


now we load the route we want. reloading the app. we dont want that.
for every link, a new request is sended

this is not the best behaviour. it restarts our app in every link

it restarts the state

this is not how we do it

special directive angular gives us

get rid of href attributes. 

routerLink="" parse a string

this will tell angular, will serve as a link at the end, but it will handle it differently

```html
<li role="presentation" class="active"><a routerLink="/">Home</a></li>
<li role="presentation"><a routerLink="/servers">Servers</a></li>
<li role="presentation"><a [routerLink]="['/users']">Users</a></li>
```

routerLink catches the link, prevents the request, it parses the string, and finds a fitting route in the configuration

it does not reload the page

# 118. Understanding navigation paths


if we delete the / in the routerLink, it stills works

servers.component.html

```html
<div class="col-xs-12 col-sm-4">
  <a routerLink="servers">Reload Page</a>
  <app-edit-server></app-edit-server>
  <hr>
  <app-server></app-server>
</div>
```

if we click it, error

it does found a route 
Error: Cannot match any routes. URL Segment: 'servers/servers'

we need 

```html
  <a routerLink="/servers">Reload Page</a>
```

with a relative path, it always appends the path you specify in the router link to the end of your current path, and important here, the current path depends on which component you are currently one

in the navigation we could delete the /, its the root component, it always sits in the localhost4200

one layer down, this page, is only loaded if we are in /servers

if we put servers, it will try to do servers/servers, a relative path

we need /servers


we can also do 

```html
<a routerLink="../servers">Reload Page</a>
```


/ -> after localhost4200. /servers, /users
if you dont put it, it adds to wher you are /s1/s2/servers

absolute paths / at the beginning, appended to the route domain

without /, added to the current url at the end

../ one level app


# Styling Active Router Links

we have noindication of the current active route is

the selected tap is changed

that is just a css thing

app.component.html we have tehe active class

dynamically set this class

we remove it, angular gives us a specifiy directive for this

routerLinkAktive="active"

to the list item, a wrapping element, or to the link itself.

add to all my links

should recieve this active css class when they are clicked, active

```html
<li role="presentation" routerLinkActive="active"><a routerLink="/">Home</a></li>
<li role="presentation" routerLinkActive="active"><a routerLink="/servers">Servers</a></li>
<li role="presentation" routerLinkActive="active"><a [routerLink]="['/users']">Users</a></li>
```

some how home is always marked as active

the routerlinkactive directive

it analysies the current loaded path, checks which links lead to a router, which uses this path

by default, it marks an element as active, if it contains the path you are on

/servers link, /users,

for the / nothing link, is always the case.

/users, we have the / in between

the empty path sgment, is part of all paths

to fix this, we can add some config

on the same element, routerLinkActiveOptions=""

we need property binding, since we are not only passing a string but a js object


[routerLinkActiveOptions]="{exact: true}"

reserverd property, 
tell angular, only add this routerlinkactive class, if the full path, is whatever this link leads do


we changed the default behavior, mark this,if the full path is what we have in our router link

#120. Navigating Programatically


the user clicks a btn, and then we trigger the navigation from our typescript code

home.component.html

```html
<button class="btn btn-primary" (click)="onLoadServers()">Load Servers</button>
```

we could use routerlink, but lets do sth in the ts code

we can inject this router

constructor(private router: Router)

now we can use the router

this.router.navigate()

takesk an argument, allows us to navigate to a new route

it takes an array, you could have the elements of the path

this.router.navigate(['/servers']);

absolute path here

```js
export class HomeComponent implements OnInit {

  constructor(private router:Router) { }

  ngOnInit() {
  }

  onLoadServers() {
    // complex calculation
    this.router.navigate(['/servers'])
  }

}
```

# 121. Using relative paths in programmatic navigation

we want to also, use relative paths there

servers.component.html

```html
  <a class="btn btn-primary" (click)="onReload()">Reload Page</a>
```

servers.component.ts

```js
  constructor(
    private serversService: ServersService,
    private router: Router
  ) { }
```


```js
  onReload() {
    this.router.navigate(['/servers'])
  }
```

it works

if we remove the /, we make it a relative path

and we still are in the servers component

we still dont get an error, before, we got an error with routerlink

it tried servers/servers

why is working fine now, if we use the navigate method?
the navigatemethdo does not know in which router you are currently on

routerlink it knows it

to tell this, where we currently are in, we have to pass a second argument
{}one config is the relativeTo: property
we define, relative to which route this link should be loaded, by default its the root domain

we need to give a route though, not a string

we can inject a route

private route: ActivatedRoute, we need to import it also

simpley, injects the currently active route

the route is a js object


```js
  constructor(
    private serversService: ServersService,
    private router: Router,
    private route: ActivatedRoute
  ) { }
```

```js
  onReload() {
    this.router.navigate(['servers'], {relativeTo: this.route})
  }
```

angular knows, what are currently active route is, relativeto this route you should navigate, 

now we broke the app again, because we are in servers/servers



# 122. passing parameters to routes

add a new route
we also want to load the user component

we want to load a user dynamically

pass the id of the user we want to load in this route path


```js
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'users/1', component: UserComponent },
  { path: 'users/2', component: UserComponent },
  { path: 'users/3', component: UserComponent },
  { path: 'servers', component: ServersComponent }
];
```

shitty

we can add parameters in our path, we add a colon, any name you like

later you can retrieve this parameter, the colon tells, this is a dynamic path

```js
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'users/:id', component: UserComponent },
  { path: 'servers', component: ServersComponent }
];
```


user.component.html

```html
<p>User with ID _ID_ loaded.</p>
<p>User name is _NAME_</p>
```

lets see if we reach this component-page

yeahhhh

how can we get access to this data inside our component

# 123. fetching route parameters

data encouded in the url, we want to get

user.component.ts

we need to inject sth that we injected before, the activatedroute

by injecting this, we get access to the currently activated route

there is a lot of metadata, we find the data there

```js
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent implements OnInit {
  user: {id: number, name: string};

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
  }

}
```

in ngonginit, when our comonent gets initilaized

```js
this.user = {
	id: this.route.snapshot.params['id'],
	name: ,
}
```

```js
{ path: 'users/:id/:name', component: UserComponent }
```

```js
this.user = {
	id: this.route.snapshot.params['id'],
	name: this.route.snapshot.params['name']
}
```


it works, still marked as active route, since users is contain in the path users/1/albert

# 124. fetching route parameters reactively

there are cases where this approach dont work

users.components.html

add a link, routerLink,, 

```html
<a [routerLink]="['/users', 10, 'Anna']">Load Anna (10)</a>
```

constructs a route which is

/users/10/Anna


this array approach its better, you can structure the path very well

we see the link, if i click, the url was updated, but the text not

this is not a bug, its default behaviour

we load our data, by using the snapobjects on the route

if we laod a new route, angular has to look our appmodule, finds the route there, finds the component, initialze thecomponent, gives us the data by accessing th snapshot

that only happens if we havent been in this component before

the url changes, we are already in the component. angular is clever, he doesnt want to reload a component where we are already one

the dta does not know, that the data changed

still, you want the updated data

snapshot is good for the first initialization, but to react to the subsequent changes, we need another approach


in ngongint

this.route.params

params here, is an observable.

basically, are a feature, added by other thirdparty packages, not by angular

allows you to work with asyncronous tasks

this is a asysncronous task, params may change , you dont know when, how long it will take etc.

therefore, you cant block your code, and wait for this to happen

an observable is an easy way to subscribe to an event that it might happen in the future, to then exectue some code when it happens, without having to wait for him now

this.route.params
	.subscribe(
	);

subscribe can have 3 functions as arguments

thefirst one

it will be fired, whenever new data is sent through the observable, whenever the params changed in this case

()=> {
	
}

we will get the updated params as an argument,

(params: Params)=> {
	this.user.id = params['id'],
}


user.component.ts

```js
  ngOnInit() {
    // first time, we create
    this.user = {
      id: this.route.snapshot.params['id'],
      name: this.route.snapshot.params['name']
    }
    // if params change, we dont create the c
    this.route.params
      .subscribe(
        (params: Params) => {
          this.user.id = params['id'],
          this.user.name = params['name']
        }
      );
  }
```

update our user object, whenever the params change
this code its not going to be called (inside subscribe)

on ngoninit, the subscription will be just set up

now the link works

observable fires

if you know, that your component will never be reload, from inside, you dont need this addition

simply the snapshot

# 125. An important note about route observables

this subscription works fine

angular does sth for you, in the background, which is important

it cleans up the subscription you build here, whenever the component is destroyed

subscription will live in the memory

theoreticylly

implements OnDestroy

ngOnDestroy() {
	
}


```js
import { Subscription } from 'rxjs/Subscription';
```

```js
paramsSubscription: Subscription;
```


```js
this.paramsSubscription = this.route.params
.subscribe(
  (params: Params) => {
    this.user.id = params['id'],
    this.user.name = params['name']
  }
);
```

```js
ngOnDestroy() {
  this.paramsSubscription.unsubscribe();
}
```

angular does that for you, but you need that, if you create your own observables

# 126. passing query parameters and fragments

we can add more things to our url

query parameteres, separated by a ?,multiple, with &

how to pass them, with routerlinks, how to retrieve them

the # in the url, jump to a specific place

pass extra information, routerLink derictive, navigate method in code
and how to retrieve

lets pass it first

more routes!

app.module.ts

```js
  { path: 'servers/:id/edit', component: EditServerComponent }
```

servers.component.html

the route i want to call:

```html
<a
  [routerLink]="'/servers', 5, 'edit'"
  href="#"
  class="list-group-item"
  *ngFor="let server of servers">
  {{ server.name }}
</a>
```

add query parameter, if we can edit the server or not

add the question mark, we dont do ?=

we add a new propertyof this routerlink directive
queyParams its just another bindable property of routerlink directive

key value pairs

[queryParams]="{allowEdit: '1'}"

```js
http://localhost:4200/servers/5/edit?allowEdit=1
```

you can also add fragments, not key value pairs

```js
[fragment]="'loading'"
```

or

```js
fragment="loading"
```

http://localhost:4200/servers/5/edit?allowEdit=1#loading

```html
<a
  [routerLink]="['/servers', 5, 'edit']"
  [queryParams]="{allowEdit: '1'}"
  [fragment]="'loading'"
  href="#"
  class="list-group-item"
  *ngFor="let server of servers">
  {{ server.name }}
</a>
```

lets do this programatically


home.component.html

```html
<button class="btn btn-primary" (click)="onLoadServer(1)">Load Server 1</button>
```

home.component.ts

```js
onLoadServer(id: number) {
  // complex calculation
  this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'})
}
```

http://localhost:4200/servers/1/edit?allowEdit=1#loading

and you go there, awesome

retrieve the data there after

# 127. retrieving query parameters and fragments

whee are loading edit-server.component.ts at the end

```js
import { Component, OnInit } from '@angular/core';

import { ServersService } from '../servers.service';

@Component({
  selector: 'app-edit-server',
  templateUrl: './edit-server.component.html',
  styleUrls: ['./edit-server.component.css']
})
export class EditServerComponent implements OnInit {
  server: {id: number, name: string, status: string};
  serverName = '';
  serverStatus = '';

  constructor(private serversService: ServersService) { }

  ngOnInit() {
    this.server = this.serversService.getServer(1);
    this.serverName = this.server.name;
    this.serverStatus = this.server.status;
  }

  onUpdateServer() {
    this.serversService.updateServer(this.server.id, {name: this.serverName, status: this.serverStatus});
  }

}
```


we are just calling the serversService and getting the server through the id, and updating it

how we can get access to the query paramter and the gfragment

inject ActivatedRoute, needs to be imported

with this added, in ngOnInit

we can retrieve them

two ways of retrieving it

1 - access the snapshot

```js
  constructor(
    private serversService: ServersService,
    private route: ActivatedRoute
  ) { }
```


```js
ngOnInit() {
  console.log(this.route.snapshot.queryParams);
  console.log(this.route.snapshot.fragment);
```

this is only run, at the time this component is created
if you change the content from another place, this is not reactive, you need the params observable

we see them, when hitting the url 

http://localhost:4200/servers/1/edit?allowEdit=1#loading



2- the alternative

```js
this.route.queryParams.subscribe();
this.route.fragment.subscribe();
```

but we dont need it

# 128. Practicing and some common gotchas


users.component.html

add a routerLink, pass an array as an argument, target /users, we have the reouter , users/:id/name

so we can use it here

['/users', user.id, user.name]

dynamically construct such a link

```html
<a
  [routerLink]="['/users', user.id, user.name]"
  href="#"
  class="list-group-item"
  *ngFor="let user of users">
  {{ user.name }}
</a>
```

same for the servers

servers.component.html


```html
  [routerLink]="['/servers', 5, 'edit']"
```

to

```html
  [routerLink]="['/servers', server.id]"
```

i want to load the single server component

app.module.ts

add fitting route

new route, 

```js
{ path: 'servers/:id', component: ServersComponent },
```

server.component.html

we want to output the data that we pass in the url



```js
  constructor(
    private serversService: ServersService,
    private route: ActivatedRoute
  ) { }
```

now in ngOnInit

```js
  ngOnInit() {
    const id = Number.parseInt(this.route.snapshot.params['id']);
    const id = Number.parseInt(this.route.snapshot.params['id']);
    this.server = this.serversService.getServer(id);

    // react to any changes after
    this.route.params
      .subscribe(
        (params: Params) => {
          this.server = +this.serversService.getServer(params['id']);
        }
      );
  }
```

```js
const id = Number.parseInt(this.route.snapshot.params['id']);
const id = +this.route.snapshot.params['id'];
```



we have the error, because in servers.component.html we have

```html
    <app-server></app-server>
```

we comment it out (or you initialize the component always with the first server)




we want now some child routes

when we click a server, the menu remains, and we see the server details

we dont want to abandond the servers page quasi

we wnat some nested routes, child routes, a route inside a route

# 129. setting up child (nested) routes

if we click for a user, for a server, we load a brand new page


it would be nicer, if we loaded next to the menu

app.module.ts

we have some duplication. thre routes start with servers

we want to nest them, servers, and then some childs

```js
  { path: 'servers', component: ServersComponent },
  { path: 'servers/:id', component: ServerComponent },
  { path: 'servers/:id/edit', component: EditServerComponent }
```

lets add child routes

servers, i add another property, children, which is arrays

```js
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'users/:id/:name', component: UserComponent },
  { path: 'servers', component: ServersComponent, children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] }
];
```

now we group routes together
on the servers, we have the esrverscomponent, 
where will be the serverComponennt be loaded?

error, cannot find primary outlet to load 'ServerComponent'



app.component.html

we have

```html
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <router-outlet></router-outlet>
    </div>
  </div>
```

this is where we show the components

that is reserved, for all the routes, on the top level, not for the child routes levels

servers.component.html

```html
  <div class="col-xs-12 col-sm-4">
    <router-outlet></router-outlet>
    <!-- <a class="btn btn-primary" (click)="onReload()">Reload Page</a>
    <app-edit-server></app-edit-server>
    <hr>

    <app-server></app-server> -->
  </div>
```


all the childs, will be loaded here


```js
{ path: 'servers', component: ServersComponent, children: [
  { path: ':id', component: ServerComponent },
  { path: ':id/edit', component: EditServerComponent }
] }
```

thats how we add child routes

thats why we  need to suscribe, now we change the names and ids without destroy the components server or name

# 130. Using Query Parameters - Practice

in the last lecture, we add child routing

in our servers.component.html, we have our links, loading our individual servers

with this allowEdit query parameter

```html
<div class="col-xs-12 col-sm-4">
  <div class="list-group">
    <a
      [routerLink]="['/servers', server.id]"
      [queryParams]="{allowEdit: '1'}"
      [fragment]="'loading'"
      href="#"
      class="list-group-item"
      *ngFor="let server of servers">
      {{ server.name }}
    </a>
  </div>
</div>
```

server.component.html

```html
<h5>{{ server.name }}</h5>
<p>Server status is {{ server.status }}</p>

<button class="btn btn-primary" (click)="onEdit()">Edit Server</button>
```

we can not reach the server-edit.component

we just have the route

:id/edit

server.component.ts

now i want to navigate to the server-edit component

we need to get access to the router, so we get access to the navigate method

we injected it

```js
  constructor(
    private serversService: ServersService,
    private route: ActivatedRoute,
    private router: Router
  ) { }
```

```js
  onEdit() {
    this.router.navigate(['servers', this.server.id, 'edit'])
  }
```

but we are already on the path

we simply want to append edit, to the end of the currently loaded route


```js
onEdit() {
  // this.router.navigate(['servers', this.server.id, 'edit'])
  this.router.navigate(['edit'], {relativeTo: this.route});
}
```

in the url we have

http://localhost:4200/servers/2?allowEdit=1#loading

when we click edit serve, we lose this info

http://localhost:4200/servers/2/edit

we need to fix this, before we can use the information in edit

we want to be able to edit the server, if the server.id equals 3, for example

servers.components.html

```html
<a
  [routerLink]="['/servers', server.id]"
  [queryParams]="{allowEdit: server.id === 3 ? '1' : '0'}"
  [fragment]="'loading'"
  href="#"
  class="list-group-item"
  *ngFor="let server of servers">
  {{ server.name }}
</a>
```

edit-server.component.ts

we want to be able to retrieve the query params

we alredy habe the query.Params subscribe

```js
allowEdit = false;
```

whenever this changes, in the queryPrams, i want to get my queryparams,  and then in the method body, i will set thisallowedit, 

```js
ngOnInit() {
  console.log(this.route.snapshot.queryParams);
  console.log(this.route.snapshot.fragment);
  this.route.queryParams.subscribe(
    (queryParams: Params) => {
      this.allowEdit = queryParams['allowEdit'] === '1' ? true : false;
    }
  );
```

edit-server.component.html

```html
<h4 *ngIf="!allowEdit">You are not allowed to edit</h4>

<div *ngIf="allowEdit">
  <div class="form-group">
    <label for="name">Server Name</label>
    <input
      type="text"

```

we are always in not allowwed because isAllow is always false, because we lose our query params

we need to fix that

how to preserver our query params once we navigate again

# 131. configuring the handling of query parameters

ourparams go away when we leave our single server component

we want to preserve them

server.component.ts

we can pass another property, to the js object, we use to config our navigation

the queryParamsHandling: 
takes a stirng as a value, that can be merge

but we dont add news, we use 'preserve'

make sure that the old ones are kept. if we add new ones, we use 'merge'


```html
  onEdit() {
    // this.router.navigate(['servers', this.server.id, 'edit'])
    this.router.navigate(['edit'], {relativeTo: this.route});
  }
```

we do

```js
  onEdit() {
    // this.router.navigate(['servers', this.server.id, 'edit'])
    this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'preserve'});
  }
```

cool

if we click a server

http://localhost:4200/servers/1?allowEdit=0#loading

if we click edit server

http://localhost:4200/servers/1/edit?allowEdit=0



# 132. redirecting and wildcard routes

what happens if we enter localhost4200/somoething

we get an error

some 404 error handling, redirect the user when he tries to go to a page that we dont have

redirecting request

lets start with redirecting

we add a new component

```js
ng g c page-not-found
```


page-not-found.component.html

```html
<h3>this page was not found</h3>

```

i want to visit this page if the route is not found

app.modules.ts

```js
  { path: 'something', component: PageNotFoundComponent}
```

we see this page was not found

component: ... loads the component

redirectTo redirects to the path

```js
  { path: 'not-found', component: PageNotFoundComponent },
  { path: 'something', redirectTo: '/not-found'}
```

if we type someting in the url, we are redirected to not-found

more convenient way to catch all routes not covered by the appRoutes constant

use the **

wild card route, catch all paths you dont know. the order is super important, this very generic route, must be the last one

```js
  { path: 'not-found', component: PageNotFoundComponent },
  { path: '**', redirectTo: 'not-found'}
```

wild care route is the one with the path **

cool

# 135. Important: Redirection Path Matching

In our example, we didn't encounter any issues when we tried to redirect the user. But that's not always the case when adding redirections.

By default, Angular matches paths by prefix. That means, that the following route will match both /recipes  and just / 

{ path: '', redirectTo: '/somewhere-else' } 

Actually, Angular will give you an error here, because that's a common gotcha: This route will now ALWAYS redirect you! Why?

Since the default matching strategy is "prefix" , Angular checks if the path you entered in the URL does start with the path specified in the route. Of course every path starts with ''  (Important: That's no whitespace, it's simply "nothing").

To fix this behavior, you need to change the matching strategy to "full" :

```js
{ path: '', redirectTo: '/somewhere-else', pathMatch: 'full' }
```

Now, you only get redirected, if the full path is ''  (so only if you got NO other content in your path in this example).


# 134. Outsourcing the Route Configuration

app.module.ts

the routing, takes some significant space

if you have a lot of routes, you dont add them in the app module

you add them in a new file

app-routing.module.ts

```js
import { NgModule } from "../../node_modules/@angular/core";

@NgModule({
	
})

export class AppRoutingModule {

}
```

this module will handle all the routes

```js
import { NgModule } from "../../node_modules/@angular/core";
import { Routes } from "../../node_modules/@angular/router";
import { HomeComponent } from "./home/home.component";
import { UsersComponent } from "./users/users.component";
import { UserComponent } from "./users/user/user.component";
import { ServersComponent } from "./servers/servers.component";
import { ServerComponent } from "./servers/server/server.component";
import { EditServerComponent } from "./servers/edit-server/edit-server.component";
import { PageNotFoundComponent } from "./page-not-found/page-not-found.component";

const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent },
  ] },
  { path: 'servers', component: ServersComponent, children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] },
  { path: 'not-found', component: PageNotFoundComponent },
  { path: '**', redirectTo: 'not-found'}
];

@NgModule({

})

export class AppRoutingModule {
	
}
```

no declarations in ngmodule, because they are declared in the app.module.ts

we also need to remove the router module

```js
RouterModule.forRoot(appRoutes)
```

in app.module.ts

in app-routing.module.ts

make sure to import

```js
import { Routes, RouterModule } from "../../node_modules/@angular/router";
```

we need to config sth in ng module

we want to add imports again,

imports: [
	RouterModule.forRoot(appRoutes)
]

this is not enough

we need to add our app routing module back to our main module

we need to add the exports array
tells angular, from this module, if i were to ask something from this module in the imports of another module, what should be accessible

the one thing we want to make accessible is the routermodule

```js
@NgModule({
	imports: [
		RouterModule.forRoot(appRoutes)
	],
	exports: [RouterModule]
})
```

in app.module.ts, now we can import our own app-routing module

in imports array

```js
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule
  ],
```

now we have the same setup as before

but with a cleaner code

# 135. an introduction to guards

route guards: functionality, logic, code, executed before a route is loaded

you only want to give access to your server component or the edit server component, if the user is log in

we will fake the log in

still, we want to check the log in for all the sub routes

manually chcecking this, in the ngoninit, of the compoennts server and edit-server would be too much

we will use a feature from angular, which is run before the component is loaded

well use the canactivate guard

# 136. Protecting Routes with canActivate

run some code, at a point of time defined by us

auth-guard.service.ts

it guards certain actions, navigation to a route etc.

its a normal service

we use the CanActivate interface

```js
import { CanActivate } from '@angular/router';

export class AuthGuard implements CanActivate {

}
```

we could name it AuthGuardService

it forcse you, to have a canActivate method in this class
recieves two arguments

```js
import { CanActivate, RouterStateSnapshot, ActivatedRouteSnapshot } from '@angular/router';

export class AuthGuard implements CanActivate {
	canActivate(
		route: ActivatedRouteSnapshot,
		state: RouterStateSnapshot
	)  
}
```

where are we getting this arguments from?

angular executes this code before a route is loaded, so it will give us this data

we need to be able to handle the data

canactivate also returns something. either an observable. 


```js
import { CanActivate, RouterStateSnapshot, ActivatedRouteSnapshot } from '@angular/router';

import { Observable } from 'rxjs/Observable'

export class AuthGuard implements CanActivate {
	canActivate(
		route: ActivatedRouteSnapshot,
		state: RouterStateSnapshot
	): Observable {

	}
}
```

the observable wraps a boolean. at the end, resolves to a true or false
alternatively, it can return a promise, or a boolean

canActivate, asynchronously, returning a promise or a observable, or synchronously, then returns a boolean

```js
export class AuthGuard implements CanActivate {
	canActivate(
		route: ActivatedRouteSnapshot,
		state: RouterStateSnapshot
	): Observable<boolean> | Promise<boolean> | boolean {

	}
}
```

we want to be able to log in or out

lets say we have another service. auth.service.ts

just a fake service


auth.service.ts

```js
export class AuthService {
	loggedIn = false;

	isAuthenticated() {
		
	}
	
	login() {
		this.loggedIn = true;
	}

	logout() {
		this.loggedIn = false;
	}
}
```

isAuthenticated

check the state, i want to simulate that this takes some time

i will return a promise, a new promise. this promise take a method, as an argument
resolve and reject methods we can execute

and in this promise, i will execute settime out to wait

and then execute another method, where i will resolve the promise

return this.LoggedIn

just to fake that we call a server

```js
	isAuthenticated() {
		const promise = new Promise(
			(resolve, reject) => {
				setTimeout(() => {
					resolve(this.loggedIn);
				}, 800);
			}
		);
		return promise;
	}
```

auth-guard.service.ts

we want to use it in my auth-guard, auth is a service

we need to add @Injectable

i will add a constructor to my authguard to inject the service

```js
import { AuthService } from "./auth.service"

@Injectable()
export class AuthGuard implements CanActivate {

	constructor(private authService: AuthService) {}

	canActivate(
		route: ActivatedRouteSnapshot,
		state: RouterStateSnapshot
	): Observable<boolean> | Promise<boolean> | boolean {

	}
}
```

in the canActivate guard, i just wnat to check if the user is logged in or not

remember, isAuthenticated returns a promise

so i use then

i will get back a boolean, this loggedIn boolean we resolved

i want to check if is true, i will return true, otherwise, i want to navigate away.to force the uesr to go somewhere else

so we need to inject the router

now, we can navigate with our navigate method



```js
canActivate(
	route: ActivatedRouteSnapshot,
	state: RouterStateSnapshot
): Observable<boolean> | Promise<boolean> | boolean {
	this.authService.isAuthenticated()
		.then(
			(authenticated: boolean) => {
				if (authenticated) {
					return true;
				} else {
					this.router.navigate(['/'])
				}
			}
		);
}
```

i will return this promise, if we return something inside the promise it will give us back another promise

we add return infront of this.authService.isAuthenticated





we are still not using this guard

app-routing.module.ts

which route should be protected by this guard

in path: 'servers' route, and all its child routes

adding, canActivate, takes an array, of all the code, all the guards, you want to apply to this route, and automatically will be apply to all the childs

canActivate: [AuthGuard] will use my AuthGuard

servers, and all its child routes, is only accessible if the authguard, canactivate, returns true at the end, which only happens, when the authservice, is set to true

now, is always false




```js
import { AuthGuard } from './auth-guard.service';

const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent },
  ] },
  { path: 'servers', canActivate: [AuthGuard], component: ServersComponent, children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] },
  { path: 'not-found', component: PageNotFoundComponent },
  { path: '**', redirectTo: 'not-found'}
];
```

we added two new services, we need to inform angular in the app.module.ts

app.module.ts




```js
  providers: [ServersService, AuthService, AuthGuard],
```



otherwise angular will not be able to inject it for us


our guard is working!

if i click servers, i go to home, after 800ms

cool.

but i want to see all the servers, and protect just the childs

# 137. protecting child (nested) routes with canActivateChild

the guard was working for our whole serveres

we could grab it, and add it to the childs. but if we have a lot of childs, its bad

there is another guard we can use



CanActivateChild, its an interface, lets implement it to


```js
export class AuthGuard implements CanActivate, CanActivateChild {
```

this interface requires a method canActivateChild()

it has the route and state, and it returns an observable promise or boolan

```js
canActivateChild(
	route: ActivatedRouteSnapshot,
	state: RouterStateSnapshot
): Observable<boolean> | Promise<boolean> | boolean {
	
}
```

since it uses the same logic, we can

```js
canActivateChild(
	route: ActivatedRouteSnapshot,
	state: RouterStateSnapshot
): Observable<boolean> | Promise<boolean> | boolean {
	return this.canActivate(route, state);
}
```

we can use a different hook in our routes


```js
{ path: 'servers', 
    // canActivate: [AuthGuard], 
    canActivateChild: [AuthGuard],
    component: ServersComponent, 
    children: [
      { path: ':id', component: ServerComponent },
      { path: ':id/edit', component: EditServerComponent }
    ] 
  },
```

AuthGuard can protect the component and the child, since we imlement it that way

but we just use it to the childs, because we are calling canActivateChild, the canActivate is comment out


now just the chlds are protected



# 138. Using a Fake Auth Service

canActivate behavior, by allowing the user to log in

home.component.ts

two buttons, log in, log out

```html
<button class="btn btn-default" (click)="onLogin()">Login</button>
<button class="btn btn-default" (click)="onLogout()">Log out</button>
```

home.component.ts

```js
  constructor(
    private router:Router,
    private authService: AuthService
  ) { }
```

home.component.ts

```js
  onLogin() {
    this.authService.login();
  }

  onLogout() {
    this.authService.logout();
  }
```

# 139. Controllling Navigation with canDeactivate

if you are allow to leave a route or not

if you are edit sth, ask the user, do you really want to leave???

edit-server.component.ts

new property, 

```js
changesSaved = false;
```

i want to change, whenever i click onUpdateServer

```js
  onUpdateServer() {
    this.serversService.updateServer(this.server.id, {name: this.serverName, status: this.serverStatus});
    this.changesSaved = true;
  }
```

after the changes are saved, i want to navigate away

we inject the router

```js
  constructor(
    private serversService: ServersService,
    private route: ActivatedRoute,
    private router: Router
  ) { }
```

navigate up one level, to the last loaded server

i will pass the relativeto configuration, to the relative currently loaded route

```js
  onUpdateServer() {
    this.serversService.updateServer(this.server.id, {name: this.serverName, status: this.serverStatus});
    this.changesSaved = true;
    this.router.navigate(['../'], { relativeTo: this.route })
  }
```

now lets prevent to navigate away

at least, ask if he really wants to live

we need to execute the code in this component, because we need access to the changesSaved component

which informs us, whether the server update bttn was clicked it or not

however, a guard, always needs to be a service, something does not fit. we need the code in our component, but it needs to be also in a service

solution

in my edit-server folder

new file: can-deactivate-guard.service.ts

first of all, i want to export an interface

its a contract, which can be imported by some other class, which forces this calss to provide some logic

this interface will require one thing from the component, which implements it

this component should have a canDeactivate method

this method, here will not contain the actual logic, 

()=> this is how i define the type of this method, since the logic will be not here. just the information how it will look like

this method should take no arguments, but it shoudl return an observable, which will resolve to a boolean in the end, or  apromise, which will resolve to a boolean in the end, or a bolean

```js
import { Observable } from 'rxjs/Observable'

export interface CanComponentDeactivate {
	canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean; 
}
```

that is my interface

nice to have it, we need the meat of the class, the service itself

i will name it CanDeactivateGuard, this guard, will implement CanDeactivate, this is an interface provided by the angular/router

this is a generic type it will wrap our own interface, it will wrap an interface, which forces some component, or some class, to implement the canDeactivate method

this is the setup which will make sure, that we later can easily connect a component to OUR canDeacivategUARD HERE

this class, this guard

```js
export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
	
}
```

this class needs also to have a canDeactivate method, this is the canDeactivate method, which will be called by the angular router, once we try to leave a route

therefore, this wil have the component on which we are currently on as an argument, and this component needs to be of type CanComponentDeactivate 



which means, it needs to be a component which has this interface here implemented, therefore a component which has the canDeactivate method
we also recieve currentRout as an argument, it will have the currentState, and the next enxtState: where do you want to go. rememeber, we want to leave a route

this will also return an observable, a promise or a boolean

```js
import { Observable } from 'rxjs/Observable'
import { CanDeactivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '../../../../node_modules/@angular/router';

export interface CanComponentDeactivate {
	canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean; 
}

export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
	canDeactivate(
		component: CanComponentDeactivate,
		currentRoute: ActivatedRouteSnapshot,
		currentState: RouterStateSnapshot,
		nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
			
		} 
}
```

now i want to call canDeactivate on the component we are currently on.

this is, why i need to implement this interface in this component, why i created this interface, now the angular router can execute canDeactivate in our service and can rely on the fact that the component we are currntly on, has the candeactive method too

cuz this is where we will implement the logic checking weather we are alowed to leave or not

becase we need this conection between our guard and  component

thi sis why we can savely call canDeactivate here



```js
import { Observable } from 'rxjs/Observable'
import { CanDeactivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '../../../../node_modules/@angular/router';

export interface CanComponentDeactivate {
	canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean; 
}

export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
	canDeactivate(
		component: CanComponentDeactivate,
		currentRoute: ActivatedRouteSnapshot,
		currentState: RouterStateSnapshot,
		nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
			return component.canDeactivate();
		} 
}
```

app-routing.modul.ts

```js
      { path: ':id/edit', component: EditServerComponent }

```



on the edit page, we want to add this guard, so we can add canDeactivate as a property to this route config, itt akes an array, now we will point to our canDeactivateGuard

now angular will run this guard, whenever we try to leave this path


```js
  { path: ':id/edit', component: EditServerComponent, canDeactivate: [CanDeactivateGuard] }
```

for this to work, we need to go to our module, and provide our CnaDeactivateGuard

one important piece is missing

remember that i told you that, the canDeactivate guard will in the end call the canDeactivaate in our component, 

for this to work in our edit-server component, we need to implement CanComponentDeactivate interface
our own interface, we exported in our candeactivate guard service file

this interface forces us to implment the candeactivate method in our component, its important cuz we try to call this method on our component here

in the edit server component

add a canDeactivate method, we know how this method should look like, we defined it before

```js
  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    
  }
```

here, we now provide the actual logic, deciding wheter we are allowd to leave or not

this logic will be run, whenever the canDeactivagte guard is checked by the angular router

if we are not allwoed, we return true, sure you may leave
otherwise, i will check, if my serverName is unlike the serve name we had at the beginning

if we have differences name or on the server status, if one of the two was changed, and the changes were not changed. in this case, i want to return a confirm dialog, do you want to discard the changes?

edit-server.component.ts

```js
  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    if (!this.allowEdit) {
      return true;
    }
    if ((this.serverName !== this.server.name || this.serverStatus !== this.server.status) && !this.changesSaved) {
      return confirm('Do you want to discard the changes?')
    } else {
      return true;
    }
  }
```

!!! buf, reread and reread, very difficult

```js
  ngOnInit() {
    console.log(this.route.snapshot.queryParams);
    console.log(this.route.snapshot.fragment);
    this.route.queryParams.subscribe(
      (queryParams: Params) => {
        this.allowEdit = queryParams['allowEdit'] === '1' ? true : false;
      }
    );
    this.route.fragment.subscribe();
    this.server = this.serversService.getServer(1);
    this.serverName = this.server.name;
    this.serverStatus = this.server.status;
  }
```

we are still just updating the id 1

```js
    const id = +this.route.snapshot.params['id'];
    this.server = this.serversService.getServer(id);
    // subscribe route params to update the id if params change
```

still one guard to learn

# 140. Passing Data to a route

we had a look at two guards, canactivate and can deactivate


how to get some data, static data, or dynamic data, once our route is loaded

not from the url

some of the routes may depen on some data

lets start with static data

create a new component, the error page component

```js
ng g c error-page
```

generic error page


we could image that we use the error page for all kind of errors

i dont want to show , page not found or sth like this, hardcoed

i want to show

error-page.component.html

```js
<h4>{{ errorMessage }}</h4>
```

```js
export class ErrorPageComponent implements OnInit {
  errorMessage: string;
```

the route is not found, would be the usecase

app-routing.module.ts

```js
{ path: 'not-found', component: PageNotFoundComponent },
{ path: '**', redirectTo: 'not-found'}
```
to

```js
{ path: 'not-found', component: ErrorPageComponent },
{ path: '**', redirectTo: 'not-found'}
```

with this, we are now loading this error page, if we target the not found route

the issue is, we try to output a message we havent got in the component


now, we could reach this page, through different errors. for each error, there is just one message

but in this example, the error is always not found, so we can pass this as a static data

data property, we can pass an object, we can define any key value pairs, any properties we want, 

we dont write this message in the error page, because from another route, we may have another message
so we can reuse the page

```js
  { path: 'not-found', component: ErrorPageComponent, data: {message: 'Page not found!'} },
  { path: '**', redirectTo: 'not-found'}
```

with this, we want to retrieve this whenever we load our errorpage component

we need the route: ActivatedRoute

in ngOnInit
snapshot.data['message']

or we could use with a subscribe

this.route.data.subscribe(
	(data: Data) => {
		this.errorMessage = data['message'];
	}
)


```js
export class ErrorPageComponent implements OnInit {
  errorMessage: string;

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.errorMessage = this.route.snapshot.data['message'];
    this.route.data.subscribe(
      (data: Data) => {
        this.errorMessage = data['message'];
      }
    )
  }

}
```


we see the static message we pass with the data property

# 141. Resolving Dynamic Data with the resolve Guard

in the last lecture we learnt how to pass static data

now, dynamic data, we have to fetch before a route is displayed or rendered

once we click a esrver, i want to load the server from some backend

we need a resolver, its a service, just like canactivate and candeactivate

it allows us run some code, before a route is render

the differene is, the resolver will not decide if the route is renderer or not, the resolver will always render teh component, but will fetch some data the component will need

alternative, in the ngoninit fetch the data, and while, show a spinner

but if you want to load it before displaying the route, we should do the resolver

file: server-resolver.service.ts

has to implement the Resolve interface, provided by @angular/router, is a generic type, it should wrap, whichever item you will get here, you fetch here in the end

we will fetch a server, therefore, we should define this. we could outsurce this in a interface or a model

this is a type definition of what this resolver will give us in the end, to what it will resolve in the end

```js
import { Resolve } from '@angular/router';

export class ServerResolver implements Resolve<{ id: number, name: string, status: string }> {

}
```

the resolve interface requires us to implement the resolve method, this resolve method takes two arguments, the route, the activatedroutesnapshot, and will also provide us the state, routerstatesnapshot

these information pieces, the resolve method gets by angular

```js
export class ServerResolver implements Resolve<{ id: number, name: string, status: string }> {
  resolve (route: ActivatedRouteSnapshot, state: RouterStateSnapshot)
}
```

and this needs to retun
either an observable, this observable returns this type, but we can quickly define an interface
we could outsurce this in a file.

interface server

```js
import { Resolve } from '@angular/router';
import { Observable } from 'rxjs/Observable';

interface Server {
	id: number;
	name: string;
	status: string;
}

export class ServerResolver implements Resolve<Server> {
  resolve (route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Server> 
}
```
so we have a shortcut for this type

the observable should give us in the end such a Server, or we get a promise, which return such a server, or just such a server, synchronously

```js
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs/Observable';

interface Server {
	id: number;
	name: string;
	status: string;
}

export class ServerResolver implements Resolve<Server> {
  resolve (route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Server>   | Promise<Server> | Server {
		
	}
}
```

this is what we will get back from the resolve

now we implement logic to get this back

we have our servers.service, there we have the getServer method

this is syhncronous code, we will resolve instantly


```js
  getServer(id: number) {
    const server = this.servers.find(
      (s) => {
        return s.id === id;
      }
    );
    return server;
  }
```

reach out to our service.servies, we should inject it in the constructor

if you want to insert a service into another service, in the service, add @injectable

the easiest way to use the resolve function to reach out to your service, and there we call getServer, we need to know the id of the server we have to fetch

thats what we do in the server component in the end, in the ngOnInit method we are getting the server

```js
  ngOnInit() {
    const id = +this.route.snapshot.params['id'];
    this.server = this.serversService.getServer(id);

    // react to any changes after
    this.route.params
      .subscribe(
        (params: Params) => {
          this.server = this.serversService.getServer(+params['id']);
        }
      );
  }
```

but now we want to outsource this, because we want to get the server before the component loads

the good thing is, we get the route, it only is the snapshot, but this service here, will run each time we rerender the route.
the snapshot is all we need, unlike the component itsel, this is executed each time, no need to execute an observable

we can acces our route.params['id']

resolver will do the loding of our data in advance

this would work if this returns an observable or a promise, for eample, an http request

```js
@Injectable()
export class ServerResolver implements Resolve<Server> {
	constructor(private serversService: ServersService) {}

  resolve (route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Server>   | Promise<Server> | Server {
		return this.serversService.getServer(+route.params['id'])
	}
}
```

with this resolver in place, we have to add it

in app.moudle.ts we should add it to our proviers, its a service!

```js
  providers: [ServersService, AuthService, AuthGuard, CanDeactivateGuard, ServerResolver],
```

we want to add it to our routing module

in the ServerComponent, we add another property, the resolve property, takes a js object, we map all the resolvers.

this is different from the other guards, there we use arrays, but for resolve, a different approach is taken

key value pairs of the resolver we want to use, name of the property is up to you, we will use the ServerResolver


app-routing.module.ts

```js
  { path: 'servers', 
    // canActivate: [AuthGuard], 
    canActivateChild: [AuthGuard],
    component: ServersComponent, 
    children: [
      { path: ':id', component: ServerComponent, resolve: {server: ServerResolver} },
      { path: ':id/edit', component: EditServerComponent, canDeactivate: [CanDeactivateGuard] }
    ] 
  },
```

this will map the data, this resolver gives us back, and remember, it gives us back some data, we implemented the resolve method

this method will be called by angular when the route is loaded

the data it gives us back here

will be stored in the server object, or server property, we will have available to beloaded compoment, 


in our server comonent, we have the ngOnInit. we comment it out

now we use a resolver for this.

here, we can easily get our server, by binding the data observable, just like static data, which we passed through the data object

the data return by your resolver, it will also go to this data package, object we have, in our to be loaded compoment

we cna listen to any changes, we use an observable, because the server can page while we are not the page, because we have the servers menu

we get the data from data type, we can simply assign the sever property of this component by bindng to data['server']. this name is the same you use in app-routing.module, when we assign our resolver to some property

```js
  ngOnInit() {
    this.route.data
      .subscribe(
        (data: Data) => {
          this.server = data['server'];
        }
      );

    // const id = +this.route.snapshot.params['id'];
    // this.server = this.serversService.getServer(id);

    // // react to any changes after
    // this.route.params
    //   .subscribe(
    //     (params: Params) => {
    //       this.server = this.serversService.getServer(+params['id']);
    //     }
    //   );
  }
```


if we log in, and go to servers, and we click a server, it stills load the server, but now, not using the server in the params component but instead using a resolver 


# 142. Understanding Location Strategies

we have a couple of routes, it works fine, in local setup, but actually, this is not sth that you should take for granted

localhost:4200/servers, in a hosting, in internet. maybe it dont work

routes, the url, is always parsed, handled, by the server first, by the server which hosts your application

in local envirionment, we use a development serve. it has a special configuration your real life application it also have to has

the server hosting your angular single page application, such to be configured, in a case that we have a 404 it has to return the index.html

all the urls are pased by the server first, not by angular

if you have /servers, it will look for this route, in the real server, hosting the app

chances are, you dont have the route, because you just have one file, index.html, containging your angular app

you want angular to take over, and to parse this route. it will never get a change, if the server decides, no, i dont know the route, here is the 404 not found

you have to make sure, that your web server, returns the index.html file

if for some reason, you can not make this to work

you have an alternative approach

we can suse a hash sign in our route

app-routing.module.ts
w

where we register the routes

forRoot(appRoutes)

we can pass a second argument, a js object, to ths method, configuring the setup of the routes.

one iportant confi useHash: true, the default is false

if we do this

```js
@NgModule({
	imports: [
		RouterModule.forRoot(appRoutes, {useHash: true})
	],
	exports: [RouterModule]
})
```

we have now a hashtag in the url

http://localhost:4200/#/servers



this is the hash mode routing. this hash, informs our web server, only care, about the part of the url, before the hash tag

the part after, will be ingore by the server, therefore, this will run, even in servers which dont return html file in case of 404 errors

the part after the hash, will be parsed by the client, by angular

html history mode, its better. with useHash: false

# 143. Wrap up










































































