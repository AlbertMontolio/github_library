# Section 12: Course Project - Routing

# 144. Planning the General Structure

recipe book

time to add it to our recipe boook

we imlemented the navigation with ngIf

we want to use the angular router

we will not only implement the router, but the recipe add component

we need routes for our shopping list and recipe book

inside recipe book, we need child routes, for recipe detail and recipe edit

# 145. Setting up Routes

route routes, the overall

the main routes, of our application

recipe page, and the shopping list page

app-routing.module.ts

```js
import { NgModule } from "../../node_modules/@angular/core";
import { Routes, RouterModule } from '@angular/router'
import { RecipesComponent } from "./recipes/recipes.component";
import { ShoppingListComponent } from "./shopping-list/shopping-list.component";

const appRoutes: Routes = [
	{ path: '', redirectTo: '/recipes' },
	{ path: 'recipes', component: RecipesComponent },
	{ path: 'shopping-list', component: ShoppingListComponent }
];

@NgModule({
	imports: [RouterModule.forRoot(appRoutes)],
	exports: [RouterModule]
})

export class AppRoutingModule {
  
}
```

we redirect to recipes if we are in the home page, in the /

app.module.ts

```js
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
```

we need a place to render our routes

in app.component.html

```html
<app-header (featureSelected)="onNavigate($event)"></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <router-outlet></router-outlet>
      <!-- <app-recipes *ngIf="loadedFeature === 'recipe'"></app-recipes>
      <app-shopping-list *ngIf="loadedFeature !== 'recipe'"></app-shopping-list> -->
    </div>
  </div>
</div>
```

Error:

Error: Invalid configuration of route '{path: "", redirectTo: "/recipes"}': please provide 'pathMatch'. The default value of 'pathMatch' is 'prefix', but often the intent is to use 'full'.

we will always redirect the router, because the default matching strategy of the reouter, for the path, it will see if its a prefix of the current path, and the empty path which is our dredirect route, uses an empty path, therefore, this is part of any route we visit

that wasnt a problem, when we were redirecting from sth to somewhere else, we had a specific path

but an empty path is path of every route

we have to add the pathMatch: 'full', this overwrites the default prefix, only redirect, if the full path is empty

```js
	{ path: '', redirectTo: '/recipes', pathMatch: 'full' },
```

# 146. Adding navigation to the App

header.component.html

```html
<ul class="nav navbar-nav">
    <li><a href="#" (click)="onSelect('recipe')">Recipes</a></li>
    <li><a href="#" (click)="onSelect('shopping-list')">Shopping List</a></li>
</ul>
``` 

i no longer need the click listeners

header.component.ts

remove on selectmethod

```js
export class HeaderComponent {
	@Output() featureSelected = new EventEmitter<string>();

	onSelect(feature: string) {
		this.featureSelected.emit(feature);
	}
}
```

and the event Emitter

```js
import { Component } from "@angular/core";

@Component({
    selector: 'app-header',
    templateUrl: './header.component.html'
})

export class HeaderComponent {

}
```

header.component.html

```html
<ul class="nav navbar-nav">
	<li><a routerLink="/recipes">Recipes</a></li>
	<li><a routerLink="/shopping-list">Shopping List</a></li>
</ul>
```

we could use square brackets, and pass an arrays, to configure the path

i want to mark the active route

# 147. Marking Active Routes

bootstrap gives as a class


header.component.html

```html
<li class="active"><a routerLink="/recipes">Recipes</a></li>
```

this is the class we dynamically we want to add

```html
<ul class="nav navbar-nav">
	<li routerLinkActive="active"><a routerLink="/recipes">Recipes</a></li>
	<li routerLinkActive="active"><a routerLink="/shopping-list">Shopping List</a></li>
</ul>
```


# 148. fixing page reload issues

one of the recipe icons, we reload the page

recipe-list.component.html

in the recipe-item

we. have a link with the ref attribute

```html
<a href="#" class="list-group-item clearfix"
  (click)="onSelected()"
>
```

```html
<a
  style="cursor: pointer;"
  class="list-group-item clearfix"
  (click)="onSelected()"
>
```

for the drop down

recipe-detail component

```html
<ul class="dropdown-menu">
  <li><a style="cursor: pointer" (click)="onAddToShoppingList()">To Shopping List</a></li>
  <li><a href="#">Edit Recipe</a></li>
  <li><a href="#">Delete Recipe</a></li>
</ul>
```

