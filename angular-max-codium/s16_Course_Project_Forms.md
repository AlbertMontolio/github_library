# Section 16: Course Project - Forms

# 204. Introduction

lets add forms

two places, when we create a new recipe or edit 

and in the shopping list

lets start with the shopping list

we will use the TD

# 205. TD: Adding the Shopping List Form

in the shopping-edit.component.html

get rid of thelocal references

get rid of the click listener of the button
type="submit"

```html
<form (ngSubmit)="onAddItem(f)" #f="ngForm">
```

we should rgister some controls

```html
<input 
  type="text" 
  id="name" 
  class="form-control"
  ngModel
  name="name"
>
```

ngModel, so that we register it as a control in our td form

```js
<input 
  type="number" 
  id="amount" 
  class="form-control"
  ngModel
  name="amount"
>
```

in shopping-edit.component.ts

remove

```js
@ViewChild('nameInput') nameInputRef: ElementRef;
@ViewChild('amountInput') amountInputRef: ElementRef;
```

```js
onAddItem(form: NgForm) {
  const value = form.value;
  const newIngredient = new Ingredient(value.name, value.amount);
  this.slService.addIngredient(newIngredient);
}
```

we can add some validations

# 206. Adding Validation to the form

i want to add validation, 

in name, 
required, same for our number

disabled the add button if the form is invalid

in the submit btn

```html
[disabled]="!f.valid"
```

a negative amount works

validator, the pattern validator

this checks the user input against a regular expresion

in the amount input

```html
[pattern]="'^[1-9]+[0-9]*$'"
```

since its a string we can omit the single quotation marks and the []

lets make sure

# 207. Allowing the selection of items in the list

time to make the items editable by clicking them

shopping-list.component.html

where we list them with a ngForm

i add a click listener to the item, so that we can click it

```html
<ul class="list-group">
  <a 
    href="#" 
    class="list-group-item" 
    style="cursor: pointer"
    *ngFor="let ingredient of ingredients"
    (click)="onEditItem()"
  >
    {{ ingredient.name }} ({{ ingredient.amount}})
  </a>
</ul>
```

i also want to get the id of the ingredient

i need the id, so later i can replace the correct one in the array of ingredients

which is managed in a service

```html
*ngFor="let ingredient of ingredients; let i = index"
(click)="onEditItem(i)"
```

shopping-list.component.html

```js
onEditItem(index: number) {
  
}
```

we want to pass the index to the edit-component

i will use a service, a subject in the service, to which i can listen to in the shopping-edit component

add a new service in the shopping-list.esrvice.ts

```js
export class ShoppingListService implements OnInit {

	ingredientsChanged = new Subject<Ingredient[]>();
	startedEditing = new Subject<number>();
```

shopping-list.component.ts

with this subject created, i can reach out to the slService

emit a new value, the index

```js
onEditItem(index: number) {
  this.shoppingListService.startedEditing.next(index);
}
```

we want to listen to that in some other place

that other place, the shopping edit component

in ngoninit do the listening

we need to inject the slservice

we subscribe to the startedEditing subject

shopping-edit.component.ts

```js
ngOnInit() {
  this.slService.startedEditing
    .subscribe();
}
```

we need to unsubscribe 

we store it in a property, 

```js
export class ShoppingEditComponent implements OnInit {
  subscription: Subscription;
```

```js
ngOnInit() {
  this.subscription = this.slService.startedEditing
    .subscribe();
}
```

```js
export class ShoppingEditComponent implements OnInit, OnDestroy {
```

```js
ngOnDestroy() {
  this.subscription.unsubscribe();
}
```

to avoid memory leaks

what should happpen in the subscription?

we will recive the number of the item we are about to edit
this will be triggered whenever we start the startedEditing

```js
ngOnInit() {
  this.subscription = this.slService.startedEditing
    .subscribe(
      (index: number) => {
        
      }
    );
}
```

should we create a new item or edit an existing one?

i want to store the mode we are, in a property

```js
editMode = false;
```

