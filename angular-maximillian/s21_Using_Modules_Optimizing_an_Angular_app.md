# Section 21: Using Modules & Optimizing an Angular App

# 268. Module introduction

one thing we used a lot, but we never took a closer look at it

the app.module, what modules are?

here is about modules, about how you may your angular project

modules play an important role

how increase the performance of the app, the file size

and reestructure your code, in a better way, in an easier way to mantain

# 269. The idea behind Modules

The idea behind app modules

our app has a couple of components, directives, pipes

we have this app.module, that egnlvoes everything

all this elements, need to be in a module, or in multiple modules

the idea behind is, you clearly have to tell angular, what your app consists of

which services do you use, which components

angular does not scan all your files automatically

with modules, you know what you need, and also what yyou dont need

this allows you in production to strip out the things you dont need

thats the idea behind modules

right now we just use one module.
thats not good or bad, but we can do it better

as a devloper, we can make it easier to mantain

when we push to production, it can be more perfomance

lets have a look at the app.module.ts

# 270. Understanding the app module

the app.module.ts

is pretty big. it has a lot of import statements

we have a lot of types we use

a import here, is ts feature. ts needs to know where a specific thing, a class, where it leaves
import is not related to angular modules in any way

webpack will go through this inputs

and bundle our app in one file bundle at the end

angular modules, are sth totally different

they define how our angular app looks like to angular

this imports are a language feature, are required for a techncal perspective

then, we have alot of declaration and provideres

that defines our whole recipe application

its not bad, but we could use multiple modules

we have the declaration arays, imports, providers, bootstrap array

declarations: we define which components, directives or pipes, our app, this module, uses

inimports, we define: which other modules does this module use

we import other modules 

when we import an other module, we import everything this module export for us

in the example of a--routing.module.ts you can have a llook

we export a router module, the only reason to use this module, so that we dont need to put this code in the app.modoule.ts

because we exported the module, we have access to everything that is inside

thats the same for the built in modules, the forms module, it defines a bunch of directives, ngmodul, ngmodul group, by importing, we get access to all these directives

it saves the time, having to import all of these directives on our own

modules bundles certain functionalities and we import them

provideres: that simply defines which services we may use

mostly, that means, in our app. it the app.module, it means that
everything that we provide here, will be available for the whole application

with dependency injection, we use one of the same instance of this each of the service, in the whole app

bootstrap array, that defines our root component

this is diffrnt than the root module

the root component, is our main component, our main template, where we register all the components we use, like the app-header, router-outlet

root component, the starting point of what we see, of our business logic

app.module is like. ameta thing, it wraps the features we use



which other modules we may build, and use

# 271. Understanding Feature Modules

which additional modules we may build, and why

typical module you may add to you rapplication, would be a feature module

appModule, we have an appcomponent, component, directive, and then more components and directive

some of these features, like the app component, component and directive

are only used in the app.component, only in the root of our application

but then, wwe may have components and directive

which really only make up a feature of our app

in the recipe book, all recipe related components 

this is a goood case, to put all this components in a feature module

and everything it belogns to it

then we just import this module into the appmodule

technically, the app works like always, we dont win perfomance, but our file structure is better

easier to mantain

feature module, if you wnt to change sth from the feature, you just need to go to this feature

quickly see, which dependencies we have etc.

this is a great enhancment

lets build a couple of feature modules!

our recipe book, has a cuple of features

# 272. Creating a Recipe Feature Module

in the recipes folder, we can see a lot of files that could be in the recipe feature

in the declarations array, we see alot of these components

```js
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    RecipesComponent,
    RecipeListComponent,
    RecipeDetailComponent,
    RecipeItemComponent,
    ShoppingListComponent,
    ShoppingEditComponent,
    DropdownDirective,
    RecipeStartComponent,
    RecipeEditComponent,
    SignupComponent,
    SigninComponent
  ],
```

components that start with recipes

they belong to our recipe feature

you may also say, the recipeservice also belongs to the recipe feature

later we explain it

we can go to the recipe folder

and build our first module

the recipes.module.ts

to make this a module, we need to add a decorator

@NgModule, from angular core

```js
import { NgModule } from '@angular/core';

@NgModule()
export class RecipesModule {}
```

we transform this ts class into sth angular will recognize

into a module

in this ngmodule, we can define the same propeties as in the app.module. declarations, imports, 

which components are only used by the recipes module

we need to import the components. ts needs that

