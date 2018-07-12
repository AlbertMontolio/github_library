# 78. Adding navigation with event binding and ngIf

either load recipes or shopping list

in my app component

```html
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <app-recipes></app-recipes>
      <app-shopping-list></app-shopping-list>
    </div>
  </div>
</div>
```

i should pass the information from the header, what was clicked, to the app.component

header.component.html

```html
<ul class="nav navbar-nav">
    <li><a href="#" (click)="onSelect('recipe')">Recipes</a></li>
    <li><a href="#" (click)="onSelect('shopping-list')">Shopping List</a></li>
</ul>
```

header.component.ts

```js
export class HeaderComponent {
	@Output() featureSelected = new EventEmitter<string>();

	onSelect(feature: string) {
		this.featureSelected.emit(feature);
	}
}
```

this event needs to be listened

tip:

```html
<li><a href="#" (click)="onSelect('recipe')">Recipes</a></li>
```

we overwrite $event, but we can put it later

```html
<li><a href="#" (click)="onSelect('recipe', $event)">Recipes</a></li>
```


app.component.html

```html
<app-header (featureSelected)="onNavigate($event)"></app-header>
```

app.component.ts

```js
export class AppComponent {
  loadedfeature = 'recipe';

  onNavigate(feature: string) {
    this.loadedfeature = feature;
  }
}
```


app.component.html

```html
<div class="col-md-12">
  <app-recipes *ngIf="loadedFeature === 'recipe'"></app-recipes>
  <app-shopping-list *ngIf="loadedFeature !== 'recipe'"></app-shopping-list>
</div>
```

# 79. Passing Recipe Data with Property Binding

from recipe-list.component.html to recipe-item.component.html

```html
<a href="#" class="list-group-item clearfix" *ngFor="let recipe of recipes">
  <div class="pull-left">
    <h4 class="list-group-item-heading">{{ recipe.name }}</h4>
    <p class="list-group-item-text">{{ recipe.description }}</p>
  </div>
  <span class="pull-right">
    <img 
      [src]="recipe.imagePath"
      alt="{{ recipe.name }}" class="img-responsive" style="max-height: 50px;">
  </span>
</a>
```

get rid of the ngFor there, 

recipe-list.component.html

```html
<div class="col-xs-12">
  <app-recipe-item 
    *ngFor="let recipe of recipes"
  ></app-recipe-item>
</div>
```

recipe-item.component.ts

```js
@Input() recipe: Recipe;
```

allows this component property from outside

recipe-list.component.html

```
<div class="col-xs-12">
  <app-recipe-item 
    *ngFor="let recipeEl of recipes"
    [recipe]="recipeEl"
  ></app-recipe-item>
</div>
```

# 80. Passing Data with Event and Property Binding

click on a list item and show the details

recipe-item.component.html

```html
<a href="#" class="list-group-item clearfix"
  (click)="onSelected()"
>
````

recipe-item.component.ts

```js
  onSelected() {

  }
```

the eventemitter wont pass any information, so, type void

```js
  @Output() recipeSelected = new EventEmitter<void>();
```

```js
  onSelected() {
    this.recipeSelected.emit();
  }
```

we don't need to pass the recipe, since in the loop, we already have the recipe!!!

recipe-list.component.html

```html
<div class="col-xs-12">
  <app-recipe-item 
    *ngFor="let recipeEl of recipes"
    [recipe]="recipeEl"
    (recipeSelected)="onRecipeSelected(recipeEl)"
  ></app-recipe-item>
</div>
```

recipe-list.component.ts

!!! we can listen to a custom event of a child form a child

```js
  @Output() recipeWasSelected = new EventEmitter<Recipe>();
```

```js
  onRecipeSelected(recipe: Recipe) {
    this.recipeWasSelected.emit(recipe);
  }
```


recipes.component.html

```html
<div class="col-md-5">
  <app-recipe-list
    (recipeWasSelected)=""
  ></app-recipe-list>
</div>
```

recipes.component.ts
```js
selectedRecipe: Recipe;
```

```html
<div class="col-md-5">
  <app-recipe-list
    (recipeWasSelected)="selectedRecipe = $event"
  ></app-recipe-list>
</div>
````

```html
<app-recipe-detail
  *ngIf="selectedRecipe"
></app-recipe-detail>
```

if its not defined, it returns null, so i define a template for that

```html
<div class="col-md-7">
  <app-recipe-detail
    *ngIf="selectedRecipe; else infoText"
  ></app-recipe-detail>
  <ng-template #infoText>
    <p>please select a recipe!</p>
  </ng-template>
</div>
```


recipe-detail.component.ts

```js
  @Input() recipe: Recipe;
```

now we can bind to that recipe

recipes.component.html

```html
<div class="col-md-7">
  <app-recipe-detail
    *ngIf="selectedRecipe; else infoText"
    [recipe]="selectedRecipe"
  ></app-recipe-detail>
  <ng-template #infoText>
    <p>please select a recipe!</p>
  </ng-template>
</div>
```

recipe-detail.component.html

```html
<h1>{{ recipe.name }}</h1>
```

property binding to display image

```html
  <img [src]="recipe.imagePath" alt="{{ recipe.name }}" class="img-responsive">
```

```html
<div class="row">
  <div class="col-xs-12">
    {{ recipe.description }}
  </div>
</div>
```


now to the shopping list!

# 81. Allowing the user to add ingredients to the shopping list

we have some input fields and btns

we want to add new items

we add local references

shopping-edit.component.ts

```js
<div class="col-sm-5 form-group">
  <label for="name">Name</label>
  <input 
    type="text" 
    id="name" 
    class="form-control"
    #nameInput
  >
</div>
<div class="col-sm-2 form-group">
  <label for="amount">Amount</label>
  <input 
    type="number" 
    id="amount" 
    class="form-control"
    #amountInput
  >
</div>
```

passing them as an argument, or, by selecting them with viewChild, once you click the button, you create an ignredinet, and add to the array

i could pass the local references, even localreferences.value

shopping-edit.component.html

```html
  <button class="btn btn-success" type="submmit" (click)="onAddItem()">Add</button>
```

shopping-edit.component.ts

```js
@ViewChild('nameInput') nameInputRef: ElementRef;
@ViewChild('amountInput') amountInputRef: ElementRef;
```

i get teh values so

now i want to emit a new event

```js
  @Output() ingredientAdded = new EventEmitter<Ingredient>();
```

```js
  onAddItem() {
    const ingName = this.nameInputRef.nativeElement.value;
    const ingAmount = this.amountInputRef.nativeElement.value;
    const newIngredient = new Ingredient(ingName, ingAmount);
    this.ingredientAdded.emit(newIngredient)
  }
```


shopping-list.component.html

```html
<app-shopping-edit
  (ingredientAdded)="onIngredientAdded($event)"
></app-shopping-edit>
```

shopping-list.component.ts

```js
  onIngredientAdded(ingredient: Ingredient) {
    this.ingredients.push(ingredient);
  }
```

learn directives now, to make drop down buttons wooork














































































