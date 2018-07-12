buttons

list

the inonic 2 grid system

gestures

# 79. Another look at the components doc

ionicframework.com/docs

components section

API

if you have a component, at the beginning, API doc

difference between api and component doc

api has too much info

# 80. Using and styling buttons

buttonssssssssss

pages/home
home.html

```html
<ion-content padding>
  <button (click)="onClick()">A normal button</button>
</ion-content>
```

home.js

```js
  onClick() {
    console.log('clicked!');
  }
```

the button is ugly

we use a directive

we turn the normal html butotn element, into a ionic2 element

now we have a pretty button

```html
<ion-content padding>
  <button (click)="onClick()"
  ion-button
  color="secondary"
  >A normal button</button>
</ion-content>
```

outline: we just see the outline, no color in the background
clear: no border
round: rounded corners

position -->
block: it makes bigger.
it will turn this button from inline element to block eleemnt, it gets added to the flow of elements in the html element, dom
will be treated like a normal block element, like a div

no longer inline, like the inline elements

!!!css difference between in-line and block

what if you want the button to all the size of the page

full instead of block


```html
<button (click)="onClick()"
  ion-button
  full
  >Another button</button>
```

sizes of buttons

small, large

inside of the button
now we have a text

you can add more than text inside

```html
<button (click)="onClick()"
  ion-button
  small
  block
  >
    <ion-icon name="home"></ion-icon>
  </button>
```

icon-only: informs ionic that we have icons. it looks nicer

combine icon and text

you can configure where the icon sits

get reid of icon-only

define where the icon is

icon-left

# 81. Understanding lists

list and list items!

data, or buttons, 

```html
  <ion-list>
    <ion-item>A normal item</ion-item>
    <button ion-item>A button item</button>
  </ion-list>
```

button has the arrow, and when we click, we notice it

ion-item does not have these things

ion-list, you dont need it. the style is the same, but we remove the separators at the start and end of the list

the list gives us other features

# 82. Understanding list items and their content

elements inside list items

```html
<ion-item>
    <ion-icon name="star"></ion-icon> 
  A normal item
</ion-item>
```

you want the star to be more on the left


```html
<ion-icon name="star" item-left></ion-icon>
```


```html
<ion-item>
  <ion-icon name="star" item-left></ion-icon> 
  <p>A normal item</p>
</ion-item>
```

```html
<ion-item>
  <ion-icon name="star" item-right></ion-icon>
  <h1>The heading of the item</h1>
  <p>A normal item</p>
</ion-item>
```

put an avatar! read the documentation


# 83. Configuring lists

change the list looks like

remove lines inbetween the items

```html
<ion-list no-lines>
```

inset: more margins



# 84. Item Groups and List Headers

you want to have some categories in your list

ionic expects to have some groups

```html
  <ion-item-group>
    <ion-item-divider>A</ion-item-divider>
    <ion-item>Apples</ion-item>
    <ion-item>Avocados</ion-item>
    <ion-item-divider>B</ion-item-divider>
    <ion-item>Bananas</ion-item>
    <ion-item>Berries</ion-item>
  </ion-item-group>
```

different way to divers

you might want to have a header

```html
  <ion-list>
      <ion-list-header>Privacy</ion-list-header>
    <ion-item>Tracking</ion-item>
    <ion-item>Personal Images</ion-item>
  </ion-list>
```




The `<ion-list>`  element has a big advantage: It allows you to easily create a re-orderable list. Here are the steps/ the code to add this functionality.

```html
// list.html in /pages folder
<ion-list reorder="true" (ionItemReorder)="reorderItems($event)">
    <ion-item *ngFor="let item of items">{‌{ item }}</ion-item>
</ion-list>

```

```js
 
// list.ts in /pages folder
import { Component } from '@angular/core';
import { NavController, reorderArray } from 'ionic-angular';
 
@Component({
    selector: 'page-list',
    templateUrl: 'list.html'
})
export class ListPage {
    items = ['Apples', 'Bananas', 'Berries'];
 
    reorderItems(indexes){
        this.items = reorderArray(this.items, indexes);
    }
}
```
This is all!

You need to set reorder  to true  (on `<ion-list>` ) and add a listener to the ionItemReorder  event. In the method executed on this event, you should simply use the reorderArray  method (which is provided by ionic-angular ) to reorder your array.

!!! not working