```html
<ul class="dropdown-menu">
  <li><a style="cursor: pointer" (click)="onAddToShoppingList()">To Shopping List</a></li>
  <li><a style="cursor: pointer">Edit Recipe</a></li>
  <li><a style="cursor: pointer">Delete Recipe</a></li>
</ul>
```

the drop down in the navbar also reloads the page

header.component.html

```html
<li class="dropdown" appDropdown>
	<a href="#" class="dropdown-toggle" role="button">Manage <span class="caret"></span></a>
	<ul class="dropdown-menu">
		<li><a href="#">Save Data</a></li>
		<li><a href="#">Fetch Data</a></li>
	</ul>
</li>
```

to

```html
<li class="dropdown" appDropdown>
	<a style="cursor: pointer;" class="dropdown-toggle" role="button">Manage <span class="caret"></span></a>
	<ul class="dropdown-menu">
		<li><a style="cursor: pointer;">Save Data</a></li>
		<li><a style="cursor: pointer;">Fetch Data</a></li>
	</ul>
</li>
```

lets add some child routes!

# 149. Child route: challenge

child routing for recipes

we need a new component if we are not displaying any selected recipe

app-routing.module.ts

```js
{ path: 'recipes', component: RecipesComponent, children: [
		{ path: '' }
	] },
```

default route, if we dont selected recipe. like recipes/

we need a component to show

```js
ng g c recipes/recipe-start --spec false
```

recipe-start.component.ts

```html
<h3>please select a recipe</h3>
```

app-routing.component.ts

```js
{ path: 'recipes', component: RecipesComponent, children: [
		{ path: '', component: RecipeStartComponent }
	] },
```

now it can be displayed in

recipes.component.html

provide a router outlet for the childs to be rendered

```html
<div class="row">
  <div class="col-md-5">
    <app-recipe-list
      
    ></app-recipe-list>
  </div>
  <div class="col-md-7">
    <router-outlet></router-outlet>
    <!-- <app-recipe-detail
      *ngIf="selectedRecipe; else infoText"
      [recipe]="selectedRecipe"
    ></app-recipe-detail>
    <ng-template #infoText>
      <p>please select a recipe!</p>
    </ng-template> -->
  </div>
</div>
```

if we click, it does not work

app-routing.module.ts

new path

```js
{ path: 'recipes', component: RecipesComponent, children: [
		{ path: '', component: RecipeStartComponent },
		{ path: ':id', component: RecipeDetailComponent }
	] },
```

we have a route that can load the component

# 151. Configuring Route Parameters

recipe.detail.html

we have recipe everywhere

its a property of this component, property set with property binding, with @Input, it awaits to be set from the outside

now this component is set throgh routes

in our route, we pass the id, which is a dynamic parameter


recipe.detail-component.js

we can delete @Input

recipe-item.component.html

we can remove on selected click element

recipe-item.component.html

delete onSelected() method, we can remove the recipeService


recipe-detail.component

fetch the data

```js
  constructor(
    private recipeService: RecipeService,
    private route: ActivatedRoute
  ) { }
```

```js
export class RecipeDetailComponent implements OnInit {

  @Input() recipe: Recipe;
  id: number;

  constructor(
    private recipeService: RecipeService,
    private route: ActivatedRoute
  ) { }

  ngOnInit() {
    // const id = this.route.snapshot.params['id'];
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id']
        }
      );
  }
```

with the id
we want to load the recipe from the recipe service

recipe.service.ts

```js
	getRecipe(index: number) {
		return this.recipes.slice()[index];
	}
```

recipe-detail.component.ts

```js
  ngOnInit() {
    // const id = this.route.snapshot.params['id'];
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
          this.recipe = this.recipeService.getRecipe(this.id);
        }
      );
  }
```

enable the links to work again

# 152. Passing Dynamic Parameters to link

recipe-list, recipe-item.html

```html
<a
  style="cursor: pointer;"
  [routerLink]="[]"
  class="list-group-item clearfix"
  (click)="onSelected()"
>
```

to get the id, i need to pass this extra information from the recipe-item component

recipe-item.component.ts

```js
@Input() index: number;
```

now i can pass the item of the input from outside

recipe-list.component.html

```html
<div class="col-xs-12">
  <app-recipe-item 
    *ngFor="let recipeEl of recipes; let i = index"
    [recipe]="recipeEl"
    [index]="i"
  ></app-recipe-item>
</div>
```

now i have the link, because of theh @Input

recipe-item.component.html

```html
<a
  style="cursor: pointer;"
  [routerLink]="[index]"
  class="list-group-item clearfix"
  (click)="onSelected()"
>
```