```js
import { NgModule } from '@angular/core';
import { RecipeListComponent } from './recipe-list/recipe-list.component';
import { RecipeEditComponent } from './recipe-edit/recipe-edit.component';
import { RecipeDetailComponent } from './recipe-detail/recipe-detail.component';
import { RecipeStartComponent } from './recipe-start/recipe-start.component';
import { RecipesComponent } from './recipes.component';
import { RecipeItemComponent } from './recipe-list/recipe-item/recipe-item.component';

@NgModule({
	declarations: [
		RecipesComponent,
		RecipeStartComponent,
		RecipeListComponent,
		RecipeEditComponent,
		RecipeDetailComponent,
		RecipeItemComponent
	]
})
export class RecipesModule {}
```

in the app.module.ts, remove them

```js
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ShoppingListComponent,
    ShoppingEditComponent,
    DropdownDirective,
    SignupComponent,
    SigninComponent
  ],
```

what about the RecipeService, in the providers arrays. in the app.module.ts

we should leave it here

cuz the recipeservice, is not only used by the recipes feature area

we use it from the header, when we store data

in the data-storage.service, we also use it

thats, why we shouldnt move it

we need to provide it here in our whole application

what else can we move to the recipes module

the recipes forms module, the reactiveForms module

we clearly not use it in the app module, we just use it in the recipes

app.module.ts

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  FormsModule,
  // ReactiveFormsModule,
  HttpModule
],
```

we want to copy one thing: the dropdown directive

because we use this dropdowndirective in the recipes component too, in the recipe detial copmonent

therefore, in the recipes module

imports array, is sth recognized by angular, which finds which other modules we want to use in this module

```js
@NgModule({
	declarations: [
		RecipesComponent,
		RecipeStartComponent,
		RecipeListComponent,
		RecipeEditComponent,
		RecipeDetailComponent,
		RecipeItemComponent,
		DropdownDirective
	],
	imports: [
		ReactiveFormsModule
	]
})
```

in every feature module you need the CommonModule

not to every, but, what the commonmodules, it gives you access to these directives: ngClass, ngFor

```js
imports: [
	CommonModule,
	ReactiveFormsModule
]
```

our recipe module, is taking shape

in app.module.ts

we dont have a commonmodule, we jsut have a browser module

the reason, the browser module contains all the feature of the common module and some additional features only needed when the application starts

therefore are only needed in the app.module.ts

therefore, we just use browser module in app.module.ts, and common module in the others

that'S our recipe module

now we can import our recipes module in to our

app.module.ts

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  FormsModule,
  HttpModule,
  RecipesModule
],
```

```js
import { RecipesModule } from './recipes/recipes.module';
```

we have an error message 

```js
Uncaught Error: Type DropdownDirective is part of the declarations of 2 modules: RecipesModule and AppModule! Please consider moving DropdownDirective to a higher module that imports RecipesModule and AppModule. You can also create a new NgModule that exports and includes DropdownDirective then import that NgModule in RecipesModule and AppModule.
```

thats what we did, that is not allowed. you can not declare componets, directives or pipes, in more than one module

you must not duplicate your declarations

so, from recipes.module.ts

wehave to remove 

```js
DropdownDirective
```

```js
@NgModule({
	declarations: [
		RecipesComponent,
		RecipeStartComponent,
		RecipeListComponent,
		RecipeEditComponent,
		RecipeDetailComponent,
		RecipeItemComponent,
		// DropdownDirective
	],
```

it shoul dstill work

but we still have an error, regarding the routing

in our recipes component

```html
<div class="row">
  <div class="col-md-5">
    <app-recipe-list
      
    ></app-recipe-list>
  </div>
  <div class="col-md-7">
    <router-outlet></router-outlet>
  </div>
</div>
```

we also have a router outlet

we define our routes in the app module

we import the approutingmodule, that is not enough

that doesnt travel down to our recipes module

a module, is only able to use, what we define inside the module (services are kind of an exception)

how can we put the routing, regarding the routes related to recipes, in to the recipes module


# 273. Registering Routes in a Feature Module

we have a problem with the routing

the router module, is only registered in our route module

we have to do, if we create a feature module, we also have to move our routes into the feature module

new routing module, in the recipes folder

recipes-routing.module.ts

```js
import { NgModule } from "../../../node_modules/@angular/core";

const recipesRoutes: Routes = [
    
]

@NgModule({})
export class RecipesRoutingModule {}
```

