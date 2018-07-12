# The Recipe Book App (User Input, Forms and Data Management)

usage of more built-in Ionic 2 Components

handle user input

template-driven and reactive Forms approach

# 102. what we are going to build

# 103. Breaking the app into pieces (defining the app structure)

# 104. Creating the required Pages

we have the home page

we can delete the home page

we use the cli to create pages

the tabs in the bottom is a separated page

```
ionic generate page tabs
ionic generate page shopping-list
ionic generate page recipes
ionic generate page recipe
ionic generate page edit-recipe
```

add them to the app module

then set up the tabs navigation

app.module.ts

remove the home page

```js
import { MyApp } from './app.component';
import { EditRecipePage } from '../pages/edit-recipe/edit-recipe';
import { RecipePage } from '../pages/recipe/recipe';
import { RecipesPage } from '../pages/recipes/recipes';
import { ShoppingListPage } from '../pages/shopping-list/shopping-list';
import { TabsPage } from '../pages/tabs/tabs';

@NgModule({
  declarations: [
    MyApp,
    EditRecipePage,
    RecipePage,
    RecipesPage,
    ShoppingListPage,
    TabsPage
  ],
```


```js
  entryComponents: [
    MyApp,
    EditRecipePage,
    RecipePage,
    RecipesPage,
    ShoppingListPage,
    TabsPage
  ],
```


# 105. Implementing a tabs navigation

tabs.html

get rid of eveything


i want to point to the recipespage property, which i also have to create, which will hold a reference to the recipes page

```html
<ion-tabs>
  <ion-tab [root]="slPage" tabIcon="list-box" tabTitle="Shopping List"></ion-tab>
  <ion-tab [root]="recipesPage" tabIcon="book" tabTitle="Recipes"></ion-tab>
</ion-tabs>
```



add these properties i am referencing in my template in the body of this component, in the tabs.ts

```js
import { Component } from '@angular/core';
import { ShoppingListPage } from '../shopping-list/shopping-list';
import { RecipesPage } from '../recipes/recipes';

@Component({
  selector: 'page-tabs',
  templateUrl: 'tabs.html',
})
export class TabsPage {
  slPage = ShoppingListPage;
  recipesPage = RecipesPage;
}
```


finally, use this taps as the root 

app.component.ts

delete the home page

```js
rootPage:any = TabsPage;
```

```js
import { TabsPage } from '../pages/tabs/tabs';
```

colors

Primary: #bc7e40
Secondary: #7ad61d

# 106. Setting the App theme

```css
$colors: (
  primary:    #bc7e40,
  secondary:  #7ad61d,
  danger:     #f53d3d,
  light:      #f4f4f4,
  dark:       #222
);
```


# 107. Planning the Next Steps

we need some service to store our list of items

set up the form

# 108. Forms: Template-driven vs Reactive Approach

1) automatically creating the js representation of the form for you
2) reactive approach, you create the form on your own, in your code, and then you tell icon2, which elements of your html need to sincronize

# 109. Understanding Ionic 2 Controls

documentation, check box

# 110. Creating a Form Template (for Template-Driven Approach)

own user interface

shopping-list.html

ion-label, check out the docu! almost as google you can achieve



```html
<ion-content padding>
  <form>
    <ion-list>
      <ion-item>
        <ion-label fixed>Name</ion-label>
        <ion-input type="text" name="ingredientName" placeholder="Milk"></ion-input>
      </ion-item>
    </ion-list>
  </form>
</ion-content>
```

```html
<ion-content padding>
  <form>
    <ion-list>
      <ion-item>
        <ion-label fixed>Name</ion-label>
        <ion-input type="text" name="ingredientName" placeholder="Milk"></ion-input>
      </ion-item>
      <ion-item>
        <ion-label fixed>Amount</ion-label>
        <ion-input type="number" name="amount" placeholder="2"></ion-input>
      </ion-item>
    </ion-list>
    <button ion-button type="submit" block>Add Item</button>
  </form>
</ion-content>
```


next step, hook it up, extract some values when we do submit this form

# 111. Registering Controls (Template-driven)

