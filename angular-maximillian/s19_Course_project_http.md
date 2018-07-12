# Section 19: Course Project - Http

# 248. Introduction

http in our angular app

how to reach out to our back end

our angular app, is looking good, but we dont have persistence


we are not able to store the data the user enters

save data, fetch data

# 249. Setting up Firebase as a Dummy Backend

we use firebase, we could 

firebase, go to the console

new firebase project

ng-recipe-book-albert

on the dashboard, database

for now, just use the database feature

allows us to simply send request to the url

real data base, start in testmode

```js
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

everyone is able to access this database

lets start and save the recipes

put request



# 250. Sending PUT Requests to Save Data


for reaching out to the backend, i want to create a central repository for the methods i use

save data method, fetch data method

where? we could add it to the recipe.service.ts

we could create a new service for this

place it in the shared folder

we will just use it from the recipes area, but maybe we want to access this from other places of our app

create new file, in shared

data-storage.service.ts

```js
import { Injectable } from "../../../node_modules/@angular/core";
import { Http } from '@angular/http';
import { RecipeService } from "../recipes/recipe.service";

@Injectable()

export class DataStorageService {
	constructor(
		private http: Http,
		private recipeService: RecipeService
	) {}
	
	storeRecipes() {
		return this.http.put(
			'https://ng-recipe-book-albert.firebaseio.com/recipes.json',
			this.recipeService.getRecipes()
		);
	}

}
```

header.component.html

```html
<li><a style="cursor: pointer;" (click)="onSaveData()">Save Data</a></li>
```

app.module.ts

```js
providers: [ShoppingListService, RecipeService, DataStorageService],
```

header.component.ts

```js
import { Component } from "@angular/core";
import { DataStorageService } from "../shared/data-storage.service";
import { Response } from '@angular/http';

@Component({
    selector: 'app-header',
    templateUrl: './header.component.html'
})

export class HeaderComponent {
	constructor(
		private dataStorageService: DataStorageService
	) {}

	onSaveData() {
		this.dataStorageService.storeRecipes()
			.subscribe(
				(response: Response) => console.log(response)
			);
	}

}
```

next task, retrieve it

# 251. Geting Back the recipes

data-storage.service.ts

```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json');
}
```

i dont want to use the response in the header.ts

i just want to replace the existing recipes with the ones i get back

i basically want to subscribe to this observable, to instantly fire it whenever the getRecipes is called

we get a response, the response itself its not what we want to store

instead, our recipes, will be the extracted data of our respond

```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json')
		.subscribe(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
				this.recipeService.
			}
		);
}
```

i want to store the recipes, i dont have a method for that

lets quickly implement such a method in the recipe.service.ts

```js
setRecipes(recipes: Recipe[]) {
	this.recipes = recipes;
	this.recipesChanged.next(this.recipes.slice());
}
```

data-storage.service.ts

```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json')
		.subscribe(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
				this.recipeService.setRecipes(recipes);
			}
		);
}
```


we should be able to replace the recipes of our app from the server

header.component.html

```html
<li><a style="cursor: pointer;" (click)="onFetchData()">Fetch Data</a></li>
```

```js
onFetchData() {
	this.dataStorageService.getRecipes();
}
```

we dont need to subscribe, because we are already doing int in the dataStorage service

```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json')
		.subscribe(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
				this.recipeService.setRecipes(recipes);
			}
		);
}
```

if we save a property with no ingredients, the ingredients array does not appear in the nodes of firebase

bad, technically, this recipe does not fulfill the recipe definition declaration

lets fix this tinny issue in the next elcture

# 252. Transforming Response Data to Prevent Errors

if we store a recipe without ingredients, no ingredient property in the server
thats bad

we dont get an error, in the edit component, in the oninit, when we initialize the form,we could get an error, but we check for 

```js
if (recipe['ingredients'])
```

you rely on your recipes always having the recipes array on it

even if its empty

lets fix this, by adding the map operator in the get request

so we can transform the response we get back

i want to transform, 


```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json')
		.map(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
			}
		)
		.subscribe(
			(recipes: Recipe[]) => {
				this.recipeService.setRecipes(recipes);
			}
		);
}
```


```js
getRecipes() {
	this.http.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json')
		.map(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
				for (let recipe of recipes) {
					if (!recipe['ingredients']) {
						recipe['ingredients'] = []
					}
				}
				return recipes;
			}
		)
		.subscribe(
			(recipes: Recipe[]) => {
				this.recipeService.setRecipes(recipes);
			}
		);
}
```

transforming our data, to add the ingredients property, in case we dont have it

now lets add authentication, to allow just logged in users to store and fetch data























