```js
.subscribe(
  (index: number) => {
    this.editMode = true;
  }
);
```

with this, we got the index and we are editing

i also want to store the index on the item we are editing

```js
editedItemIndex: number;
```

it will get initialize 

```js
.subscribe(
  (index: number) => {
    this.editedItemIndex = index;
    this.editMode = true;
  }
);
```

with that info, i want to load the item we are about to edit

# 208. Loading the shopping list items into the form

i want to get the item we want to edit

so, in the shopping-list.service.ts

i need a new method

```js
getIngredient(index: number) {
	return this.ingredients[index];
}
```

shopping-edit.component.ts

we can reach out to this method

and store it in the 

```js
editedItem: Ingredient
```

we do this, whenever we get a new information we are in the edit mode

```js
ngOnInit() {
  this.subscription = this.slService.startedEditing
    .subscribe(
      (index: number) => {
        this.editedItemIndex = index;
        this.editMode = true;
        this.editedItem = this.slService.getIngredient(index);
      }
    );
}
```

now we need to update our form, if we are in edit mode

we want to have access to the form

in the form, we have a local reference, f

```html
<form (ngSubmit)="onAddItem(f)" #f="ngForm">
```

we can use @ViewChild to get access to it

```js
export class ShoppingEditComponent implements OnInit, OnDestroy {
  @ViewChild('f') slForm: NgForm;
```

inside of the callback, i want to reach out to my form, and call set value

assign a new value, for the name, editedItem

```js

ngOnInit() {
  this.subscription = this.slService.startedEditing
    .subscribe(
      (index: number) => {
        this.editedItemIndex = index;
        this.editMode = true;
        this.editedItem = this.slService.getIngredient(index);
        this.slForm.setValue({
          name: this.editedItem.name,
          amount: this.editedItem.amount
        })
      }
    );
}
```

a delete the href in the item

now, the button add is misleading, it should be update

# 209. Updating existing items

we loaded the item, if we are in the edit mode, make sure we dont add an existing one, we just edit the selected one

it starts in the template with the button name

it will only be add, if we are adding an item

```html
<button 
  class="btn btn-success" 
  type="submit"
  [disabled]="!f.valid"
>Add</button>
```

```html
<button 
  class="btn btn-success" 
  type="submit"
  [disabled]="!f.valid"
>
  {{ editMode ? 'Update' : 'Add' }}
</button>
```

i also want to change the way we change the this

in shopping-edit.component.ts

```js
onAddItem(form: NgForm) {
  const value = form.value;
  const newIngredient = new Ingredient(value.name, value.amount);
  this.slService.addIngredient(newIngredient);
}
```

we always call addIngredient, if we want to update an existing one

in shopping-list.service.ts

```js
updateIngredient(index: number, newIngredient: Ingredient) {
	this.ingredients[index] = newIngredient;
	this.ingredientsChanged.next(this.ingredients.slice());
}
```

!!! why the next? subject. teh shopping-list component, where we have the ingredients, that we list, is of course subscribe to this subject

i am updating the ingredient

shopping-edit.component.ts

call this method in the onAddItem

```js
onAddItem(form: NgForm) {
  const value = form.value;
  const newIngredient = new Ingredient(value.name, value.amount);
  if (this.editMode) {
    this.slService.updateIngredient(this.editedItemIndex, newIngredient);
  } else {
    this.slService.addIngredient(newIngredient);
  }
}
```

we wnat to reset the form after updating or creating an item

# 210. Resetting the Form


after we save our updated value or created


```js
onAddItem(form: NgForm) {
  const value = form.value;
  const newIngredient = new Ingredient(value.name, value.amount);
  if (this.editMode) {
    this.slService.updateIngredient(this.editedItemIndex, newIngredient);
  } else {
    this.slService.addIngredient(newIngredient);
  }
  form.reset();
}
```

with this approach, we never leave the edit mode

initialie, we are in edit mode set to false, then we switch to true, we never switch it back

