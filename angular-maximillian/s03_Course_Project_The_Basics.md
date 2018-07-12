Planning the app

root
header

shopping list
shopping list
shopping list edit

recipe book
recipe list
recipe item
recipe detail

recipes

Model: Ingredient

Model: Recipe

# 43. Creating the components

app.component.html

```html
<app-header></app-header>
<div class="container">
  <div class="row">
    <div class="col-md-12">
      I'm working
    </div>
  </div>
</div>
```

```js
ng g c recipes --spec false
```

avoid creating spec file

```js
.
├── app.component.css
├── app.component.html
├── app.component.spec.ts
├── app.component.ts
├── app.module.ts
├── header
│   ├── header.component.html
│   └── header.component.ts
├── recipes
│   ├── recipe-detail
│   │   ├── recipe-detail.component.css
│   │   ├── recipe-detail.component.html
│   │   └── recipe-detail.component.ts
│   ├── recipe-list
│   │   ├── recipe-item
│   │   │   ├── recipe-item.component.css
│   │   │   ├── recipe-item.component.html
│   │   │   └── recipe-item.component.ts
│   │   ├── recipe-list.component.css
│   │   ├── recipe-list.component.html
│   │   └── recipe-list.component.ts
│   ├── recipes.component.css
│   ├── recipes.component.html
│   └── recipes.component.ts
└── shopping-list
    ├── shopping-edit
    │   ├── shopping-edit.component.css
    │   ├── shopping-edit.component.html
    │   └── shopping-edit.component.ts
    ├── shopping-list.component.css
    ├── shopping-list.component.html
    └── shopping-list.component.ts
```

# 45. Adding a Navigation bar

```html
<nav class="navbar navar-default">
    <div class="container-fluid">
        <div class="navbar-header">
            <a href="#" class="navbar-brand">Recipe Book</a>
        </div>

        <!-- <div class="collapse navbar-collapse"> -->
        <div class="navbar-default">
            <ul class="nav navbar-nav">
                <li><a href="#">Recipes</a></li>
                <li><a href="#">Shopping List</a></li>
            </ul>
            <ul class="nav navbar-nav navbar-right">
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" role="button">Manage <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Save Data</a></li>
                        <li><a href="#">Fetch Data</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

recipe-list.component.list
```js
recipes = [];
```

we need to define a recipe, we do a model

in recipes!!! folder we create 

recipe.model.ts

```js
export class Recipe {
  public name: string;
  public description: string;
  public imagePath: string;

  // when we call new, the constructor is called!
  constructor(name: string, desc: string, imagePath: string) {
    this.name = name;
    this.description = desc;
    this.imagePath = imagePath;
  }
}
```

# 48. Adding content to the recipes components

recipe-list.component.ts

```js
recipes: Recipe[] = [];
```

an array of recipes! the type

```js
recipes: Recipe[] = [
  new Recipe()
];
```

this calls the constructor

```js
recipes: Recipe[] = [
  new Recipe("A test recipe", "This is simply a test", "https://www.seriouseats.com/recipes/images/2017/06/20170627-summer-nachos-matt-clifton-1-1500x1125.jpg")
];

```

recipe-list.component.html

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

```html
src="{{ recipe.imagePath }}"  // string interpolation
[src]="recipe.imagePath" // property binding
```

recipe-detail.component.html

```html
import { Component, OnInit } from '@angular/core';

@Component({
  selector: '[app-servers]',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = "No server was created!";
  serverName = '';
  serverCreated = false;
  servers = ['Testserver', 'Testserver 2']

  constructor() { 
    console.log("name: ", name);
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }

  ngOnInit() {
  }

  onCreateServer() {
    this.serverCreated = true;
    this.servers.push(this.serverName);
    this.serverCreationStatus = "server was created! name is " + this.serverName;
  }

  onUpdateServerName(event: Event) {
    console.log(event);
    this.serverName = (<HTMLInputElement>event.target).value;
  }
}
```



cross component comunication

shopping-list.component.html

```html
<div class="row">
  <div class="col-xs-10">
    <app-shopping-edit></app-shopping-edit>
    <hr>
    <ul class="list-group">
      <a href="#" class="list-group-item" style="cursor: pointer"></a>
    </ul>
  </div>
</div>
```

shopping-list.component.ts

```js
ingredients = [];
```

app/shared

ingredient.model.ts

```js
export class Ingredient {
	public name: string;
	public amount: number;

	constructor(name: string, amount: number) {
		this.name = name;
		this.amount = amount;
	}
}
```

thats so typical, there is a shortcut

```js
export class Ingredient {
	constructor(public name: string, public amount: number) {
	}
}
```

this makes the same, create the instance, and create the properties, and assign the values that we recieve while creating

# 53. creating and outputing the shopping list


shopping-list.component.ts
```js
ingredients: Ingredient[] = [];
```

```js
ingredients: Ingredient[] = [
  new Ingredient('Apples', 5),
  new Ingredient('Tomatoes', 10),
];
```

shopping-list.component.html

```html
<ul class="list-group">
  <a 
    href="#" 
    class="list-group-item" 
    style="cursor: pointer"
    *ngFor="let ingredient of ingredients"
  >
    {{ ingredient.name }} ({{ ingredient.amount}})
  </a>
</ul>
```

# 54. Adding a shopping list edit section

shoping-edit.component.html

```html
<div class="row">
  <div class="col-xs-12">
    <form action="">
      <div class="row">
        <div class="col-sm-5 form-group">
          <label for="name">Name</label>
          <input type="text" id="name" class="form-control">
        </div>
        <div class="col-sm-2 form-group">
          <label for="amount">Amount</label>
          <input type="number" id="amount" class="form-control">
        </div>
      </div>
      <div class="row">
        <div class="col-xs-12">
          <button class="btn btn-success" type="submmit">Add</button>
          <button class="btn btn-danger" type="button">Delete</button>
          <button class="btn btn-clear" type="button">Primary</button>
        </div>
      </div>
    </form>
  </div>
</div>
```




# 57. Debugging code in the browser using sourcemaps

chrome, sources, the scripts

main.bundle.js its not easy to debug

sourcemaps, translates ts to js

we can put break pauses

under webpack, you can find your scripts
dot folder, app

# 58. Using Augury to dive into angular apps

added to chrome

you can see your application


























