angular 2 will create the form in the background in js for us, when it detects the form element

we have to tell angular 2 which controls our form has

```html
<ion-input 
  type="text" 
  name="ingredientName" 
  placeholder="Milk"
  ngModel
></ion-input>
```

we need to add forms module in angular 2,

ionicmodule provides this in ionic, wedont need to add forms module

make this form submittable

# 112. Submitting the Form (Template-Driven)

local reference

`#f` would access the html form

i want to have access to the form created by angular, not the html

```html
<form #f="ngForm">
```

we use the ngForm

ngSubmit directive, it has just this event, to listen to the submition of this form
and executes a method, onAddItem, and pass the form tho this method

```html
<form #f="ngForm" (ngSubmit)="onAddItem(f)">
```


shopping-list.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'page-shopping-list',
  templateUrl: 'shopping-list.html',
})
export class ShoppingListPage {

  onAddItem(form: NgForm) {
    console.log(form);
  }

}
```

# 113. Validating the form (template-driven)

both inputs have to be nonempty, disable submit btn if one is not fullfilled

shopping-list.html

add required to `<ion-input>` tag

on the button

```html
<button ion-button type="submit" block
	[disabled]="!f.valid"
>Add Item</button>
```

i can use that on the btn, add property binding, bind the disable property, i want to disable the btn if the form is not valid

# 114. Handling Data with a model for our ingredients

storing the items that were created

we need a service for this. we want to store this state, this array, in some place, which is persistanced, and we can access this from everywhere from our app

neew folder

services inside pages


a model for this ingredient class, an interface, to have a class definition

addinga. single item, removing an item, getting the array

create a model

a typescript class, which defines how our ingredients look like

in the src, folder models, file ingredient.ts

```js
export class Ingredient {
    constructor(public name: string, public amount: number) {}
    // this constructor will get this arguments when a new ingredient its created
    // we want to store this data in properties of this newly instantianted class
    // we add an accessor in front of the arguments, done
}
```

we can reuse this class to create ingredients

# 115. Managing Data with a service (shooping list service)


services/ shooping-list.ts

```js
import { Ingredient } from "../../models/ingredient";

export class ShoppingListService {
    private ingredients: Ingredient[] = [];
}
```

```js
addItem(name: string, amount: number) {
    this.ingredients.push(new Ingredient(name, amount));
}
```

add multiple items at once

es6 feature

this.ingredients.push(...items)


spread operator, ...

infront of items emans, deconstruct this array into its individual elements
we have a list of elements, not an array of elements

now we can use push

```js
addItems(items: Ingredient[]) {
    this.ingredients.push(...items);
}
```

```js
    getItems() {
        return this.ingredients;
    } 
```

this returns a reference to this array

better to show a copy

```js
getItems() {
    return this.ingredients.slice();
}
```

```js
removeItem(index: number) {
    this.ingredients.splice(index, 1);
}
```

lets hook it up with the form

# Saving items with the shopping list service

lets provide the service

app.module.ts

```js
providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    ShoppingListService
  ]
  ```

```js
import { ShoppingListService } from '../pages/services/shopping-list';
```

shopping-list.ts

access the service, inject it

```js
  constructor(
    private slService = ShoppingListService
  ) {}
  ```

```js
import { ShoppingListService } from '../services/shopping-list';
```

```js
  onAddItem(form: NgForm) {
    console.log(form.value);
    this.slService.addItem(form.value.ingredientName, form.value.amount);
    form.reset();
  }
```

int he service, addItem, `console.log(this.ingredients);`

display these items in the shopping page

# 117. Displaying Items from the Shopping List

shopping list template

shopping-list.html

```js
  <ion-list>
    <ion-item *ngFor="let item of listItems">
      <h3>{{ item.name }} ({{ item.amount }})</h3>
    </ion-item>  
  </ion-list>
```

the missing piece is listItems

```js
listItems: Ingredient[];
```

```js
import { Ingredient } from '../../models/ingredient';
```


i always load this list when the page is rendered

```js
ionViewWillEnter() {
    
}
```

```js
ionViewWillEnter() {
    this.listItems = this.slService.getItems();
  }
