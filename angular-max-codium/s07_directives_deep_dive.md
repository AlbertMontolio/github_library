# Understanding directives

Attribute vs Structural

## attribute directives: they sit on elements
you never destroy an element of the dom, you just change properties of the element

- look like a normal HTML Attribute (possibly with databinding or event bindning)
- only affet/ change the element they are added to

# Structural directives

structural: they also change the structure of the doma round this element

- look like a normal HTML attribute but have a leading * (for desugaring)
- affect a whole area in the DOM (elements get added/ removed)

# 83. ngFor and ngIf Recap

example:

app.component.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <button
        class="btn btn-primary"
        (click)="onlyOdd = !onlyOdd">Only show odd numbers</button>
      <br><br>
      <ul class="list-group">
        <li
          class="list-group-item">
        </li>
      </ul>
      <ng-template [ngIf]="onlyOdd">
        <p>Only odd</p>
      </ng-template>
    </div>
  </div>
</div>
```

app.component.ts

```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  numbers = [1, 2, 3, 4, 5];
  onlyOdd = false;
}
```

app.component.html

```html
<ul class="list-group">
  <li
    class="list-group-item"
    *ngFor="let number of numbers"
  >
  {{ number }}
  </li>
</ul>
```

you cant not have two structural directives in the same element

```html
<li
  class="list-group-item"
  *ngFor="let number of numbers"
  *ngIf="number % 2 == 0"
>
{{ number }}
</li>
```

```js
export class AppComponent {
  // numbers = [1, 2, 3, 4, 5];
  oddNumbers = [1, 3, 5];
  evenNumberes = [2, 4]
  onlyOdd = false;
}
```

app.component.html

```html
<ul class="list-group">
  <li
    class="list-group-item"
    *ngFor="let odd of oddNumbers"
  >
  {{ odd }}
  </li>
  <li
    class="list-group-item"
    *ngFor="let even of evenNumbers"
  >
  {{ even }}
  </li>
</ul>

but we just want to display one of the two

```html
<ul class="list-group">
  <div *ngIf="onlyOdd">
    <li
      class="list-group-item"
      *ngFor="let odd of oddNumbers"
    >
    {{ odd }}
    </li>
  </div>
  <div *ngIf="!onlyOdd">
    <li
      class="list-group-item"
      *ngFor="let even of evenNumbers"
    >
    {{ even }}
    </li>
  </div>
</ul>
````

# 84. ngClass and ngStyle Recap

app.component.css

```css
.container {
  margin-top: 30px;
}

.odd {
  color: red;
}
```

app.component.html

```html
<ul class="list-group">
  <div *ngIf="onlyOdd">
    <li
      class="list-group-item"
      [ngClass]="{odd: odd % 2 !== 0}"
      *ngFor="let odd of oddNumbers"
    >
    {{ odd }}
    </li>
  </div>
  <div *ngIf="!onlyOdd">
    <li
      class="list-group-item"
      [ngClass]="{odd: even % 2 !== 0}"
      *ngFor="let even of evenNumbers"
    >
    {{ even }}
    </li>
  </div>
</ul>
```

[] indicates, we are binding to some property in our ngClass

[ngStyle] allows us to pass a object so some property, which is also named ngStyle,on the same directive

```html
<ul class="list-group">
  <div *ngIf="onlyOdd">
    <li
      class="list-group-item"
      [ngClass]="{odd: odd % 2 !== 0}"
      [ngStyle]="{backgroundColor: odd % 2 !== 0 ? 'yellow' : 'transparent'}"
      *ngFor="let odd of oddNumbers"
    >
    {{ odd }}
    </li>
  </div>
  <div *ngIf="!onlyOdd">
    <li
      class="list-group-item"
      [ngClass]="{odd: even % 2 !== 0}"
      [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}"
      *ngFor="let even of evenNumbers"
    >
    {{ even }}
    </li>
  </div>
</ul>
```

# 85. Creating a Basic Attribute Directive

simply highlights an element when i hover over

create new folder, basic-highlight, inside app
new file, basic-highlight.directive.ts


```js
export class BasicHighlightDirective {

}
```

to make it a directive

@Directive, we need to pass an object, to configure the directive

```js
import { Directive } from "@angular/core";

@Directive({
  selector: '[appBasicHighlight]'
})
```

change the background color of the element where we attach the directive

need to get access to the element the directive sits on, we can inject the element the directive sits on, into this directive

```js
constructor() {}
```

we inject by adding the constructor, in the parenthesis, onthe list of args, we list a couple of args we want to get, whenever a instance of this class is created

if we tell angular to please give us a specific type of argument, this is what injection is, angular tries to create this ting we need and give it to us

this thing we need, is, a reference to the element, the directive was placed on

the type is iportant, has to be ElementRef (we saw it in viewchild)

to be able to use this data in our class, we can use a shortcut, add a private, it makes a property of this class, and automatically assigning the value

now we have access to the element, access the nativ element, and do something with it
we could do 


```js
export class BasicHighlightDirective {
	constructor(private elementRef: ElementRef) {
		
	}
}
```

i could use it in my constructor but better in OnInit

```js
export class BasicHighlightDirective implements OnInit {
	constructor(private elementRef: ElementRef) {}