# 153. Styling Active Recipe Items

recipe-item.component.html

```html
<a
  style="cursor: pointer;"
  [routerLink]="[index]"
  class="list-group-item clearfix active"
>
```

all marked

```html
<a
  style="cursor: pointer;"
  [routerLink]="[index]"
  class="list-group-item clearfix"
  routerLinkActive="active"
>
```

recipes is still marked, because we are still at recipes, and we are not in exact matching

lets do the new recipe and edit recipe btn


# 154. Adding Editing Routes

```js
ng g c recipes/recipe-edit --spec false
```

we will work on the form later, i just want to load this component

app-routing.module.ts

```js
const appRoutes: Routes = [
	{ path: '', redirectTo: '/recipes', pathMatch: 'full' },
	{ path: 'recipes', component: RecipesComponent, children: [
		{ path: '', component: RecipeStartComponent },
		{ path: ':id', component: RecipeDetailComponent },
		{ path: 'new', component: RecipeEditComponent },
		{ path: ':id/edit', component: RecipeEditComponent }
	] },
	{ path: 'shopping-list', component: ShoppingListComponent }
];
```

we have our new recipe route

i will determin in the component weather we are in edit mode or not

/0/edit is working

/new failsl, because it fials to get a recipe with the id new

recipes/new, due to the way we ordered our routes, it tries to parse new as an id

```html
{ path: ':id', component: RecipeDetailComponent },
{ path: 'new', component: RecipeEditComponent },
```

switch these two routes

```html
{ path: 'new', component: RecipeEditComponent },
{ path: ':id', component: RecipeDetailComponent },
```

# 155. Retrieving Route Parameters

determine if we are in edit mode or not

recipe-edit.component.ts

```js
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '../../../../node_modules/@angular/router';

@Component({
  selector: 'app-recipe-edit',
  templateUrl: './recipe-edit.component.html',
  styleUrls: ['./recipe-edit.component.css']
})
export class RecipeEditComponent implements OnInit {
  id: number;

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
        }
      );
  }

}
```


with that we retrieve that id

i also want to find out wheather w are creating the recipe or adding a new one

```js
editMode = false;
```

i check this when the params change

i want to see if params has an id property

```js
ngOnInit() {
  this.route.params
    .subscribe(
      (params: Params) => {
        this.id = +params['id'];
        this.editMode = params['id'] != null;
      }
    );
}
```

if the id is undefined, it return false, we are in new mode

edit link work, new link work

# 156. Programmatic Navigation to the Edit Page

recipe-list.component.html

```html
<button (click)="onNewRecipe()" class="btn btn-success">New Recipe</button>
```

recipe-list.component.ts

```js
  onNewRecipe() {
    this.router.navigate(['new']);
  }
```

we also new to inform to the router of our current route

```js
  constructor(
    private recipeService: RecipeService,
    private router: Router,
    private route: ActivatedRoute
  ) { }

  ngOnInit() {
    this.recipes = this.recipeService.getRecipes();
  }

  onNewRecipe() {
    this.router.navigate(['new'], {relativeTo: this.route});
  }
```

same for the edit recipe, there we also need to pass our id

recipe-detail.component.html

```js
<ul class="dropdown-menu">
  <li><a style="cursor: pointer" (click)="onAddToShoppingList()">To Shopping List</a></li>
  <li><a (click)="onEditRecipe()" style="cursor: pointer">Edit Recipe</a></li>
  <li><a style="cursor: pointer">Delete Recipe</a></li>
</ul>
```

recipe-detail.component.ts

```js
  onEditRecipe() {
    this.router.navigate(['edit'], {relativeTo: this.route})
  }
```

we dont need the id in this case, 
why were alredy in recipes/0

other approach

```js
onEditRecipe() {
  // this.router.navigate(['edit'], {relativeTo: this.route});
  this.router.navigate(['../', this.id, 'edit'], {relativeTo: this.route});
}
```

# 157. One Note about Route Observables


we are subscribing to params. we dont need to clean up the subscription

if we use observables that we create, therefore not managed by angular, we need to unsubscribe

app.component.html

```js
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <router-outlet></router-outlet>
      <!-- <app-recipes *ngIf="loadedFeature === 'recipe'"></app-recipes>
      <app-shopping-list *ngIf="loadedFeature !== 'recipe'"></app-shopping-list> -->
    </div>
  </div>
</div>
```

we deleted the (featureSelected)="onNavigate($event)"
























































