```

```js
export class ShoppingListPage {

  listItems: Ingredient[];

  constructor(private slService: ShoppingListService) {}

  ionViewWillEnter() {
    this.loadItems();
  }

  onAddItem(form: NgForm) {
    console.log(form.value);
    this.slService.addItem(form.value.ingredientName, form.value.amount);
    form.reset();
    this.loadItems();
  }

  private loadItems() {
    this.listItems = this.slService.getItems();
  }
}
```

we have to load the items, when we added from this page, and when we enter the page, maybe we add some items in another page

remove items while clicking on them

# 118. removing item from the shopping list

shopping-list.html

```html
<ion-item *ngFor="let item of listItems; let i = index" (click)="onCheckItem(i)">
```

shopping-list.ts

```js
  onCheckItem(index: number) {
    this.slService.removeItem(index);
    this.loadItems();
  }
```

you can add swipe effect

recipes, a complex form

# 119. Adding a new recipe button and page transition

i want to add a button to the navbar, which creates a recipe

recipes.html

```html
  <ion-navbar>
    <ion-buttons end>
      <button ion-button icon-only (click)="onNewRecipe()">
        <ion-icon name="add"></ion-icon>
      </button>
    </ion-buttons>
    <ion-title>Recipes</ion-title>
  </ion-navbar>
```

recipes.ts

onnewrecipe, we want to navigate to the edit recipe page

```js
import { Component } from '@angular/core';

@Component({
  selector: 'page-recipes',
  templateUrl: 'recipes.html',
})
export class RecipesPage {

  onNewRecipe() {

  }

}
```


```js
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-recipes',
  templateUrl: 'recipes.html',
})
export class RecipesPage {

  constructor (private navCtrl: NavController) {}

  onNewRecipe() {

  }

}
```

i can now access it, and push a new page on the stack of pages, just in the tab that we are, the recipes tab


```js
onNewRecipe() {
    this.navCtrl.push(EditRecipePage);
  }
```

```js
import { EditRecipePage } from '../edit-recipe/edit-recipe';

```

pass a parameter, New, because we will use the page for a new recipe, and for edit a recipe

```js
onNewRecipe() {
    this.navCtrl.push(EditRecipePage, {mode: 'New'});
  }
```


# 120. Creating a Edit Recipe Form (Reactive Approach)

edit-recipe.js

retrieving the data that we are passing, this mode argument

retriving the params!

```js
import { Component } from '@angular/core';
import { NavParams } from 'ionic-angular';

@Component({
  selector: 'page-edit-recipe',
  templateUrl: 'edit-recipe.html',
})

export class EditRecipePage implements OnInit {
  
  constructor(private navParams: NavParams) {}

}
```

to retrieve it, i could do it in the constructor, by i do it in the ngoninit method

initialization tasks good idea in the ngoninit

```js
import { Component, OnInit } from '@angular/core';
import { NavParams } from 'ionic-angular';

@Component({
  selector: 'page-edit-recipe',
  templateUrl: 'edit-recipe.html',
})

export class EditRecipePage implements OnInit {
  
  constructor(private navParams: NavParams) {}

  ngOnInit() {

  }

}
```

store the mode in a property


```js
  mode = 'New';
  constructor(private navParams: NavParams) {}

  ngOnInit() {
    this.mode = this.navParams.get('mode');
  }
```

edit-recipe.html

```html
<ion-header>

  <ion-navbar>
    <ion-title>{{ mode }} Recipe</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>
  <form>
    <ion-list>
      <ion-item>
        <ion-label floating>Title</ion-label>
        <ion-input type="text"></ion-input>
      </ion-item>
      <ion-item>
        <ion-label floating>Description</ion-label>
        <ion-textarea></ion-textarea>
      </ion-item>

      <ion-item>
        <ion-label floating>Difficulty</ion-label>
      </ion-item>

    </ion-list>
  </form>
</ion-content>
```


docu, dropdown, is a select item

```html
<ion-item>
  <ion-label floating>Difficulty</ion-label>
  <ion-select>
    <ion-option></ion-option>
  </ion-select>