this ensures we leave edit mode because we are done

```js

onAddItem(form: NgForm) {
  const value = form.value;
  const newIngredient = new Ingredient(value.name, value.amount);
  if (this.editMode) {
    this.slService.updateIngredient(this.editedItemIndex, newIngredient);
  } else {
    this.slService.addIngredient(newIngredient);
  }
  this.editMode = false;
  form.reset();
}
```

we could rename onAddItem to onSubmit




lets work on the clear btn next

# 211. Allowing the User to Clear (Cancel) the Form

clear btn is similar to that

shopping-edit.component.html

```html
<button 
  class="btn btn-primary" 
  type="button"
  (click)="onClear()"
>Clear</button>
```

shopping-edit.component.ts

```js
onClear() {
  this.slForm.reset();
  this.editMode = false;
}
```

# 212. Allowing the deletion of shopping list items

make sure that we delete items we select

shopping-edit.component.html

```html
<button 
  class="btn btn-danger" 
  type="button"
  (click)="onDelete()"
>Delete</button>
```

shopping-edit.component.ts

```js
onDelete() {
  
}
```

inform the service that he has to remove an ingredient in the array

we also need to call, onClear, to clear the form

shopping-list.service.ts

```js
deleteIngredient(index: number) {
	this.ingredients.splice(index, 1);
	this.ingredientsChanged.next(this.ingredients.slice());
}
```

shopping-edit.component.ts

```js
onDelete() {
  this.slService.deleteIngredient(this.editedItemIndex);
  this.onClear();
}
```

this throws an error if we have not selected an ingredient

therefore, ngIf to the btn, only display the btn, if we are in edit mode

shopping-edit.component.html

```html
<button 
  class="btn btn-danger" 
  type="button"
  *ngIf="editMode"
  (click)="onDelete()"
>Delete</button>
```

next section, recipe edit form, with reactive approach

# 213. Creating the Template for the (Reactive) Recipe Edit Form



recipe-edit.component.html

```html
<p>
  recipe-edit works!
</p>
```

lets add a form

save and cancel btn, at the start

now, the inputs


```html
<div class="row">
  <div class="col-xs-12">
    <form>
      <div class="row">
        <div class="col-xs-12">
          <button type="submit" class="btn btn-success">Save</button>
          <button type="button" class="btn btn-danger">Cancel</button>
        </div>
      </div>
      <div class="row">
        <div class="col-xs-12">
          <div class="form-group">
            <label for="name">Name</label>
            <input 
              type="text" 
              id="name"
              class="form-control"
            >
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-xs-12">
          <div class="form-group">
            <label for="imagePath">Image URL</label>
            <input 
              type="text" 
              id="imagePath"
              class="form-control"
            >
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-xs-12">
          <img src="" class="img-responsive">
        </div>
      </div>

      <div class="row">
        <div class="col-xs-12">
          <div class="form-group">
            <label for="description">Description</label>
            <textarea 
              type="text" 
              id="description"
              class="form-control"
              rows="6"
            ></textarea>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-xs-12">
          <div class="row">
            <div class="col-xs-8">
              <input 
                type="text" 
                class="form-control"
              >
            </div>
            <div class="col-xs-2">
              <input 
                type="number" 
                class="form-control"
              >
            </div>
            <div class="col-xs-2">
              <button class="btn btn-danger">x</button>
            </div>
          </div>
            
        </div>

      </div>

    </form>
  </div>
</div>

```

# 214. Creating the Form For Editing Recipes

recipe-edit.component.ts

lets initialize our form

its important to know if we are in edit more or in new mode

new private method, initForm, responsible for initializing the form

the form itself, should be a property, type FormGroup

```js
recipeForm: FormGroup;
```

```js
private initForm() {

}
```

```js
private initForm() {
  let recipeName = "";

  if (this.editMode) {
    recipeName = this.
  }

  this.recipeForm = new FormGroup({
    'name': new FormControl()
  });
}
```

we have the id, we can fetch the recipe