move here, the routes form recipes

```js
import { NgModule } from "../../../node_modules/@angular/core";
import { RecipesComponent } from "./recipes.component";
import { RecipeStartComponent } from "./recipe-start/recipe-start.component";
import { RecipeEditComponent } from "./recipe-edit/recipe-edit.component";
import { RecipeDetailComponent } from "./recipe-detail/recipe-detail.component";
import { Routes } from "../../../node_modules/@angular/router";
import { AuthGuard } from "../auth/auth-guard.service";

const recipesRoutes: Routes = [
  { path: 'recipes', component: RecipesComponent, children: [
		{ path: '', component: RecipeStartComponent },
		{ path: 'new', component: RecipeEditComponent, canActivate: [AuthGuard] },
		{ path: ':id', component: RecipeDetailComponent },
		{ path: ':id/edit', component: RecipeEditComponent, canActivate: [AuthGuard] }
	] },
]

@NgModule({})
export class RecipesRoutingModule {}
```


recipes routing module, 

we need to import the routermodule

we dont call forRoot, we call forChild!!!!!!!!!

in your app, you must only call forRoot, in your app.module.ts

in your root module

if you ever register routes in other places, always forChild

we are not on the root router, we are on a child module

in the end, everything will be imported to the appmdoule

```js
@NgModule({
	imports: [
		RouterModule.forChild(recipesRoutes)
	]
})
```

we need to export our RouterModule, which has now our recipe routes registered

```js
@NgModule({
	imports: [
		RouterModule.forChild(recipesRoutes)
	],
	exports: [RouterModule]
})
```

recipes.module.ts

```js
imports: [
	CommonModule,
	ReactiveFormsModule,
	RecipesRoutingModule
]
```

with this, the routes registered in this feature module, which we import in the app.module

so now they are available in our whole applciation


that is looking alright, but the dropdown is not working

we dong get an error in the console, but its not working

we removed the dropdown directive from the recipes.modules.ts, the declarations array

angular does not recognize it in the recipe-detail.component.html


```js
<div class="row">
  <div class="col-xs-12">
    <div class="btn-group" appDropdown >
      <button 
        type="button" 
        class="btn btn-primary dropdown-toggle"
      >Manage Recipe <span class="caret"></span>
```

appDropdown not recognized

actually recognized, becauase we added DropDownDirective in app.module.ts

but its not enabled for our feature module

we can't import it to our recipes module

besides the dropdown, our applicaiton is working fine

# 274. Understanding Shared Modules

- appmodule

	- feature module 1
		- component
		- component
		-directive

	- featuremodule 2
		- component
		- component
		- directive


we have a directive share through different modules

weather betwen feat modules, or app module - feature module

in this case, we can put this directives into a shared module

a module not containing a feature

only sth that is shared between modules

like our dropdown directive

how can we build this and use it?

# 275. Creating a Shared Module

in shared folder

shared.module.ts

typically only one

is a normal ngmodulel, but we use it differently

in ngmodule, we declare components, pipes or directives we want to share

is our dropdowndirective

once its declared, we want to make it available outside the module

why are we exporting it?

```js
@NgModule({
	declarations: [
		DropdownDirective
	],
	exports: [
		DropdownDirective
	]
})
```

every component directive pipe u used, has to be declared somewhere in your app


in some module, it has to be declared once, only one

not multiple times, it gives an error

the idea behind the shared module is, we can import the shared module, into the other modules

therefore, to be able to use the dropdowndirective, we have to export it

bydefault, everything you setup in a module, is just available in a module

it isnt accessible from outside

to make it accesible, we need to export them

we can also add the commonmodule

```js
@NgModule({
	declarations: [
		DropdownDirective
	],
	exports: [
		CommonModule,
		DropdownDirective
	]
})
```

we export the commonmodule

we didnt even imported first.

we could imported it, but its not necesarry

now, we 

app.module.ts

```js
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ShoppingListComponent,
    ShoppingEditComponent,
    // DropdownDirective,
    SignupComponent,
    SigninComponent
  ],
```

we must declare it once in our app