</ion-item>
```

i could store it by hand

edit-recipe.ts

```js
export class EditRecipePage implements OnInit {
  mode = 'New';
  selectOptions = ['Easy', 'Medium', 'Hard'];
  ```

edit-recipe.html

```html
<ion-select>
  <ion-option *ngFor="let option of selectOptions"
    [value]="option"
  >

  </ion-option>
</ion-select>
```

i can bind to the value property of that ion-option, [value]
i do bind the option that i extract from the array

this is important for the background, that angular knows which value is store there

```html
<ion-select>
  <ion-option *ngFor="let option of selectOptions"
    [value]="option"
  >
    {{ option }}
  </ion-option>
</ion-select>
```


# 121. Creating the Reactive Form

edit-recipe.html

```html
			</ion-list>
    <button type="submit" ion-button block>{{ mode }} Recipe</button>
  </form>
```

edit-recipe.ts

```js
  private initializeForm() {
    
  }
  ```

store the form in a property of this class

```js
  recipeFrom: FormGroup;
  ```


```js
import { FormGroup } from '@angular/forms';

```

we initialize it manually

```js

  private initializeForm() {
    this.recipeForm = new FormGroup({
      
    });
  }
  ```

we have three controls

```js
  private initializeForm() {
    this.recipeForm = new FormGroup({
      'title': new FormControl(),
      'description': new FormControl(),
      'difficulty': new FormControl()
    });
  }
  ```

i want to add default values and validators

we dont need default, pass null

```js

  private initializeForm() {
    this.recipeForm = new FormGroup({
      'title': new FormControl(null, Validators.required),
      'description': new FormControl(null, Validators.required),
      'difficulty': new FormControl('Medium', Validators.required)
    });
  }
  ```

the form is created in the ts code

we need to hook it up, syncronize with the html code

# 122. syncing form and html (reactive approach)

in html

i have to inform angular, that i havae my own form, not that angular creates it for me

edit-recipe.html

```html
<ion-content padding>
  <form [formGroup]="recipeForm">
```

i pass my form

the form is syncronized, now, tell

which input referes to which control


```html
<ion-label floating>Title</ion-label>
<ion-input type="text"
  formControlName="title"
></ion-input>
```

title, same name as in the controllers

```html
<ion-textarea formControlName="description"></ion-textarea>
```

```html
<ion-select formControllName="difficulty">
```

to see something


```html
  <form [formGroup]="recipeForm" (ngSubmit)="onSubmit()">