```js
export class RecipeEditComponent implements OnInit {
  id: number;
  editMode = false;
  recipeForm: FormGroup;
```

inject recipe service

```js
constructor(
  private route: ActivatedRoute,
  private recipeService: RecipeService
) { }
```

now, we can fetch our recipe, the recipe we are currently editing

```js
private initForm() {
  let recipeName = "";

  if (this.editMode) {
    const recipe = this.recipeService.getRecipe(this.id);
    recipeName = recipe.name
  }

  this.recipeForm = new FormGroup({
    'name': new FormControl()
  });
}
```

```js
private initForm() {
  let recipeName = "";
  let recipeImagePath = '';
  let recipeDescription = '';
```

```js
private initForm() {
  let recipeName = "";
  let recipeImagePath = '';
  let recipeDescription = '';

  if (this.editMode) {
    const recipe = this.recipeService.getRecipe(this.id);
    recipeName = recipe.name;
    recipeImagePath = recipe.imagePath;
    recipeDescription = recipe.description;
  }

  this.recipeForm = new FormGroup({
    'name': new FormControl(recipeName)
  });
}
```

recipeName to the FormControl, it will be an empty string, or if we are in edit mode, it will have the name of the recipe

```js
private initForm() {
  let recipeName = "";
  let recipeImagePath = '';
  let recipeDescription = '';

  if (this.editMode) {
    const recipe = this.recipeService.getRecipe(this.id);
    recipeName = recipe.name;
    recipeImagePath = recipe.imagePath;
    recipeDescription = recipe.description;
  }

  this.recipeForm = new FormGroup({
    'name': new FormControl(recipeName),
    'imagePath': new FormControl(recipeImagePath),
    'description': new FormControl(recipeDescription)
  });
}
```

when should we call initForm ?

we should call it, whenever our route params change

that indicates that we reloaded the page

```js
ngOnInit() {
  this.route.params
    .subscribe(
      (params: Params) => {
        this.id = +params['id'];
        this.editMode = params['id'] != null;
        this.initForm();
        console.log(this.editMode);
      }
    );
}
```

we are creating the form, we need to syn to our html code

# 215. Syncing HTML with the Form

first of all, app.module.ts

we need the ReactiveFormsModule



```js
imports: [
  BrowserModule,
  AppRoutingModule,
  FormsModule,
  ReactiveFormsModule
],
```

recipe-edit.component.html

```html
<form [formGroup]="recipeForm">
```

angular should take our form, not its

```html
<input 
  type="text" 
  id="name"
  formControlName="name"
  class="form-control"
>
```

```html
<input 
  type="text" 
  id="imagePath"
  formControlName="imagePath"
  class="form-control"
>
```

```html
<textarea 
  type="text" 
  id="description"
  class="form-control"
  rows="6"
  formControlName="description"
></textarea>
```

```html
<form [formGroup]="recipeForm" (ngSubmit)="onSubmit()">
```

recipe-edit.component.ts

```js
onSubmit() {
  console.log(this.recipeForm);
}
```

next step, get ouringredients to work correctly

we need to manage the arrayof ingredients for that

# 216. Adding Ingredient Controls to a Form Array

array of ingredients

recipe-edit.component.ts

when we initialize the form

recipeIngredients = new FormArray

with a default value of an empty array

```js
private initForm() {
  let recipeName = "";
  let recipeImagePath = '';
  let recipeDescription = '';
  let recipeIngredients = new FormArray([]);
```

if we are in edit mode, check if we have ingredients

check if my recipe that i loaded, if it does have ingredients. if that is defined, i can use them, i want to use all the ingredients, loop through them

i can push them on my recipe ingredients form array

i dont push the ingredient, i push a new form group instead

```js
if (this.editMode) {
  const recipe = this.recipeService.getRecipe(this.id);
  recipeName = recipe.name;
  recipeImagePath = recipe.imagePath;
  recipeDescription = recipe.description;

  if (recipe['ingredients']) {
    for (let ingredient of recipe.ingredients) {
      recipeIngredients.push(
        new FormGroup({
          'name': new FormControl(ingredient.name),
          'amount': new FormControl(ingredient.amount)
        })
      );
    }
  }
}
```