and then

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  FormsModule,
  HttpModule,
  RecipesModule,
  SharedModule
],
```

important, we eaxport the commonmodule, that doesnot interfere with our browser module



so, we added the sharedmodule to the app.module

lets do the same in the recipes module

```js
imports: [
	CommonModule,
	ReactiveFormsModule,
	RecipesRoutingModule,
	SharedModule
]
```

now the dropdown is working

we have the same state as before

but we are using a feature module and a shared module

lets quickly optimize our app, creating a feature module for the auth, and for the shopping list section

# 276. Creating a Shopping List Feature Module

in shopping-list folder

shopping-list.module.ts

```js
import { NgModule } from "../../../node_modules/@angular/core";
import { ShoppingListComponent } from "./shopping-list.component";
import { ShoppingEditComponent } from "./shopping-edit/shopping-edit.component";

@NgModule({
  declarations: [
		ShoppingListComponent,
		ShoppingEditComponent
	]
})

export class ShoppingLlistModule {}
```

i wont add the provider of my shopping list service, because i want to use it app wide

which modules we use here?

we could import the shared module, but we dont need the dropdown module, it wouldnt hurt the app, but it doesnot make any sesne
let simport just the commonmodule

formsModule unlocks all the forms directive like ngModelgroop, ngmodel etc

```js
imports: [
	CommonModule,
	FormsModule
]
```

in the app module

we delete the FormsModule, since we are not using it anywhere else in the app
if we were using it. we would leave it inappmodule, and in shoppinglist we still have to import it again, because the directives jsut live inside a module, they need to have the module

only servicesneed to be provided once and not in multiple modules

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  HttpModule,
  RecipesModule,
  SharedModule,
  ShoppingLlistModule
],
```

we just have one route for the shopping list, we dont need to do a separate module routing

# 277. Loading Components via Selectors vs Routing

we said that we leave the shopppinglist comopnent route in the app-routing.module.ts

```js
	{ path: 'shopping-list', component: ShoppingListComponent },
```

sth important: we have to reference the ShoppingListComponent, we have to leave the 

```js
import { ShoppingListComponent } from "./shopping-list/shopping-list.component";
```

the import is a language feature, ts needs to know where to find this component, this type

since this is only a ts thing

and the shoppinglist component is declared in the shopping-list.module

how could we do that, because the app-routing module is not directly related with teh shoppinglist moduel

we declare the shoppinglist component somewhere else than we use it

thats the special thing about routing

and nthats the same for all components

for routing, its not important that you declare the component in the same file as the routes live


its just important that you declare them anywhere in your application, before you get a chance to visit this route

its a different thing, if you decide to use the selector of a component

if yo udecide to use the app-shopping-list selector

shopping-list.component.ts

```js
@Component({
  selector: 'app-shopping-list',
```

normally we load it by a routing

```html
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <app-shopping-list></app-shopping-list>
      <router-outlet></router-outlet>
    </div>
  </div>
</div>
```

error, appshopinglist is not a known element

cuz this element is not known by the appcomponent, which is part of the app module

because we didnt declare it in app.module.ts

thats the difference between the selector and routing

for the selector you have to declare it in the module where you plan to use the selector, or you have to import the module that exports that thing

like with the dropdown directive

it was not defined in the recipe module,but in the sharedmodule

you have to get a direct connection, either in the same file, or because you import the module

for routing, thats different

we dont have to have this connection

you have to make sure, that the component you want to load, is declared somewhere, and definitely,declared before you try to access the route

# 278. A common goTCHA

shopping-list.component.ts

we have an error, 


```js
Uncaught Error: Template parse errors:
'app-shopping-list' is not a known element:
```

we are using ngModule in our sign in sign up components

we have to make sure,that the forms module,which simply unlocks some directives

you have to make sure, that theis formsmodule is provided in every, not just one, where you want to use this directives

we could export it, from the shoppinglist module

shopping-list.module.ts

```js
exports: [
	FormsModule
]
```


its working, but thats not what we should do

lets create a feature module for the auth section

and import it there

# 279. Creating the Auth Feature Module

auth folder

auth.module.ts

```js
import { NgModule } from "../../../node_modules/@angular/core";
import { SigninComponent } from "./signin/signin.component";
import { SignupComponent } from "./signup/signup.component";
import { FormsModule } from "../../../node_modules/@angular/forms";


@NgModule({
  declarations: [
		SigninComponent,
		SignupComponent
	],
	imports: [
		FormsModule
	]
})

export class AuthModule {}
```

in app.module.ts

import it in imports

now, we canoutsource the routes for the auth module

auth folder, 

auth-routing.module.ts

we register our routes with forChild

we want to use this routing in the auth module