# 86. Ionic 3.0.0 and the Grid

The next lecture will explore the Ionic Grid - here's something you should watch out for!

If you're using Ionic 3.0.0 instead of 2.x (check your package.json to find out), you need to update some parts of the grid (the properties mainly): http://blog.ionic.io/build-awesome-desktop-apps-with-ionics-new-responsive-grid/

Specifically, you should change width-*  to col-*  and also adjust to a 12 column grid. So width-50  becomes col-6  (50% of 12 = 6).

The same for offset-* . That also switches to 12 columns, so offset-50  becomes offset-6 .




# 87. Understanding the Grid System

its a very flexible system, you can use it anywhere in your template
even inside

based on flexbox

you can add width

width-10

```
  <ion-grid>
    <ion-row>
      <ion-col>A column</ion-col>
    </ion-row>
  </ion-grid>
```

i dont see the column

home.scss

```css
page-home {
    ion-col {
        border: 1px dashed #ccc;
    }
}
```

same amount of space, the two columns

```html
<ion-grid>
  <ion-row>
    <ion-col>A column</ion-col>
    <ion-col>A column</ion-col>
  </ion-row>
</ion-grid>
```

like in bootstrap

```html
<ion-grid>
  <ion-row>
    <ion-col col-4>A column</ion-col>
    <ion-col>A second column</ion-col>
  </ion-row>
</ion-grid>
```

we can offset columns!


```html
  <ion-grid>
    <ion-row>
      <ion-col col-4>A column</ion-col>
      <ion-col>A second column</ion-col>
    </ion-row>
    <ion-row>
        <ion-col offset-4>A new column</ion-col>
    </ion-row>
  </ion-grid>
```


# 88. More than click - using gestures

```html
  <div class="gestures">
    
  </div>
```

```css

    .gestures {
        width: 100%;
        height: 30px;
        background-color: lightgreen;
    }
```




```html
  <div class="gestures" (click)="onElementClicked()">
    Click me
  </div>
```

we can long press, we can swipe it, etc. different gestures that we can executed

component, gestures, learn more in the docu

tap, press, pan, swipe, rotate and pinch events

they are event listeners

```html
  <div class="gestures" (tap)="onElementTapped()">
    Tap me
  </div>
  ```

```js
  onElementTapped() {
    console.log("i was tapped");
  }
```

```html
  <div class="gestures" (press)="onElementPressed()">
    Long press me
  </div>
```

```js
  onElementPressed() {
    console.log("i was long pressed");
  }
```

two actions in a button. both are fired

```html
  <div class="gestures"
    (press)="onElementPressed()"
    (click)="onElementClicked()"
    >
    Long press me
  </div>
```

click and tab

click will always be fired while you touch an element

diffferentiante between long andshort, you have to use tap


```html
  <div class="gestures"
    (press)="onElementPressed()"
    (tap)="onElementTapped()"
    >
    Long press me
  </div>
```



# 89. Creating and using Custom Components

we are building an angular application

we can create our own components!

in src, folder components

touch-event.component.ts

```js
import { Component } from "@angular/core";

// component decorator
@Component({
    selector: 'app-touch-event',
    template: `
    <div class="gestures" (click)="onElementClicked()">
        Click me
    </div>

    <div class="gestures" (tap)="onElementTapped()">
        Tap me
    </div>

    <div class="gestures"
        (press)="onElementPressed()"
        (tap)="onElementTapped()"
        >
        Long press me
    </div>
    `
})

export class TouchEventComponent {

}
```

remove the gestures tag from home.html

remove methods gesture from home.ts (just onClick bleibt)

and put these methods in our component

```js
export class TouchEventComponent {
    onElementClicked() {
        console.log("i was clicked or touched");
    }

    onElementTapped() {
    console.log("i was tapped");
    }

    onElementPressed() {
    console.log("i was long pressed");
    }
}
```

app.module.ts

```js
import { TouchEventComponent } from '../components/touch-event.component';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    TouchEventComponent
  ],
```

we dont need entryComponents

home.html

```html
<ion-content padding>
  <app-touch-event></app-touch-event>
```


we are using a normal component in a normal angular application






# SOLUTION