i have two controls, which will control the ingredient, the name and the amount

with that, we can assign the ingredients control 

```js
this.recipeForm = new FormGroup({
  'name': new FormControl(recipeName),
  'imagePath': new FormControl(recipeImagePath),
  'description': new FormControl(recipeDescription),
  'ingredients': recipeIngredients
});
```

we need to sync our array with the html code

each control will be the row
we will have formgroups, as each pair of amount name control

```html
<div 
  class="row" 
  *ngFor="let ingredientCtrl of recipeForm.get('ingredients').controls; let i = index"
  formGroupName="i"
>
```

this should give me the form group

now we need to populate this inputs

we are in formgroup, we have name and amount

with property binding

we want to add now dynamically new ingredients



# 217. Adding new ingredient controls

add a button that allows us to add a new ingredient


iwant some spacing between the errors

```html
<div 
  class="row" 
  *ngFor="let ingredientCtrl of recipeForm.get('ingredients').controls; let i = index"
  [formGroupName]="i"
  style="margin-top: 10px;"
>
```

new button at the end of the array area

```html
<div class="row">
  <div class="col-xs-12">
    <button class="btn btn-success" (click)="onAddIngredient()">Add Ingredient</button>
  </div>
</div>
```

recipe-edit.component.ts

```js
onAddIngredient() {
  this.recipeForm.get('ingredients')
}
```

we know that this ingredients is a FormArray, but ts does not know it

explicitely cost it, with the cost command

enclosing all in parehtesis. now this is treated as a form array, i can call push, i want to push, a new form group
i will have a name, which is a new form control


```js
onAddIngredient() {
  (<FormArray>this.recipeForm.get('ingredients')).push(
    new FormGroup({
      'name': new FormControl(),
      'amount': new FormControl()
    })
  )
}
```

i also submit the form with each new line

fix: make sure that this button recieves the type button

before we make the form submitable, lets add some validation to it

# 218. Validating User Input

recipe-edit.component.ts

add validation in our typescript code

validation attach to our control

when we initialize the form, at the end of initForm

for the name

Validators.required

dont eexecute it, just pass a reference, so that angular executes the method at the time it validates the input


```js
this.recipeForm = new FormGroup({
  'name': new FormControl(recipeName, Validators.required),
  'imagePath': new FormControl(recipeImagePath, Validators.required),
  'description': new FormControl(recipeDescription, Validators.required),
  'ingredients': recipeIngredients
});
```




for the ingredients, when we create a new ingredient

```js
if (recipe['ingredients']) {
  for (let ingredient of recipe.ingredients) {
    recipeIngredients.push(
      new FormGroup({
        'name': new FormControl(ingredient.name, Validators.required),
        'amount': new FormControl(ingredient.amount, [
          Validators.required,
        Validators.pattern(/^[1-9]+[0-9]*$/)
        ])
      })
    );
  }
}
```

lets also copy that to the onAddIngredient method, we also create a formgroup

```js
onAddIngredient() {
  (<FormArray>this.recipeForm.get('ingredients')).push(
    new FormGroup({
      'name': new FormControl(null, Validators.required),
      'amount': new FormControl(null, [
        Validators.required,
      Validators.pattern(/^[1-9]+[0-9]*$/)
      ])
    })
  )
}
```

to use these validations, disabled the save btn if the form is not valid

```html
<button 
  type="submit" 
  class="btn btn-success"
  [disabled]="!recipeForm.valid"
>Save</button>
```

to finish the validation, mark invalid input swith css

```css
.ng-invalid {
	border :1px solid red
}
```


the form is also bordered


```css
input.ng-invalid, textarea.ng-invalid {
	border :1px solid red
}
```

to prevent being red from the start

```html
input.ng-invalid.ng-touched, 
textarea.ng-invalid.ng-touched {
	border :1px solid red
}
```