auth.module.ts

```js
@NgModule({
  declarations: [
		SigninComponent,
		SignupComponent
	],
	imports: [
		FormsModule,
		AuthRoutingModule
	]
})
```

we imported the auth.module in the app.module

next step, dive deeper, how can we use this feature modules to improve the performance of our paplication


and which roles services play in our feature modules

# 280. Understanding Lazy Loading

Modules and Routing (lazy loading)

we did feature modules to restructure our code, to see, which elements our features use

we didnt improve the performance of our app

what is lazy loading

we have a root router, which has child routes

root router, we register routes with forRoot

also, we have a featuremodule, with its own routes. child router, 


maybe, theuser never visit this routes, of the featuremodule

still, our overall js bundle, is downloaded at the beginning
a lot of the code, may never be used

we downlod too much code at the beginning

that is lazy loading, we just load when needed (child router)

that means, this module, feature module, is only module, if we actually visit a route, leading to this module

lets ipmlement it
# 281. Adding Lazy Loading to the recipes module

app.module.ts


we see thre areas, the auth feature, the recipe feature and the shopping list feature

we will visit the auth feature always

the recipe feature also, because thats where we land 

we might not always visit the shopping list area

lets add a new component, just to make the recipes visit optional, because there we have the most code

```js
ng g c home --spec false
```

in the template

home.component.html

```html
<h2>Welcome to the Recipe book</h2>
```

app-routing.module.ts

```js
const appRoutes: Routes = [
	{ path: '', redirectTo: '/recipes', pathMatch: 'full' },
	{ path: 'shopping-list', component: ShoppingListComponent },
];
```

to

```js
const appRoutes: Routes = [
	{ path: '', component: HomeComponent },
	{ path: 'shopping-list', component: ShoppingListComponent },
];
```

now, if we go to /, we see, welcome to the recipes book

that means, we not always go to the recipes, so lets implement lazy loading

how can we load this module lazily?

issue is, we always load it eagerly, because we always imort the recipes module in the 

app.module.ts

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  HttpModule,
  RecipesModule,
  SharedModule,
  ShoppingLlistModule,
  AuthModule
],
```

at the point of time, our applicaiton launches, it is bootstrap with the appmodule

everything listed in the imports, its imported

we dont want to load recipesmodule eagerly

we need to remove it somehow from the imports array.

we delete it

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  HttpModule,
  // RecipesModule,
  SharedModule,
  ShoppingLlistModule,
  AuthModule
],
```

so that webpack doesnt add this recipes module to our initial bundle anymore

since webpack doesnt add this module to the bundle anymore

everrything we reference in recipes.module.ts is not added anymore



all that stuff

```js
import { RecipeListComponent } from './recipe-list/recipe-list.component';
import { RecipeEditComponent } from './recipe-edit/recipe-edit.component';
import { RecipeDetailComponent } from './recipe-detail/recipe-detail.component';
import { RecipeStartComponent } from './recipe-start/recipe-start.component';
import { RecipesComponent } from './recipes.component';
```

we now load less code, onthe other hand we broke our app

```js
Error: Cannot match any routes. URL Segment: 'recipes'
```


we should fix this, we want to load lazily

app-routing file

we reintroduce a route we had there before

it points to recipes, but we dont load the recipescomponent

app-routing.module.ts

```js
const appRoutes: Routes = [
	{ path: '', component: HomeComponent },
	{ path: 'recipes', component: RecipesComponent },
	{ path: 'shopping-list', component: ShoppingListComponent },
];
```

dont do this!!!!

that would load it eagerly

we use another property, loadChildren!
it takes a string, not a type. for copmonent we alyways pass a type, thats why we need to import

so now, we put a string, and point to the module we want to load, lazily, at the point of time we visit this route

```js
const appRoutes: Routes = [
	{ path: '', component: HomeComponent },
	{ path: 'recipes', loadChildren: './recipes/recipe.module#RecipesModule' },
	{ path: 'shopping-list', component: ShoppingListComponent },
];
```

now we have to specify the name of the class in this file after the hash


cuz, technically we could name it everything that we want, its not connect it to the file name




with this, whenever we visit the recipes, it will dynamically load the recipes module

but only when we enter "recipes" in the url

so we need to make a tiny change

in recipes-routing, we have also the 'recipes' path


```js
const recipesRoutes: Routes = [
  { path: 'recipes', component: RecipesComponent, children: [
```

