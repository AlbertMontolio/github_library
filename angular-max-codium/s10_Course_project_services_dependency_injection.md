# Section 10 Course - project - Services & Dependency Injection

# 105. Introduction

root
	header
		shopping list
			shopping list
			shopping list item
			ingredient

		recipe book
			recipe list
			recipe item
			recipe detail
			recipe


we need a srevice for both areas, shopping list service, recipe service

we should place services in the folder from the feature they belong to

recipes/recipe.service.ts

```js
export class RecipeService {
    
}
```

shopping-list/shopping-list.service.ts

# 107. managing recipes in the recipe service

where we manage our recipes

we should take our recipes, that we manage in our recipe-list, and manage it in our recipeservice

# recipe.service.ts

```js
import { Recipe } from "./recipe.model";

export class RecipeService {
	private recipes: Recipe[] = [
    new Recipe("A test recipe", "This is simply a test", "https://www.seriouseats.com/recipes/images/2017/06/20170627-summer-nachos-matt-clifton-1-1500x1125.jpg"),
    new Recipe("A test recipe 2", "This is simply a test 2", "https://www.seriouseats.com/recipes/images/2017/06/20170627-summer-nachos-matt-clifton-1-1500x1125.jpg")
  ];
}
````

we can not access this array from the outside

with a method yes, getRecipes

arrays an objects are references in typescrit

returns a new array, exactly the same as recipes. we just get a copy
that way, we can not really touch array recipes

```js
	getRecipes() {
		return this.recipes.slice();
	}
```

letes provide our recipe services in our recipes component

recipes.component.ts

```js
  providers: [RecipeService]

```

add providers

recipes component have the childs




recipe-list.component.ts

we delete all the recipes

```js
recipes: Recipe[];
```

well get teh value on ngoninit

we need to inject the service. we can do it, since we provided it in the parent component

```js
  constructor(private recipeService: RecipeService) { }

```

```js
  ngOnInit() {
    this.recipes = this.recipeService.getRecipes();
  }
```

# 108. Using a service for cross component communication


lets use this service, we have this long chain inputs and outputs

we have teh selected recipe, from the recipe-item comopnent, to the recipe-list component, to the recipes component

and then all the way done to the recipe detail

thats too long, to just infom a component that we selected an item

recipe-item.component.ts

```js
  @Output() recipeSelected = new EventEmitter<void>();

```

ii dont want to emit an event, we get rid of it

```js
import { Component, OnInit, Input, EventEmitter, Output } from '@angular/core';
import { Recipe } from '../../recipe.model';

@Component({
  selector: 'app-recipe-item',
  templateUrl: './recipe-item.component.html',
  styleUrls: ['./recipe-item.component.css']
})
export class RecipeItemComponent implements OnInit {

  @Input() recipe: Recipe;
  @Output() recipeSelected = new EventEmitter<void>();

  constructor() { }

  ngOnInit() {
  }

  onSelected() {
    this.recipeSelected.emit();
  }

}
```

```js
import { Component, OnInit, Input } from '@angular/core';
import { Recipe } from '../../recipe.model';

@Component({
  selector: 'app-recipe-item',
  templateUrl: './recipe-item.component.html',
  styleUrls: ['./recipe-item.component.css']
})
export class RecipeItemComponent implements OnInit {

  @Input() recipe: Recipe;

  constructor() { }

  ngOnInit() {
  }

  onSelected() {
  }

}
```

i want to call some method in my service which will then transport the data

!!! cross component, we use an event emitter from the service

recipe.service.ts


```js
	recipeSelected = new EventEmitter<Recipe>();
```

public property, recipeSelectd
it will be an object, instantiated by using event emitter, form angular core
it will hold some recipe data

we could encapsulate this by providing a method to get access to it when we want to emit the event

but, i will use the property itself

recipe-item.component.ts

first of all, inject the service

```js
  constructor(private recipeService: RecipeService) { }
```

then we can use this recipeSelected event emitter and call emit, of this recipe compomnent, the one that we seelcted

```js
  onSelected() {
    this.recipeService.recipeSelected.emit(this.recipe)
  }