lets submit the form and save it, or successful update a recipe

# 219. Submitting the Recipe Edit Form

```js
onSubmit() {
  console.log(this.recipeForm);
}
```

i wnt to create a new one in my array of recipes, or update one

in my recipe.service.ts

```js
addRecipe(recipe: Recipe) {
	this.recipes.push(recipe);
}

updateRecipe(index: number, newRecipe: Recipe) {
	this.recipes[index] = newRecipe;
}
```

recipe-edit.component.ts

```js
onSubmit() {
  const newRecipe = new Recipe(
    this.recipeForm.value['name'], 
    this.recipeForm.value['description'],
    this.recipeForm.value['imagePath'],
    this.recipeForm.value['ingredients']
  );
  if (this.editMode) {
    this.recipeService.updateRecipe(this.id, newRecipe)
  } else {
    this.recipeService.addRecipe(newRecipe);
  }
}
```

the object has a valid format already

```js
onSubmit() {
  // const newRecipe = new Recipe(
  //   this.recipeForm.value['name'], 
  //   this.recipeForm.value['description'],
  //   this.recipeForm.value['imagePath'],
  //   this.recipeForm.value['ingredients']
  // );
  if (this.editMode) {
    this.recipeService.updateRecipe(this.id, this.recipeForm.value)
  } else {
    this.recipeService.addRecipe(this.recipeForm.value);
  }
}
```

saving the data

nothing happens, is not working

we dont get an error

we had this problem earlier

recipe.service.ts

we add a new recipe, but when we get a list of recipes

we rturn a copy

```js
getRecipes() {
	return this.recipes.slice();
}
```

therefore, is not the same array, we are using in our component, the one we are updating is not the one used in our components

same approach

recipes.service.ts

```js
recipesChanged = new Subject<Recipe[]>();
```


now when we call add recipe, i simple use the recipesChange evenet, and emit a new value, this value is a new copy of our recipes

```js
addRecipe(recipe: Recipe) {
	this.recipes.push(recipe);
	this.recipesChanged.next(this.recipes.slice());
}
```

also for update

```js
updateRecipe(index: number, newRecipe: Recipe) {
	this.recipes[index] = newRecipe;
	this.recipesChanged.next(this.recipes.slice());
}
```

recipe-list.component.ts

where we get our recipes, and in ngOnInit

i want to listen to this event

subscribe to the event, and if it changed, i know i recieve an array of recipes

```js
ngOnInit() {
  this.recipeService.recipesChanged
    .subscribe(
      (recipes: Recipe[]) => {
        this.recipes = recipes;
      }
    );
  this.recipes = this.recipeService.getRecipes();
}
```

recipe form working

# 220. Adding a Delete and Clear (Cancel) Functionality

when hitting cancel

cancel, and the delete button, to delete our recipe

i want to start with the delete button

recipe.service.ts

```js
deleteRecipe(index: number) {
	this.recipes.splice(index, 1);
	this.recipesChanged.next(this.recipes.slice());
}
```

recipe-detail.component.html

```html
<ul class="dropdown-menu">
  <li><a style="cursor: pointer" (click)="onAddToShoppingList()">To Shopping List</a></li>
  <li><a (click)="onEditRecipe()" style="cursor: pointer">Edit Recipe</a></li>
  <li><a style="cursor: pointer" (click)="onDeleteRecipe()">Delete Recipe</a></li>
</ul>
```

recipe-detail.component.ts

```js
onDeleteRecipe() {
  this.recipeService.deleteRecipe(this.id);
}
```

when we hit cancel


recipe-edit.component.html

```html
<button type="button" class="btn btn-danger" (click)="onCancel()">Cancel</button>
```

recipe-edit.component.ts

onCancel i just want to navigate away

```js
constructor(
  private route: ActivatedRoute,
  private recipeService: RecipeService,
  private router: Router
) { }
```

i want to navigate in oncancel, one level up. if we are editing, to the detail page, if it s anew, to the recipes page