```html
<ion-header>
  <ion-navbar>
    <ion-title center>
      Assigment 2
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col text-center>
        <h3>Enter the right Code</h3>
        <p>Tap twice, press four times</p>
      </ion-col>
    </ion-row>

    <ion-row>
      <ion-col text-center>
        <ion-list>
          <ion-item>
            <p>Tapped: {{ tapped }}</p>
          </ion-item>
          <ion-item>
            <p>Pressed: {{ pressed }}</p>
          </ion-item>
        </ion-list>
      </ion-col>
    </ion-row>

  </ion-grid>
</ion-content>
```

home.ts

```js
import { Component } from '@angular/core';
import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {
  tapped = 0;
  pressed = 0;
  constructor(public navCtrl: NavController) {

  }

}
```



outsource the buttons

components/reset.component.ts

```js
import { Component } from "@angular/core";

@Component({
    selector: 'app-reset',
    template: `
        <ion-row>
            <ion-col>
                <button ion-button color="danger" small block
                (click)="onReset('all')"
                >
                    Reset all
                </button>
            </ion-col>
            <ion-col>
                <button ion-button color="danger" small block
                (click)="onReset('tap')"
                >
                    Reset taps
                </button>
            </ion-col>
            <ion-col>
                <button ion-button color="danger" small block
                (click)="onReset('press')"
                >
                    Reset presses
                </button>
            </ion-col>
        </ion-row
    `
})

export class ResetComponent {

}
```

we are passing a string on the method onReset

we need to create the method onReset


```js
onReset(type: string) {
	
}
```

i want to emit this event, i cant handle it here (in the click event from reset.component.ts). 

i want to reset the tapped, pressed properties in the home page, not in the reset component

i somehow need to get my event from the resetcomponent, to the homepage

lets use our selector, and add it to our home page


home.html

```html
<app-reset></app-reset>
```

we always need to add components to app.module

app.module.ts

```js
import { ResetComponent } from '../components/reset.component';

@NgModule({
  declarations: [
    MyApp,
    HomePage,
    ResetComponent
  ],
```

we see the three buttons




i want to emit the event, everytime i click onReset, that this event reaches the home page

i will add my own custome event to the resetcomponent

didReset = new EventEmitter

it will emit with text

reset.component.ts

```js
import { Component, EventEmitter } from "@angular/core";
```

```js
export class ResetComponent {
    didReset = new EventEmitter<string>();
    onReset(type: string) {
        // to be done
    }
}
```

now i can emit, didReset


```js

export class ResetComponent {
    @Output() didReset = new EventEmitter<string>();
    onReset(type: string) {
        this.didReset.emit(type);
    }
}
```

i emit the type, tab, press all

i am emitting this, passing this on, to anyone who is listening

in order to enable the parent component to listent to this event

i need to add a decorator to didReset

@Output, tells angular2, this property is an event, you should be able to listen to it from a parents component prespective





```js
import { Component, EventEmitter, Output } from "@angular/core";
```

```js
export class ResetComponent {
    @Output() didReset = new EventEmitter<string>();
    onReset(type: string) {
        this.didReset.emit(type);
    }
}
```



in home.html i can now listen to didReset, the event i just created.

```html
    <app-reset (didReset)="onDidReset($event)" ></app-reset>
```

now we need to imlement this method in our home page


i will now handle the event emitted by clicking one of my reset buttons

i know that i am passing a string. its the argument of emit

i can extract this by the $event, it gives the data of the event

we store it in resetType

home.ts


```js
  onDidReset(resetType: string) {
    switch (resetType) {
      case 'tap':
        this.tapped = 0;
        break;
      case 'press':
        this.pressed = 0;
        break;
      default:
        this.tapped = 0;
        this.pressed = 0;
    }
  }
```

listeners for tap and press event

home.html

```html
<ion-row>
  <ion-col style="background-color: lightgreen" text-center
  (tap)="onTap()"
  >
    Tap here
  </ion-col>
  <ion-col style="background-color: lightgreen" text-center
  (press)="onPress()"
  >
    Press here
  </ion-col>
</ion-row>
```

home.ts

```js
  onTap() {
    console.log('tapped');
    this.tapped++;
  }

  onPress() {
    console.log('pressed');
    this.pressed++;
  }
```


you won text

```js
  didwin() {
    return this.tapped == 2 && this.pressed == 4;
  }
```

```html
<h3>Enter the right Code</h3>
	<p *ngIf="!didWin()">Tap twice, press four times</p>
	<p *ngIf="didWin()">You won</p>
```
        


































