```

recipe-list.component.html

get rid of where we were listening

```html
<div class="row">
  <div class="col-xs-12">
    <app-recipe-item 
      *ngFor="let recipeEl of recipes"
      [recipe]="recipeEl"
      (recipeSelected)="onRecipeSelected(recipeEl)" <----------
    ></app-recipe-item>
  </div>
</div>
```

we remove listener recipeSelected

get rid of

recipe-list.component.ts

```js
  onRecipeSelected(recipe: Recipe) {
    this.recipeWasSelected.emit(recipe);
  }
```

get rid of


```js
  @Output() recipeWasSelected = new EventEmitter<Recipe>();
```

recipes.component.html

get rid of

```html
(recipeWasSelected)="selectedRecipe = $event"
```

the only thing i want to do, in the component

recipes.component.ts

remember the recipes.component.html

```html
<div class="row">
  <div class="col-md-5">
    <app-recipe-list
      
    ></app-recipe-list>
  </div>
  <div class="col-md-7">
    <app-recipe-detail
      *ngIf="selectedRecipe; else infoText"
      [recipe]="selectedRecipe"
    ></app-recipe-detail>
    <ng-template #infoText>
      <p>please select a recipe!</p>
    </ng-template>
  </div>
</div>
```


recipes.component.ts

inject the recipeService

then, this component, and all its childs, use this instance of the service

in ngOnInit()

i can set up my listener
i can subscribe to it and get inform to any changes

```js
  ngOnInit() {
    this.recipeService.recipeSelected
      .subscribe(
        (argument list) => {
          function body
        }
      );
  }
```

i know i get a recipe, we defined it like that

set the selectedRecipe to the recipe we get from the event

```js
  ngOnInit() {
    this.recipeService.recipeSelected
      .subscribe(
        (recipe: Recipe) => {
          this.selectedRecipe = recipe;
        }
      );
  }
```

# 109. Adding the shopping list service

shopping-list.service.ts

manage our shopping list, list of ingredients.
add ingredient method

first of all, initalize our ingredient,s but we make them private, and we access them through a method, and we just return a copy of that



```js
export class ShoppingListService implements OnInit {
	private ingredients: Ingredient[] = [
		new Ingredient('Apples', 5),
		new Ingredient('Tomatoes', 10),
	];