```js
onCancel() {
  this.router.navigate(['../']);
}
```

for this to work

we need to tell angular, what our current route is
the activated route

the second parameter, the relativeto this route


```js
onCancel() {
  this.router.navigate(['../'], { relativeTo: this.route });
}
```

i want the same to happen once i submit the form

```js
onSubmit() {
  if (this.editMode) {
    this.recipeService.updateRecipe(this.id, this.recipeForm.value)
  } else {
    this.recipeService.addRecipe(this.recipeForm.value);
  }
  this.onCancel();
}
```

it looks bad, but just want to navigate away.maybe change the name

everything works, except

can we delete the recipe?

it is deleted from the list, but we still see the details

# 221. Redirecting the User (After deleting a recipe)

the delete btn is not working as expected. we delete the recipe, but we stay in

http://localhost:4200/recipes/2

to change this, 

recipe-detail.component.ts

onDeleteRecipe, we just want to navigate away

```js
onDeleteRecipe() {
  this.recipeService.deleteRecipe(this.id);
  this.router.navigate(['/recipes']);
}
```

this now works, but one thing is not working

the image, we are not getting a preview of the image!

lets fix this

# 222. Adding an Image preview

```html
<div class="row">
  <div class="col-xs-12">
    <img src="" class="img-responsive">
  </div>
</div>
```

we are not binding the source!

add local reference, named imagePath, to that input imagePath

```html
<input 
  type="text" 
  id="imagePath"
  formControlName="imagePath"
  class="form-control"
  #imagePath
>
```

```js
<div class="row">
  <div class="col-xs-12">
    <img [src]="imagePath.value" class="img-responsive">
  </div>
</div>
```

the image is working

we can crate a recipe, update, we see it

but if we go back to recipes, the new created recipe is gone

# 223. Providing the recipe service correctly

we created a recipe, if we go to recips, its gone

we provide our recipe services, in the recipes component

all the components in this area, share the same instantce

but if we navigate away to the shopping list area, the recipe component is destroy and so is the instance of the service

recipes.component.ts

```js
@Component({
  selector: 'app-recipes',
  templateUrl: './recipes.component.html',
  styleUrls: ['./recipes.component.css'],
  providers: [RecipeService]
})
```

to ensure that our service survives, we need to add it in to our app.module.ts

```js
@Component({
  selector: 'app-recipes',
  templateUrl: './recipes.component.html',
  styleUrls: ['./recipes.component.css'],
})
```

app.module.ts

```js
providers: [ShoppingListService, RecipeService],
```

now we have one instance of the service, all the time available, all the time our app is running

we still have a bug.

when we click the x button of the ingredient, we are not deleting the ingredient

# 224. Deleting Ingredients and some finishing touches

the delete btn is not hooked up, no method, and its even submitting our form



```html
<button 
  class="btn btn-danger"
>x</button>
```

to

```html
<button 
  class="btn btn-danger"
  type="button"
  (click)="onDeleteIngredient()"
>x</button>
```

i should pass the information, which ingredient should be deleted

i should pass the index i, the index of the control we want to remove

```js
<div class="col-xs-2">
  <button 
    class="btn btn-danger"
    type="button"
    (click)="onDeleteIngredient(i)"
  >x</button>
</div>
```

now, lets impement the method

recipe-edit.component.ts

```js
onDeleteIngredient(index: number) {
  (<FormArray>this.recipeForm.get('ingredients')).removeAt(index);
}
```

now, we remove the ingredients


to clean this up

recipe-list.component.ts

where we subscribe to the recipesChanged


we should unsubscribe! if we destroy the component

```js
export class RecipeListComponent implements OnInit, OnDestroy {

```

```js
subscription: Subscription;
```

```js
ngOnInit() {
    this.subscription = this.recipeService.recipesChanged
```

```js
ngOnDestroy() {
  this.subscription.unsubscribe();
}
```

make sure we dont cost any memory leaks

our app should work fine




























































































