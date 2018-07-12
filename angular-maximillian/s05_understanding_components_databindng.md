# 60. Splitting apps into comopnents

```js
ng g c cockpit --spec false
```

```js
ng g c server-element --spec false
```

app.component.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
  </div>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <div
        class="panel panel-default"
        *ngFor="let element of serverElements">
        <div class="panel-heading">{{ element.name }}</div>
        <div class="panel-body">
          <p>
            <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
            <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
          </p>
        </div>
      </div>
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
  serverElements = [];
  newServerName = '';
  newServerContent = '';

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }
}
```

refactor


cockpit.component.ts

```js
<div class="row">
  <div class="col-xs-12">
    <p>Add new Servers or blueprints!</p>
    <label>Server Name</label>
    <input type="text" class="form-control" [(ngModel)]="newServerName">
    <label>Server Content</label>
    <input type="text" class="form-control" [(ngModel)]="newServerContent">
    <br>
    <button
      class="btn btn-primary"
      (click)="onAddServer()">Add Server</button>
    <button
      class="btn btn-primary"
      (click)="onAddBlueprint()">Add Server Blueprint</button>
  </div>
</div>
```

app.component.ts

```js
export class AppComponent {
  serverElements = [];
}
```


cockpit.component.ts

```js
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {

  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

}
```

server-element.component.html

```html
<div
  class="panel panel-default"
>
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <p>
      <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
      <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
    </p>
  </div>
</div>
```

app.component.html

```html
<div class="container">
  <app-cockpit></app-cockpit> 
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element *ngFor="let serverElement of serverElements"></app-server-element>
    </div>
  </div>
</div>
```

# 61. Property & Event Binding

html elements: native properties & events
directives: custom properties & events
components: custom properties & events

in server-element.component.html

we access the server, so we need it

server-element.component.ts

```js
element: {};
```

way of defining js obj !!!

```js
element: {type: string, name: string, content: string};
```

app.component.ts

we want to access show this property

```js
export class AppComponent {
  serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test!'}];
}
```

app.component.html

```html
<app-server-element 
  *ngFor="let serverElement of serverElements"
  [element]="serverElement"
>
</app-server-element>
```

element is the property in server-element.component.ts

```js
Can't bind to 'element' since it isn't a known property of 'app-server-element'.
```

!!!
by default, all properties of components are only accessible from inside the component, not from outside

you have to be explicit which properties you want to access from the outside

if you want a property, to be accessible from a parent, you need a decorator

@Input()

now we are exposing this property to the world

server-element.component.ts

```js
@Input() element: {type: string, name: string, content: string};
```

any parent, who is calling the selector, can bind

explanation!!!

in app.component.html, we are showing server-element.component.html

this server-element.component.html has

```html
<div class="panel-heading">{{ element.name }}</div>
```

so we need the element some how

in app.component.html

we pass the element in every iteration

```html
<app-server-element 
  *ngFor="let serverElement of serverElements"
  [element]="serverElement"
>
```

we bind the serverEelement, which contains the object, to the property element, which is in 

server-element.component.ts

```js
 @Input() element: {type: string, name: string, content: string};
```



so, we bind to our own properties

# 63. Assigning an alias to custom properties

sometimes, you don't want to have same name of the property in partent and in child, for example

in server-element.component.ts

```js
@Input() element: {type: string, name: string, content: string};
```

and in app.component.html

```html
<app-server-element 
  *ngFor="let serverElement of serverElements"
  [element]="serverElement"
>
</app-server-element>
```

we can pass an argument to @Input, with the proeprty name you want to have it outside this ocmponent

from the outside, you have to target the name

```html
@Input('srvElement') element: {type: string, name: string, content: string};
```

```html
<app-server-element 
  *ngFor="let serverElement of serverElements"
  [srvElement]="serverElement"
>
</app-server-element>
```


until now, we pass from parent, to child, data

now, the other direction! from child, to parent!

# 64. binding to custom events

if we have ac hild, and this child changes, 


app.component.html

```js
<p #contentParagraph>
  <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
  <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
</p>
```

we want to use that in our sever-element.component.ts

it ends up here in the end

we want to access this in our component.ts, we can not use @ViewChild, because its not part of the view, its the cntent!

thats why we have separareted hooks, contentInit, ViewInit

we also have @ContentChild

server-element.component.ts

```js
  @ContentChild('contentParagraph') paragraph: ElementRef;
```

now we can use it, because we store it in paragraph

we can not get the value, before we reach contentInit hook

```js
  ngOnInit() {
    console.log('ngOnInit called!');
    console.log("text content", this.header.nativeElement.textContent);
    console.log("text content of paragraph: " + this.paragraph.nativeElement.textContent);
  }
```

```js
  ngAfterContentInit() {
    console.log('ngAfterContentInit called!');
    console.log("text content of paragraph: " + this.paragraph.nativeElement.textContent);
  }
```

first time is empty, then its not






# assign 4

In app.component.html

<app-game-control (numberIncremented)="showNumber($event)"></app-game-control> 

I insert the component GameControl

game-control.component.html

<button class="btn btn-primary" (click)="onStartGame()">Start</button>
<button class="btn btn-danger" (click)="onStopGame()">Stop</button>
When I click Start, I call the method onStartGame

game-control.component.ts

onStartGame() {
 let emitter = this.numberIncremented;
 this.intervalInstance = setInterval(() => {
 this.emitEventTrigger(emitter);
 }, 2000)
 }
In the setInterval function, I need to create a function inside the anonymous function, so that I can pass the a paremter, the emitter.

the emitter is as follows:

@Output() numberIncremented = new EventEmitter<{num: number}>(); 

The function emitEventTrigger(emitter) increments the num and passes this num when the emitter is emitted:

emitEventTrigger(emitter) {
 this.num = this.num + 1
 emitter.emit({num: this.num});
 }


in app.component.html, i listen to the event emitter numberIncremented in the component app-game-control. It's coming from there.

<app-game-control (numberIncremented)="showNumber($event)"></app-game-control> 

now that i have listent to the event, whenever its triggered, i used the $event to get the information in the app.component.ts



num: number = 0;
 oddNums = [];
 evenNums = []
 showNumber(numObject: {num: number}) {
 this.num = numObject.num;
 if (this.num % 2 == 0) {
 this.evenNums.push(this.num);
 console.log(this.evenNums);
 } else {
 this.oddNums.push(this.num);
 console.log(this.oddNums);
 }
 console.log(numObject);
 }


I create two arrays, that i will fill in, depending on which number i got, odd or even.

in app.component.html, i will show the two arrays,

<h3>even numbers</h3>
 <app-even 
 *ngFor="let even of evenNums"
 [evenNum]="even"
 >
 </app-even>


i will use the directive ngFor, in every iteration, i have the number, so i can bind it to a property evenNum, that I created in the component even.component.ts

@Input() evenNum: number; 

i show the value of the property with string interpolation in the component

<p [ngStyle]="{backgroundColor: 'orange'}">even num is: {{ evenNum }}</p> 










