this will not work, we added reicpes in the recipes routes module

to

```js
const recipesRoutes: Routes = [
  { path: '', component: RecipesComponent, children: [
```

the whole recipes module is loaded when we visit / recipes

we need to restart the server

it works

if we go to the network tab

make sure that the disabled cache is checked

if we reload the page, all the files are loaded initially

but if i click recipes

go to the network, we see backend.js in the home page

if i click reicpes, i see a new file

we can add add lazy loading to shopping list

now, we will see what happens, if we provide services, anywhere else than in our app.module.ts

right now, its the only place where we provide them


# 282. Protecting Lazy Loaded Routes with canLoad

What if you want to use route protection (canActivate  to be precise) on lazily loaded routes?

You can add canActivate to the lazy loaded routes but that of course means, that you might load code which in the end can't get accessed anyways. It would be better to check that BEFORE loading the code.

You can enforce this behavior by adding the canLoad  guard to the route which points to the lazily loaded module:

{ path: 'recipes', loadChildren: './recipes/recipes.module#RecipesModule', canLoad: [AuthGuard] } 

In this example, the AuthGuard  should implement the CanLoad interface.

https://angular.io/api/router/CanLoad

# 283. How Modules and Services Work Together

in the last section we added lazy loading to our app

lets find how modules and the injectional services are connected

how it works behind the scenes

we have an app, feature module, not lazy, and a lazy loaded feature module

- app module
	providers: [LogService]
	
	- feature module
	 providers: [LogService]

	- lazy loaded feature module



we have this logservice, and we add it in the array providers, from the app module and feature module, sth we shouldn do

here it wont hurt you if we do it

in the end, we have a root injector, created by angular

at the point of time the applicataion starts.

since this feature module is not loaded lazy, by added to the import array of the app module

there is only one root injector, all the services we provide in eithr of the modules, is added to that root injector

in the whole app, if we inject logservice, we will use one and the same instance

we wont have a special instance for the feature module

only one root injector is created





if we were to inject this service in the lazy loaded feature module

it will also use the root injector

just one instance, eventhough we load later, the root injector was created at the very beginning

therefore, we would use the instance in the whole application, including the laz loaded feature module

what happens if we have the logservice in our providers array in the lazy loaded feature module?

- app module
	providers: [LogService]
	
	- feature module
	 providers: [LogService]

	- lazy loaded feature module
	 providers: [LogService]



the app module and the feature module, they will still merge the providers together

create a root injector, one instance of logservice

at the point of time we load the lazy loaded feature module

angular will create a child injector for the providers: [LogService]

we use a differnt instance of the logservice in this module

this is on purpose

the fact, if you load amodule eagerly lor laizly, can change how many instances you will use



side node:

enforce 'Module Scope' by providing in a component instead of a module

if you want to enforce a new instance with eager load, then you provide it in a module

in a component leve, a service, it also child injector




lets say, we dont provide the service in teh lazy loaded feature module
like in the first user case

now we have a shared module, with a providers: logservice

we should avoid this for sure

at the point of time, lazy loaded feature moduel, angular gives you a child injector

!!! don't provide services in shared modules!

especially not, if you plan to use them in lazy loaded modules

# 284. Understanding the Core Module

Core Module

- core module
	- appmodule

		- app component
		- component
		- directive


		feature module
			- component
			- component
			-directive



in the first blook, where we see the app component, component and directive

the component, like a header, and the directive,they blong to all application, they should be in the core module

only for restructuring reasons

# 285. Creating a Basic Core Module

we see, in the

app.component.html

```html
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <router-outlet></router-outlet>
    </div>
  </div>
</div>
```

we have the header. this header, we only use in our app component

that is sth we want to put in such a core module

we can also then use the core module, to bundle all the imports and provideres in there

unlike the shared module, the core module will be only imported by the root module, the app module here, 
so no problem

new folder in our app folder

folder core, i want to move theheader into this folder

and also the home. the home we also have in the root level

lets add a core module file in core folder

core.module.ts

we need the dropdown directive in the header

we need the approutingmodule, we need the router module, which we export there, as we have routerlinks in the header

what do we export

the approutingmodule, we also need it in our main module, in the app module

we have the router-outlet

i also want to export, the headercomponent. i am using the selector in my app.component.html

```html
<app-header></app-header>
```

if we dont export the headercomopnent, we can not use it. now hte headercomponent is just in the core module. its closed

