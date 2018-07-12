# Section 14: Course project - observables

# 170. Improving the Reactive Service with Observables(subjects)

clean up work in recipe services



recipe.service.ts

destroy

```js
recipeSelected = new EventEmitter<Recipe>();
```

we are not using that anymore

we change the way we select our recipes

recipes.component.ts

remove our subscription

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

and remove

```js
selectedRecipe: Recipe;
```

remove the injection of the service

we end up with

```js
import { Component, OnInit } from '@angular/core';
import { RecipeService } from './recipe.service';

@Component({
  selector: 'app-recipes',
  templateUrl: './recipes.component.html',
  styleUrls: ['./recipes.component.css'],
  providers: [RecipeService]
})
export class RecipesComponent implements OnInit {
  constructor() { }

  ngOnInit() {}
}
```

i want to use an observable to improve our shopping list service

shopping-list.service.ts

we are using an event emitter to inform our components about changes

```js
export class ShoppingListService implements OnInit {

	ingredientsChanged = new EventEmitter<Ingredient[]>();

	private ingredients: Ingredient[] = [
		new Ingredient('Apples', 5),
		new Ingredient('Tomatoes', 10),
	];
```

that is ok, but better to use a subject

```js
	ingredientsChanged = new Subject<Ingredient[]>();
```

```js
import { Subject } from "rxjs";
```

with that, tiny changes

when we call emit

we need to call next, 

```js
	addIngredient(ingredient: Ingredient) {
		this.ingredients.push(ingredient);
		this.ingredientsChanged.next(this.ingredients.slice());
	}
```

in the places where we listened to this changes, the subscribe function just worked like in the event emitter

we do subscribe to our own subject here

for that, angular wont handle our subscription. we should unsubscribe manually

shopping-list.component.ts

```js
  private subscription: Subscription;
```

```js
ngOnInit() {
    this.ingredients = this.shoppingListService.getIngredients();
    this.subscription = this.shoppingListService.ingredientsChanged
      .subscribe(
        (ingredients: Ingredient[]) => {
          this.ingredients = ingredients;
        }
      )
  }
```

```js
export class ShoppingListComponent implements OnInit, OnDestroy {
```

```js
  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
```

avoid memory leaks


header.component.html

when we click the recipe book

```html
<a href="#" class="navbar-brand">Recipe Book</a>
```

to

```html
<a routerLink="/" class="navbar-brand">Recipe Book</a>
```





