	getIngredients() {
		return this.ingredients.slice();
	}
```

shopping-list.component.ts

where should we provided the service? we could provide it here
but later we want to access it from my recipe section

we provide it in app.module.ts

```js
providers: [ShoppingListService],
```

use this service application wide

shopping-list.component.ts

```js
  constructor(private shoppingListService: ShoppingListService) { }
```

```js
  ngOnInit() {
    this.ingredients = this.shoppingListService.getIngredients();
  }
```

shopping-list.service.ts

```js
	addIngredient(ingredient: Ingredient) {
		this.ingredients.push(ingredient);
	}
```

shopping-edit.component.ts

get rid of 

```js
    this.ingredientAdded.emit(newIngredient)
```

```js
  @Output() ingredientAdded = new EventEmitter<Ingredient>();

```

in the shopping-edit.component.html

remove our listener

```js
(ingredientAdded)="onIngredientAdded($event)"
```

shopping-list.component.ts

```js
  onIngredientAdded(ingredient: Ingredient) {
    this.ingredients.push(ingredient);
  }
```

well do this in the service

shopping-edit.component.ts

```js
  constructor(private slService: ShoppingListService) {
```

```js
  onAddItem() {
    const ingName = this.nameInputRef.nativeElement.value;
    const ingAmount = this.amountInputRef.nativeElement.value;
    const newIngredient = new Ingredient(ingName, ingAmount);
    this.slService.addIngredient(newIngredient);
  }
```

ingredients not showing up

# 110. using services for "push notifications"

the reason, when we call get ingredients, we only get a slice of ingredients array, a copy

the ingredient is added to the original array, not to the copy one

couple of ways of solving
we could remove the slice.

another solution

we have to inform our component, that new data is available

shopping-list.service.ts

new event emitter, ingredientsChanged

this event emitter, can emit our ingredient array

in addIngredient, whenever we change our array, we call the ingredientsChanged and we emit the event, and we need to pass of course the array, the copy of it

```js
	ingredientsChanged = new EventEmitter<Ingredient[]>();

```

```js
	addIngredient(ingredient: Ingredient) {
		this.ingredients.push(ingredient);
		this.ingredientsChanged.emit(this.ingredients.slice());
	}
```

shopping-list.component.ts

besides getting the ingredients when we load the app

i also want to reach out to my slService, and subscribe to this ingredientsChanged event

```js
  ngOnInit() {
    this.ingredients = this.shoppingListService.getIngredients();
    this.shoppingListService.ingredientsChanged
      .subscribe(
        (ingredients: Ingredient[]) => {
          this.ingredients = ingredients;
        }
      )
  }
```

# 111. Adding ingredients to recipes

on the recipes section, i want each recipe to have some ingredients

and then, to Shopping list, send ingredients to shopping list

recipe.model.ts

```js
import { Ingredient } from "../shared/ingredient.model";

export class Recipe {
  public name: string;
  public description: string;
  public imagePath: string;
  public ingredients: Ingredient[];

  // when we call new, the constructor is called!
  constructor(name: string, desc: string, imagePath: string, ingredients: Ingredient[]) {
    this.name = name;
    this.description = desc;
    this.imagePath = imagePath;
    this.ingredients = ingredients;
  }
}
```

i expect to recive some ingredients

recipe.service.ts

```js
recipes: Recipe[] = [
  new Recipe(
		"tasty schnitzel", 
		"its so tasty the schnitzel",
		"https://www.seriouseats.com/recipes/images/2017/06/20170627-summer-nachos-matt-clifton-1-1500x1125.jpg",
		[
			new Ingredient('meat', 1),
			new Ingredient('french fries', 20)
		]
	)
	,
	new Recipe(
		"burger", 
		"amazig burguer with avocad",
		"https://www.seriouseats.com/recipes/images/2017/06/20170627-summer-nachos-matt-clifton-1-1500x1125.jpg",
		[
			new Ingredient('buns', 2),
			new Ingredient('meat', 1)
		]
	)
];
```

recipe-detail.component.html

```html

<div class="row">
  <div class="col-xs-12">
    Ingredient
  </div>
</div>
```

now we have ingredients

```html
<div class="row">
  <div class="col-xs-12">
    <ul class="list-group">
      <li 
        class="list-group-item"
        *ngFor="let ingredient of recipe.ingredients"
      >
        {{ ingredient.name }} - {{ ingredient.amount }}
      </li>
    </ul>
  </div>
</div>
```


# 112. passing ingredients from recipes to the shopping list (via service)


to shopping list button work

recipe-detail.component.html

```js
  <li><a style="cursor: pointer" (click)="onAddToShoppingList()">To Shopping List</a></li>
```

recipe-detail.component.ts

```js
  constructor(private recipeService: RecipeService) { }
```

```js
  onAddToShoppingList() {
    this.recipeService.addIngredientsToShoppingList(this.recipe.ingredients);
  }
```

recipe.service.ts

```js
	addIngredientsToShoppingList(ingredients: Ingredient[]) {

	}
```

we need to access the shoppinglist service

@Injectable()

```js
	constructor(private slService: ShoppingListService) {}

```

```js
	addIngredientsToShoppingList(ingredients: Ingredient[]) {
		this.slService;
	}
```

we need a new method

shopping-list.service.ts

```js
	addIngredients(ingredients: Ingredient[]) {
		for (let ingredient of ingredients) {
			this.addIngredient(ingredient);
		}
	}
```

too many events, better to do it in one step

es6 features, the spread operator, allows us to turn an array of elements into a list of elmeents

the push method, canhandle multiple objects, but not an array. it would put the whole array in one position

```js
	addIngredients(ingredients: Ingredient[]) {
		// for (let ingredient of ingredients) {
		// 	this.addIngredient(ingredient);
		// }
		this.ingredients.push(...ingredients);
		this.ingredientsChanged.emit(this.ingredients.slice());		
	}
```

we need to ionform that our ingredients changed!!!

shopp9ng list is subscribed

now we can call this addIngredients method

in the recipe.service.ts

```js
	addIngredientsToShoppingList(ingredients: Ingredient[]) {
		this.slService.addIngredients(ingredients);
	}
```






















