```js
import { NgModule } from "../../../node_modules/@angular/core";
import { HomeComponent } from "./home/home.component";
import { HeaderComponent } from "./header/header.component";
import { SharedModule } from "../shared/shared.module";
import { AppRoutingModule } from "../app-routing.module";

@NgModule({
  declarations: [
		HeaderComponent,
		HomeComponent
	],
	imports: [
		SharedModule,
		AppRoutingModule
	],
	exports: [
		AppRoutingModule,
		HeaderComponent
	]
})

export class CoreModule {}
```

app.module.ts

```js
@NgModule({
  declarations: [
    AppComponent,
    // HeaderComponent,
    // HomeComponent,
  ],
```

in declarations, its great to have just appcomonent

now, we import our code module

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  HttpModule,
  SharedModule,
  ShoppingLlistModule,
  AuthModule,
  CoreModule
],
```

now we can use again our app-header selector

!!!we didnt need to export the home component, because for routing, we dont need it

not working

```js
ERROR in src/app/app-routing.module.ts(4,31): error TS2307: Cannot find module './home/home.component'.
```

we cant find the home component

app-routing.component.ts

```js
import { HomeComponent } from "./core/home/home.component";

```

we need to update the path

now its working


what can we do with our services, we provide application wide

# 286. Restructuring Services to use the Child injector

app.module.ts

```js
providers: [ShoppingListService, RecipeService, DataStorageService, AuthService, AuthGuard],
```

services in providers should not be in shared modules

but you can distrubte them into multiple modules

what we can do, is, 

we can cut all providers, and put them in the 

core.module.ts

```js
@NgModule({
  declarations: [
		HeaderComponent,
		HomeComponent
	],
	imports: [
		SharedModule,
		AppRoutingModule
	],
	exports: [
		AppRoutingModule,
		HeaderComponent
	],
	providers: [
		ShoppingListService, RecipeService, DataStorageService, AuthService, AuthGuard
	]
})
```

it stills provides one instance in the whole application.

angular will provide all them togheter as long as the core module is loaded eagerly

and it is loaded eagerly, we import the core module in the app.module.ts

we can remove providers from our app.module.ts, then its cleanr

there is one special service, the AuthGuard

it is also provided application wide

but, the only place where we use the auth guard is in the recipes-routing.module

recipes-routing.module.ts

```js
const recipesRoutes: Routes = [
  { path: '', component: RecipesComponent, children: [
		{ path: '', component: RecipeStartComponent },
		{ path: 'new', component: RecipeEditComponent, canActivate: [AuthGuard] },
		{ path: ':id', component: RecipeDetailComponent },
		{ path: ':id/edit', component: RecipeEditComponent, canActivate: [AuthGuard] }
	] },
]
```

if thats the case, that you use a service just in one module

we can remove it from core module

and now only imported in the recipes-routing.module.ts

in the provideres array

we added more code, that will be loaded lazily, since we are in the recipes

sidenote: guards are the only services, that you add, to sth-routing.module.ts

other services, should be in the recipes.module.ts

it does nt affect, since at the end everything goes to the child feature module

but its semantic



# 287. Using Ahead-of-Time Compilation

angular, offers two types of comiping your code

what does comipiling the code?

that does not mean, compile the code frm ts to js

that is done by the cli

not related to angular

angular needs to compile your templates. you write them in html code

angular parses this html files, and compiles the html code into js

you can alwys represent html in js

accessing js is fasater, than accessing the dom in the browser

performance improvements

it can do this ocmplilation in two different places

- just-in-time compilation

- ahead-of-time compilation



in justintime compilation, thats what we used in development

then, we ship it, when we run ngserver

to production

then, we view it in the browser, so the whole source code gets downloaded

App downloaded in Browser

once it is downloaded, angular bootstrap the applicaiton, and in this step

it also parses and compiles all the templates, to js

this is the first time he has the change to do so, before that, the app never run

there is a disadvantage in this approach

ng didnt have a change, because we didnt let it

sum up
just-in-time compilation

development
production
app downloaded in browser
angular parses & compiles templates (to javascript)

Ahead-of-time Compilation

we can change the procedure, we still have a development step

then, we basically allow angular to compile the template

after we are done, developing our code, our templates are done

our templates could contain dynamic content

thats the idea behind creating a web app

but angular will know where we use string intrpolation, ng will also compile this into js

we only allow angular to understand our templates, in an earlier point of time

we compile before we run the app in the browser

sum up

development

angular parses & compiles templates (to javascript)

production

app downloaded in browser

that has a couple of advantatges

our app starts faster.

it also means, our templates get checked




faster startup since parsing and compilation doesnt happen in browser

templates get checked during development

smaller file size as unused features can be stripped out and the compiler itself isn't shipped

if you dont use ngif in your application, angular, it is able to remove the code form ngif away

lets see, how can we use it, in the cli, in our app

# 288. How to use AoT Compilation with the CLI

lets open the network tab

the vendor.js is the biggest file, 5.4 mb

this value should shrink, because this code contains the compiler and more stuff

lets use the cli

for aot

stop ngserve

```js
ng build
```

it builds it just it builds when you run ng serve

```js
ng build --prod
```

it optimizes the code, and minifies the code


we can add

```js
ng build --prod --aot
```

this will strip out the compiler etc

```js
ERROR in src/app/core/header/header.component.html(14,18): : Property 'authService' is private and only accessible within class 'HeaderComponent'.
```

in header.component.html

we are doing

```html
<li class="dropdown" appDropdown *ngIf="authService.isAuthenticated()">
```

to

```html
<li class="dropdown" appDropdown *ngIf="isAuthenticated()">
```

header.component.ts

```js
isAuthenticated() {
	return this.authService.isAuthenticated();
}
```


new error

```js
ERROR in src/app/recipes/recipe-edit/recipe-edit.component.html(68,13): : Property 'controls' does not exist on type 'AbstractControl'.