	ngOnInit() {
		this.elementRef.nativeElement.style.backgroundColor = 'green';
	}
}
```

to use it we have to do two things

we have to inform angular, that we have a new directive

app.module.ts, in declarations

```js
import { BasicHighlightDirective } from './basic-highlight/basic-highlight.directive';

@NgModule({
  declarations: [
    AppComponent,
    BasicHighlightDirective
  ],
```


app.component.html

```html
  <p appBasicHighlight>style me with basic directive</p>
```

we dont need the []

just the name, the directive name is just the selector we set up, the [] 

```js
@Directive({
  selector: '[appBasicHighlight]'
})
```

the [] are not part of the name, they are telling angular, select attributes with this name





we can enhance this 

# 86. using the renderer to build a better attribute directive

accessing elements how we dit, its not a good practice

```js
	ngOnInit() {
		this.elementRef.nativeElement.style.backgroundColor = 'green';
	}
```

angular can render stuff without a dom, with service workers etc, thats why this approach would not work

its not agood practice to directly access your elements

lets create a new directive

```js
ng g directive better-highlight
```


in folder, better-highlight

```js
import { Directive } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective {

  constructor() { }

}
```


inject a btter tool to acces the element

renderer

```js
export class BetterHighlightDirective {
  constructor(private renderer: Renderer2) { }
}
```

with this injected, we can use it

we use it on OnInit

i can use the render here, i call the property renderer, and we have some helper methods, to work with the dom
setStyle

```js
export class BetterHighlightDirective implements OnInit {
  constructor(private renderer: Renderer2) { }

  ngOnInit() {
    this.renderer.setStyle();
  }
}
```

for that, we need to have the element for what we need to set the style

we need the element

```js
  constructor(private elRef: ElementRef, private renderer: Renderer2) { }

```

now i can use the element ref

1- which element
2- which style
3- value we want to assign for this property
4- a flag object, this is optinal
you could set, an important tag, 

```js
  ngOnInit() {
    this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue');
  }
```


app.component.html

```html
  <p appBetterHighlight>style me with basic directive</p>
```

why is this a better approach

angular is not limited to run in the browser
it also works with serviceworkers, environments where you may not have access to the dom

you may have an error

its better the render and the methods it provides

# 87. More about the renderer

Of course, you can do more than simply change the styling of an element via setStyle(). Learn more about the available Renderer methods

https://angular.io/api/core/Renderer2

# 88. Using HostListener to listen to Host Events

it always gives us a blue background

i want to just have blue if i hover

if i move my mouse away, not

lets improve the betterhighlight directive

we need to react to some events

we add a new decorator

@HostListener()

add method on we want to execute

```js
@HostListener() mouseover() {
	
}
```


mouseover is trigger whenever the argument

takes 

```js
@HostListener('mouseenter') mouseover() {
	
}
```


that is one of the events, supported by the dom element this directive sits on

this is my hostlistener, targeting this event

```js
@HostListener('mouseenter') mouseover(eventData: Event) {
	
}
```


we can get the event in our method

i want to change the background color of the element

```js
@HostListener('mouseenter') mouseover(eventData: Event) {
	this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue', false, false);
}
```

and now do the same

```js
@HostListener('mouseleave') mouseleave(eventData: Event) {
	this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'transparent', false, false);
}
```

now we should have a reactive directive


with hostlistener, reactive to any event

# 89. Using HostBinding to Bind to Host Properties

there is another decorator we can use

we would not need the render

there is a simplier way if we just want to change the background color

```js
@HostBinding
````

we pass sth

we need to bind this to some property, which value will become important

```js
@HostBinding() backgroundColor: string;
```

in hostbinding, we can pass a string, defining to which property of the hsting elmenet we want to bind

that is simply, what we access in the basic native element, for example, style
style.backgroundColor

the dom property does not dashes

```js
@HostBinding('style.backgroundColor') backgroundColor: string;
```

with this, telling angular, on the element this directive sits, please access the style, and then, a subproperty, we set equal to whatever background color we want

```js
  @HostListener('mouseenter') mouseover(eventData: Event) {
    // this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue');
    this.backgroundColor = 'blue';
  }
```

we need to set some initial color

# 90. Binding to directive properties

new functionality

we can mouse over, away, we cant decide which colors we use

the developer using this directive, should be able to change the oclor
the color is hardcoded now

we can ues

custom Property binding!

how?

add two properties to which we bind

```js
@Input() defaultColor: string = 'transparent'
```

```js
@Input() highlightColor: string = 'blue'
```

```js
  @HostBinding('style.backgroundColor') backgroundColor: string = this.defaultColor;
```

```js
  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.backgroundColor = this.highlightColor;
  }
```

```js
  @HostListener('mouseleave') mouseleave(eventData: Event) {
    this.backgroundColor = this.defaultColor;
  }
```

app.component.html


```html
<p 
  appBetterHighlight 
  [defaultColor]="'yellow'"
  [highlightColor]="'red'"
>style me with basic directive</p>
```

colors should be in ''

small bug

we need to initialze backgroundColor in ngoninit, before anything is shown

```js
  ngOnInit() {
    this.backgroundColor = this.defaultColor;
  }
```

app.compoment.html

look how we pass down the values

we have two extra directive like looking things on the paragraph, they are property binding

how does angular know, if we want to bind to a property of p, which has no defualtColor, or toa property of our directive

angular figures it out, it hecks everything

for ngClass, 

we can give an aliias

```js
  @Input('appBetterHighlight') highlightColor: string = 'blue'

```

delete

```html

[highlightColor]="'red'"
```

another thing, how we pass down dta
if we pass down a string, square brackets, and the single quotations marks

you can delete it

```html
[defaultColor]="'yellow'"
```

its equal

```html
defaultColor="yellow"
```

# 91. What happens behind the scenes on structural directives

the star indicates, its a structural directive
what angular needs to know?

its a nicer way to us to use them.. angular transforms it to sth else

there is no * in angular syntax

property binding
event binding
two way binding
string interpolation

behind the scenes, angular transforms * to one of these tools

i can write the list differently

with ng-template
we have the contact, we conditionally want to render


```html
<ng-template>
    <li
      class="list-group-item"
      [ngClass]="{odd: even % 2 !== 0}"
      [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}"
      *ngFor="let even of evenNumbers"
    >
    {{ even }}
    </li>
</ng-template>
```

wrapped in ng-template, itself its not rendered, but we can define a template to angular to use, its render when the condition is true

```html
<ng-template [ngIf]="!onlyOdd">
  <li
    class="list-group-item"
    [ngClass]="{odd: even % 2 !== 0}"
    [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}"
    *ngFor="let even of evenNumbers"
  >
  {{ even }}
  </li>
</ng-template>
```


# 92. Building a structural directive

```js
ng g d unless
```

unless.directive.ts

```js
import { Directive } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {

  constructor() { }

}
```

i need to get the condition as an input

```js
@Input()
```

i want to bind to a property named unless, whenever this condition changes, whenver some input parameter changes, i want to execute a method, i can implement a setter, this turns into a method

this still is a property, its just a setter of the property, which is. amethod, which gets executed, whenever the property changes. and it changes, whenever it changes outside

unless needs to reciive the vlaue, the property will get, it should be a boolean

```js
@Input() set unless(condition: boolean) {
  
}
```




unless directive, will sit in a ng-template, the one transformed by angular

we need to access this template, and the position where it needs to be placed, in case of true

viewcontainer, where

```js
constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef) {}
```

angular marks the place where it needs to be placced



```js
 @Input() set unless(condition: boolean) {
    if (!condition) {
      this.vcRef.createEmbeddedView();
    } 

it creates a view in this view container

the view, is our templateRef

if the condition is not 

this.vcRef.clear();

remove everything from this place in the dom

```js
export class UnlessDirective {
  @Input() set unless(condition: boolean) {
    if (!condition) {
      this.vcRef.createEmbeddedView(this.templateRef);
    } else {
      this.vcRef.clear();
    }
  }
```

in app.compoment.html

we can use it!


```html
<div *appUnless="onlyOdd">
  <li
    class="list-group-item"
    [ngClass]="{odd: even % 2 !== 0}"
    [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}"
    *ngFor="let even of evenNumbers"
  >
  {{ even }}
  </li>
</div>
```

```js
Can't bind to 'appUnless' since it isn't a known property of 'div'. ("
```

```js
*appUnless="onlyOdd">
```

is transforemd to [appUnless]=something,

so we need the same name!

```js
  @Input() set appUnless(condition: boolean) {

```


# 93. Understanding ngSwitch

app.compoment.ts
```js
value = 10;
```

app.compoment.html
```html
 <div [ngSwitch]="value"></div>
```

```html
<div [ngSwitch]="value">
  <p>Value is 5</p>
  <p>Value is 10</p>
  <p>Value is 100</p>
  <p>Value is Default</p>
</div>
```

```html
<div [ngSwitch]="value">
  <p *ngSwitchCase="5">Value is 5</p>
  <p *ngSwitchCase="10">Value is 10</p>
  <p *ngSwitchCase="100">Value is 100</p>
  <p *ngSwitchDefault>Value is Default</p>
</div>
```

lets add some directives in our recipe app

# 94. Building and Using a Dropdown directive

























    





















