  ```

the method does not exist

edit-recipe.ts

i dont need to pass the form as an argument, its already in the property

not working, i am never calling the initializeForm!

```js
ngOnInit() {
    this.mode = this.navParams.data('mode');
    this.initializeForm();
  }
```

# 123. Add Ingredients Management to the Form

we dont have ingredients for our recipe

edit-recipe.html

i want to output my ingredients

```html
<button type="button" clear ion-button block 
  (click)="onManageIngredients()"
>Manage Ingredients</button>
<ion-list>
  <ion-item>
    <ion-label>Name</ion-label>
    <ion-input type="text"></ion-input>
  </ion-item>
</ion-list>
<button type="submit" ion-button block>{{ mode }} Recipe</button>
```

# 124. Creating an Action Sheet

```js
  onManageIngredients() {
    
  }
  ```

i want to open an action sheet -> docu

options sliding up from the button

we need to insert a certain controller

```js
  constructor(private navParams: NavParams,
    private actionSheetController: ActionSheetController
  ) {}
```

```js
import { NavParams, ActionSheetController } from 'ionic-angular';
```

```js
  onManageIngredients() {
    const actionSheet = this.actionSheetController.create();
  }
  ```

we pass an object

```js
  onManageIngredients() {
    const actionSheet = this.actionSheetController.create({
      title: 'what do youw ant to do?',
      buttons: [
        {
          text: 'Add Ingredient',
          handler: () => {

          }
        },
        {
          text: 'Remove all Ingredients',
          role: 'destructive',
          handler: () => {

          }
        },
        {
          text: 'Cancel',
          role: 'cancel'
        }
      ]
    });
  }
  ```

# 125. Creating a Dialog (Alert) with an input Field

```js
buttons: [
        {
          text: 'Add Ingredient',
          handler: () => {
            // show a dialog that allows the user enter the name of the ingredient
            
          }
        },
        ```

we use an alert, we need to inject the alert controller

```js
  constructor(private navParams: NavParams,
    private actionSheetController: ActionSheetController,
    private alertCtrl: AlertController
  ) {}        
  ```

create this alert, outsource this in a new method

we want it in the handler

```js
private createNewIngredientAlert() {
  const newIngredientAlert = this.alertCtrl.create({
    title: 'Add Ingredient',
    inputs: [
      {
        name: 'name',
        placeholder: 'Name'
      }
    ],
    buttons: [
      {
        text: 'Cancel',
        role: 'cancel'
      },
      {
        text: 'Add',
        handler: data => {
          if (data.name.trim() == '' || data.name == null) {
            // to do
          }
        }
      }
    ]
  })
}
```

# 126. Managing Ingredient Controls in a FormArray

i need to add ingredient form array to my form

initialize form method, add a forth control

it will store an array of ingredients

FormArray

edit-recipe.ts

```js
  private initializeForm() {
    console.log("hello world");
    this.recipeForm = new FormGroup({
      'title': new FormControl(null, Validators.required),
      'description': new FormControl(null, Validators.required),
      'difficulty': new FormControl('Medium', Validators.required),
      'ingredients': new FormArray([])
    });
  }
  ```

we need to syncronize it with our template, this ingredients array

```html
<ion-list>
  <ion-item 
    *ngFor="let igControl of recipeForm.get('ingredients').controls; let i = index"
  >
    <ion-label>Name</ion-label>
    <ion-input type="text"></ion-input>
  </ion-item>
</ion-list>
```


we have a parent element that we synchronize with our formArray

```html
<ion-list formArrayName="ingredients">
  <ion-item 
    *ngFor="let igControl of recipeForm.get('ingredients').controls; let i = index"
  >
    <ion-label>Name</ion-label>
    <ion-input type="text"></ion-input>
  </ion-item>
</ion-list>
```

individual elements, represent the controls


syncronize input with the control im currently at in this loop

formarray is synchronized with my template

# 127. Wiring it all up

when we have valid data

in `private createNewIngredientAlert()`

```js
buttons: [
        {
          text: 'Cancel',
          role: 'cancel'
        },
        {
          text: 'Add',
          handler: data => {
            if (data.name.trim() == '' || data.name == null) {
              return;
            }
            // tell it to angular, that this is a formarry
            (<FormArray>this.recipeForm.get('ingredients')).push(new FormControl(data.name, Validators.required))
          }
        }
      ]
      ```


i want to present it

```js
return newIngredientAlert;
```

or delete the storing const, and return directly

in `onManageIngredients()`

```js
buttons: [
        {
          text: 'Add Ingredient',
          handler: () => {
            this.createNewIngredientAlert().present();
          }
        },
```


on `onManageIngredients()` we have to

```js
actionSheet.present();
```

# 128. Removing Ingredient Controls

retrieve this formArray


in `onManageIngredients` 

in buttons

```js
{
  text: 'Remove all Ingredients',
  role: 'destructive',
  handler: () => {
    const fArray: FormArray = this.recipeForm.get('ingredients');
  }
},
```

typescript does not know that we are getting a formarray

```js
handler: () => {
  const fArray: FormArray = <FormArray>this.recipeForm.get('ingredients');
}
```

```js
{
  text: 'Remove all Ingredients',
  role: 'destructive',
  handler: () => {
    const fArray: FormArray = <FormArray>this.recipeForm.get('ingredients');
    const len = fArray.length;
    if (len > 0) {
      for (let i = len - 1; i >= 0; i--) {
        fArray.removeAt(i);
      }
    }
  }
},
```

if name is empty, we want to tell to the user

toast: little helper messages that dissapear.


# 129. Using Toasts to Present Short Messages

docu, toast

add a toast if you try to add an ingredient that is empty

we need to inject a new controller

edit-recipe.ts

```js
  constructor(private navParams: NavParams,
    private actionSheetController: ActionSheetController,
    private alertCtrl: AlertController,
    private toastCtrl: ToastController
  ) {}
  ```


```js
import { NavParams, ActionSheetController, AlertController, ToastController } from 'ionic-angular';
```

in `createNewIngredientAlert`

```js
handler: data => {
if (data.name.trim() == '' || data.name == null) {
  const toast = this.toastCtrl.create({
    message: 'Please enter a valid value!',
    duration: 1000,
    position: 'bottom'
  });
  toast.present();
  return;
}
```

```js
(<FormArray>this.recipeForm.get('ingredients')).push(new FormControl(data.name, Validators.required));
const toast = this.toastCtrl.create({
  message: 'Item added!',
  duration: 1000,
  position: 'bottom'
});
toast.present();
```


onManageINgredients

```js
handler: () => {
  const fArray: FormArray = <FormArray>this.recipeForm.get('ingredients');
  const len = fArray.length;
  if (len > 0) {
    for (let i = len - 1; i >= 0; i--) {
      fArray.removeAt(i);
    }
    const toast = this.toastCtrl.create({
      message: 'All Ingredients were deleted!',
      duration: 1000,
      position: 'bottom'
    });
    toast.present();
  }
}
```

let's add the recipe in our service!

# 130. Adding a Recipe Model and Service

edit-recipe.html

```html
<button type="submit" ion-button block
      [disabled]="!recipeForm.valid"
    >{{ mode }} Recipe</button>
```

if its valid, well trigger onSubmit

edit-recipe.ts

```js
  onSubmit() {
    console.log(this.recipeForm);
  }
  ```

we need a new service, recipe service, which stores the recipes

we want a list of methods. get recipes, remove them.

we need a model for a recipe


new file recipe.ts

```js
import { Ingredient } from "./ingredient";

export class Recipes {
    constructor(
        public title: string, 
        public description: string, 
        public difficulty: string,
        public ingredients: Ingredient[] 
    ) {}
}
```

services / recipes.ts

```js
import { Recipe } from "../models/recipe";
import { Ingredient } from "../models/ingredient";

export class RecipesService {
    private recipes: Recipe[] = [];

    addRecipe(  title: string, 
                description: string, 
                difficulty: string, 
                ingredients: Ingredient[]
    ) {
        this.recipes.push(new Recipe(title, description, difficulty, ingredients));
    }

    getRecipes() {
        return this.recipes.slice();
    }

    updateRecipe(index: number, 
        title: string,
        description: string,
        difficulty: string,
        ingredients: Ingredient[]
    ) {
        this.recipes[index] = new Recipe(title, description, difficulty, ingredients);
    }

    removeRecipe(index: number) {
        this.recipes.splice(index, 1);
    }

}


```

app.module.ts

```js
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
    ShoppingListService,
    RecipesService
  ]
```


```js
import { RecipesService } from '../services/recipes';
```

# 131. Adding Recipes through a Service

edit-recipe.ts

we want to store in the service, in the onSubmit

we need our service

```js
  constructor(private navParams: NavParams,
    private actionSheetController: ActionSheetController,
    private alertCtrl: AlertController,
    private toastCtrl: ToastController,
    private recipesService: RecipesService
  ) {}
  ```

```js
  onSubmit() {
    console.log(this.recipeForm);
    const value = this.recipeForm.value;
    this.recipesService.addRecipe(value.tite, value.description, value.difficulty, value.ingredients);
  }
```


in the ingredients list, we just have the name, not the amount

our ingredient has an amount also

```js
  onSubmit() {
    console.log(this.recipeForm);
    const value = this.recipeForm.value;
    let ingredients = [];
    if (value.ingredients.length > 0) {
      ingredients = value.ingredients.map(name => {
        return {name: name, amount: 1}
      });
    }
    this.recipesService.addRecipe(value.tite, value.description, value.difficulty, value.ingredients);
  }
  ```

```js
  onSubmit() {
    console.log(this.recipeForm);
    const value = this.recipeForm.value;
    let ingredients = [];
    if (value.ingredients.length > 0) {
      ingredients = value.ingredients.map(name => {
        return {name: name, amount: 1}
      });
    }
    this.recipesService.addRecipe(value.tite, value.description, value.difficulty, ingredients);
    this.recipeForm.reset();
    // i need the navCtrl
    this.navCtrl.popToRoot();
  }
  ```

dont forget

```js
  private navCtrl: NavController
```

services/recipes.ts

in addRecipe

```js
console.log(this.recipes);
```

# 132. Outputting Recipes

recipes.html

```html
<ion-content padding>
  <ion-list>
    <button 
      ion-item 
      *ngFor="let recipe of recipes; let i = index"
      (click)="onLoadRecipe()"
    >
    <h2>{{ recipe.title }}</h2>
    <ion-note>{{ recipe.difficulty }}</ion-note>
    </button>
  </ion-list>
</ion-content>
```

we need to populate the recipes

and add the onLoadRecipe method


recipes.ts

```js
recipes: Recipe[];
```

we want to initialize this

```js
constructor (private navCtrl: NavController,
    private recipesService: RecipesService
  ) {}
```

```js
  ionViewWillEnter() {
    this.recipes = this.recipesService.getRecipes();
  }
```


# 133. Displaying Recipe Details - Template


recipe.html

```html
<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col>
        <h2>{{ recipe.title }}</h2>
        <div >{{ recipe.difficulty }}</div>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col>
        <p>{{ recipe.description }}</p>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col>
        <ion-list>
          <ion-item *nfFor="let ingredient of recipe.ingredients">
            {{ ingredient.name }}
          </ion-item>
        </ion-list>
      </ion-col>
    </ion-row>
    <ion-row *ngIf="recipe.ingredients.length > 0">
      <ion-col>
        <button 
          ion-button 
          clear 
          (click)="onAddIngredients()"
        >Add Ingredients to Shopping List</button>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col>
        <button 
          ion-button 
          outline 
          (click)="onEditRecipe()"
        >Edit Recipe</button>
      </ion-col>
      <ion-col>
        <button 
          ion-button
          outline
          block
          (click)="onDeleteRecipe()"
          color="danger"
        >delete Recipe</button>
      </ion-col>
    </ion-row>
  </ion-grid>
</ion-content>
```




```html
<div class="subtitle">{{ recipe.difficulty }}</div>
```

recipe.css

```css
page-recipe {
    .subtitle {
        text-transform: uppercase;
        color: #ccc;
    }
}
```

let's load our recipe

we get it from params

# 135. loading the recipe detail page (and passing data to it)

we are passing it, in onLoadRecipe

in recipes.ts

we need the navCtrl, we have it already

```js
 onLoadRecipe() {
    this.navCtrl.push(RecipePage);
  }
  ```

this loads the page, but we need to pass some data

we need the index, the updateRecipe in the service, needs it


recipes.html

```html
<button 
  ion-item 
  *ngFor="let recipe of recipes; let i = index"
  (click)="onLoadRecipe(recipe, i)"
>
```

```js
  onLoadRecipe(recipe: Recipe, index: number) {
    this.navCtrl.push(RecipePage, {
      recipe: recipe, 
      index: index
    });
  }
```


now we retrieve the data in recipe.ts, with navparams

```js
import { Component } from '@angular/core';
import { NavController, NavParams } from 'ionic-angular';

@Component({
  selector: 'page-recipe',
  templateUrl: 'recipe.html',
})
export class RecipePage {

    constructor(public navCtrl: NavController, public navParams: NavParams) {}

}
```

OnInit, its executed before the template is rendered

```js
import { Component, OnInit } from '@angular/core';
import { NavController, NavParams } from 'ionic-angular';
import { Recipe } from '../../models/recipe';

@Component({
  selector: 'page-recipe',
  templateUrl: 'recipe.html',
})
export class RecipePage implements OnInit {
  recipe: Recipe;
  index: number;

  constructor(public navCtrl: NavController, public navParams: NavParams) {}

  ngOnInit() {
    this.recipe = this.navParams.get('recipe');
    this.index = this.navParams.get('index');
  }

}

```

add text-center to the columns that you want


recipe.title
recipe.description
button add ingredients to shopping list

now edit recipe, in edit

recipe.ts

```js
  onEditRecipe() {
    this.navCtrl.push(EditRecipePage, {
      mode: 'Edit',
      recipe: this.recipe, 
      index: this.index
    });
  }
```

edit-recipe.ts

we have to initialize the form differently depending on the mode

```js
recipe: Recipe;
index: number;
```

```js
  ngOnInit() {
    this.mode = this.navParams.get('mode');
    if (this.mode == 'Edit') {
      this.recipe = this.navParams.get('recipe');
      this.index = this.navParams.get('index');
    }
    this.initializeForm();
  }
```

the initializeForm

```js

  private initializeForm() {
    console.log("hello world");
    this.recipeForm = new FormGroup({
      'title': new FormControl(null, Validators.required),
      'description': new FormControl(null, Validators.required),
      'difficulty': new FormControl('Medium', Validators.required),
      'ingredients': new FormArray([])
    });
  }
  ```

we dont want null anymore


```js
  private initializeForm() {
    let title = null;
    let description = null;
    let difficulty = 'Medium';
    let ingredients = []

    this.recipeForm = new FormGroup({
      'title': new FormControl(title, Validators.required),
      'description': new FormControl(description, Validators.required),
      'difficulty': new FormControl(difficulty, Validators.required),
      'ingredients': new FormArray(ingredients)
    });
  }
  ```

```js
  private initializeForm() {
    let title = null;
    let description = null;
    let difficulty = 'Medium';
    let ingredients = []

    if (this.mode == 'Edit') {
      title = this.recipe.title;
      description = this.recipe.description;
      difficulty = this.recipe.difficulty;
      // ingredients = this.recipe.ingredients;
      // FormArray expects FormControls
      for (let ingredient of this.recipe.ingredients) {
        ingredients.push(new FormControl(ingredient.name, Validators.required))
      }
    }

    this.recipeForm = new FormGroup({
      'title': new FormControl(title, Validators.required),
      'description': new FormControl(description, Validators.required),
      'difficulty': new FormControl(difficulty, Validators.required),
      'ingredients': new FormArray(ingredients)
    });
  }
  ```

# 138. Updating the Recipe through a Service

we need to save our changes

edit-recipe.ts

```js
  onSubmit() {
    console.log(this.recipeForm);
    const value = this.recipeForm.value;
    let ingredients = [];
    if (value.ingredients.length > 0) {
      ingredients = value.ingredients.map(name => {
        return {name: name, amount: 1}
      });
    }
    if (this.mode == 'Edit') {
      this.recipesService.updateRecipe(this.index, value.title, value.description, value.difficulty, ingredients);
    } else {
      this.recipesService.addRecipe(value.title, value.description, value.difficulty, ingredients);
    }
    
    this.recipeForm.reset();
    // i need the navCtrl
    this.navCtrl.popToRoot();
  }
```

# 139. Sending Ingredients to the Shopping List

services/shopping-list.ts

add multiple ingredients to our shopping list


recipe.html

i have the onAddIngredients()

recipe.ts

```js
constructor(
  public navCtrl: NavController, public navParams: NavParams,
  private slService: ShoppingListService
) {}
```

```js

  onAddIngredients() {
    this.slService.addItems(this.recipe.ingredients);
  }
```

you have to clickthe fucking button, store all ingredients

now delete

```js
  constructor(
    public navCtrl: NavController, public navParams: NavParams,
    private slService: ShoppingListService,
    private recipesService: RecipesService
  ) {}
  ```


```js
  onDeleteRecipe() {
    this.recipesService.removeRecipe(this.index);
  }
```


if we click delete, that we go back automatically
not to stay in the page

```js
onDeleteRecipe() {
    this.recipesService.removeRecipe(this.index);
    this.navCtrl.popToRoot();
  }
  ```


our data is not persistant!

how can we store it on a server

authenticate user!



























































































