```

recipe-edit.component.html

```html
<div 
  class="row" 
  *ngFor="let ingredientCtrl of recipeForm.get('ingredients').controls; let i = index"
  [formGroupName]="i"
  style="margin-top: 10px;"
>
```

to

```html
*ngFor="let ingredientCtrl of getControls(); let i = index"
```

recipe-edit.component.ts

```js
getControls() {
  return (<FormArray>this.recipeForm.get('ingredients')).controls;
}
```

now it works

```js
Date: 2018-07-10T07:27:24.635Z
Hash: 341473cd17d5c960c22f
Time: 52524ms
chunk {0} 0.17c22ad224eac1bedb43.js () 23.1 kB  [rendered]
chunk {1} runtime.e69b9c4643fe1a8a5986.js (runtime) 1.84 kB [entry] [rendered]
chunk {2} styles.1817f60d06d502d4a471.css (styles) 114 kB [initial] [rendered]
chunk {3} polyfills.92108b287fe28032870b.js (polyfills) 59.6 kB [initial] [rendered]
chunk {4} main.800418b7644752eee5ad.js (main) 1.11 MB [initial] [rendered]
```





it works, if we open the folder of the project in finder

to the dist folder, which is created when we run ng build

the vendor, and the main file

the main file contains our app code

the vendor file, is now

i dont find it


# 289. Preloading Lazy Loaded Routes

we learnt how to use modules and lazy, 

these are the basic optimizations 

lets think of advanced things

if you lazy load in your app

we still have the effect, as soon we visit the app

/recipes

we load, the whole chunk at this point of time

it might give you a tiny window of time, where your application hangs

because the code needs to be downloaded

it could take some miliseconds

it would be nice, if we use lazyloaded, and we preload the code

at the time you visit the webpage, you dont load the code

but once the user is visitng the webpage and using different areas that are not lazy loaded, you preload the lazy loaded features

so if the user decides to visit these areas in the future , you have the code already available


how?

super easy to enable

app-routing.module.ts

where we define our routes, 
where we call forRoot, we can pass a second argument.
a js object, where we can configure this roUTERmodule

preloadingStrategy: the type of preloading strategy you want to use

the defualt is dont preload

to preload, you can add the PreloadAllModules

```js
@NgModule({
	imports: [RouterModule.forRoot(appRoutes)],
	exports: [RouterModule]
})
```

to

```js
@NgModule({
	imports: [RouterModule.forRoot(appRoutes, { preloadingStrategy: PreloadAllModules })],
	exports: [RouterModule]
})
```

preloads, all lazyloaded modules at the time the page is loaded

once it runs

you can add your own strategies, its more advanced

if we see network, we see that the chunk is now loaded

you can tell that its preloaded, because its after vendor and main files

when we click now recipes, we just see the images as new dowloadeds

# 290. Wrap up

Using Modules & Optimizing an Angular App
























































