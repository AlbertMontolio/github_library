# S01: Introduction

We use Angular to build front end Application. They are very reactive, that means, everything happens in the "same" page.

What is Angular?

Angular is a Javascript framework which allows you to create reactive Single-Page-Applications (SPAs).

It's just one HTML file, we change the url for the user, but we don't actually change the page.

The experience is as mobile applications.

The JS changes the DOM during run time.

In order to understand the Angular versioning, Angular 2 was completed rewrite from Angular 1.

Angular 1 was the first framewrok to make 1 single app

Angular 2 or higher is called just Angular

From Angular 2 on, the versions are just small incremental improvements.

# CH02: The Command Line Interface (CLI)

Just to get started it, depending on the CLI version you're using, you might also need to add the `FormsModule`  to the `imports[]`  array in your `app.module.ts`  file (add it if you don't see it there). You might not fully understand what that all means but we're going to cover that in this course, no worries.

If you don't have FormsModule  in imports[]  in AppModule , please do add it and also add an import at the top of that file:

```js
import { FormsModule } from '@angular/forms';
```

If you want to dive deeper into the CLI and learn more about its usage, have a look at its official documentation:

https://github.com/angular/angular-cli/wiki

You encountered issues during the installation of the CLI or setup of a new Angular project?

A lot of problems are solved by making sure you're using the latest version of NodeJS, npm and the CLI itself.

Updating NodeJS:

Go to nodejs.org and download the latest version - uninstall (all) installed versions on your machine first.

Updating npm:

Run [sudo]

```js
npm install -g npm  (sudo  is only required on Mac/ Linux)
```

my machine:

```js
+ npm@6.1.0
```

Updating the CLI

```js
[sudo] npm uninstall -g angular-cli @angular/cli
```

```js
npm cache clean
```

```js
[sudo] npm install -g @angular/cli
```
in my machine:

```js
+ @angular/cli@6.0.8
```

```js
ng new my-dream-app --style=scss
ng serve
```

to be able to use scss

--style=scss

```js
ng new my-first-app
```

```js
ng serve
```


Everyting starts in the index.html

Then, app component is inserted in app-root component

# CH03 Two way binding

app.module.ts

```js
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
```

NgModules configure the injector and the compiler and help organize related things together.

```html
app.component.html

<input type="text" [(ngModel)] ="name">
<p>{{ name }}</p>
```

```js
export class AppComponent {
  name = 'Albert';
}
```

# Course Structure

the basics, components & databinding, directives, services & dependency injection, routing, observables, forms, pipes, http, authentication, ptimizations & ngModules, deployment, animations & testing

# 9. Typescript

more features than vanilla JS, types, classes, interfaces

then is compiled to javascript

# Bootstrap

```js
npm install --save bootstrap@3
```

this installs locally

```js
+ bootstrap@3.3.7
```


make aware that we installed it

angular.json

architecture, build, styles

```js
"styles": [
  "node_modules/bootstrap/dist/css/bootstrap.min.css",
  "src/styles.css"
],
```

run again

```js
ng serve
```

in the head section, we should the two imports

# course source


# 13 how and angular app gets loaded and started

the index.html is the file served by the server

app-root

its a component, the app component

app.component.ts

selector: app-root, replace the html file app-root for the template of the app component

how is angular triggered?

in index.html

ng serve creates the scripts at the end

main.ts, is the first code its executed

the last line is the important,

```js
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
```

we pass appmodule to the method. bottstrap means start


app.module.ts

```js
bootstrap: [AppComponent]
```

1main.ts,
2 app.module.ts
3 bootstrap: [AppComponent] there is this appcomponent you should know when you start yourself

now angular anlyses the app component

now angular knows how to manage in index.html the app-root selector




# 15 creating a new component

inside app, folder server

server.component.ts


a component its just a typescript class, so that angular cna create instances


decorators enhange your classes. this is not a normal class, is special, its acomponent

in the end everythingis packed by webpack

server.component.ts

```js
import { Component } from "@angular/core";

@Component({
    selector: 'app-server',
    templateUrl: './server.component.html'
})

export class ServerComponent {

}
```


#. 16. understanding the role of appmodule and component declaration

its not ready to be used

app.module.ts

module, to bundle different pieces

the app module, we see 4 properties, declarations, imports, providers, bootstrap

bootstrap, which component should you recognize in the html file

we need to tell angular that we created a component
ngModule,

in declarations array

```js
import { ServerComponent } from './server/server.component';
```

```js
@NgModule({
  declarations: [
    AppComponent,
    ServerComponent
  ],
```

imports, you can add more modules

# 17. Using custom components

app.component.html

```js
<h3>I am in the AppComponent!</h3>
<hr>
<app-server></app-server>
```

our component is there!


# 18. creating components with the cli & nesting components

```js
ng generate component servers
ng g c servers
```

servers.component.ts

```js
<app-server></app-server>
<app-server></app-server>
```

import servers.component in app.module.ts

app.component.html

```js
<h3>I am in the AppComponent!</h3>
<hr>
<app-servers></app-servers>
```

# 19. Working with Component Templates

servers.component.ts

```js
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  template: '<app-server></app-server>',
  styleUrls: ['./servers.component.css']
})
```

`this` if you want with multiple lines


# 20. Working with Component styles

you can use bootstrap

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <app-server></app-server>
      <app-server></app-server>
    </div>
  </div>
</div>
```

app.component.css

```css
h3 {
    color: blue;
}
```

# 21. Fully understanding the component selector

it has to be a unique selector

servers.component.ts

```js
@Component({
  selector: '[app-servers]',
```


a selector can be an attribute, like in css

then,

app.component.html

```html
<div app-servers></div>
```

now angular selects by attribute

you can also select by class

```js
@Component({
  selector: '.app-servers',
```

```html
<div class="app-servers"></div>
```

selected by id does not work

for components, we use the normal one



1. Create two new Components (manually or with cli): WarningAlert and SuccessAlert
2. output them beath each other in the AppComponent
3. Output a warning or success message in the Components
4. Style the components appropriately (maybe some red/ green text?)



# section 2

# 22. What is Databinding

databinnding = communicatin

typescript code (business logic, server) with tremplate (html), what the user sees

output data, ---->
string interpolation {{ data }}
Property Binding ( [property]="data" )



or the other direction, the user clicks a button <---------------------------
business logic <------------- teplate html

we can react to user vents, with event binding
(event)="expression"


combination of Both: Two-way-binding [(ngModel) = "data"]




# String interpolation

server.component.ts

```js

export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';
}
```

server.component.html

```html
<h1>server with iD {{ serverId }} is {{ serverStatus }}</h1>
```

we reference a property in the component

inside the curly braces, anything that returns a string.

you can call a method

```js
export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';

    getServerStatus() {
        return this.serverStatus;
    }
}
```

```html
<h1>server with iD {{ serverId }} is {{ getServerStatus() }}</h1>
```



# String interpolation

server.component.ts

```js

export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';
}
```

server.component.html

```html
<h1>server with iD {{ serverId }} is {{ serverStatus }}</h1>
```

we reference a property in the component

inside the curly braces, anything that returns a string.

you can call a method

```js
export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';

    getServerStatus() {
        return this.serverStatus;
    }
}
```

```html
<h1>server with iD {{ serverId }} is {{ getServerStatus() }}</h1>
```


# 26. Event Binding

once we click the button

i want to output the value

```js
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = "No server was created!"
```

```html
<p>{{ serverCreationStatus }}</p>
```

```js
onCreateServer() {
  this.serverCreationStatus
}
```

we want to listen to the click event
event binding
(name of the vent)=""

```js
(click)="onCreateServer()"
```

this method will be executed whenever we click the button


# 28. Passing and Using Data with Event Binding

we can listen to the input event.

```html
<label>Server Name</label>
<input
  type="text"
  class="form-control"
  (input)="onUpdateServerName()"
>
```

i want to pass what the user types


```html
(input)="onUpdateServerName($event)"
```

```js
  onUpdateServerName(event: any) {
    console.log(event);
  }
```

if we inspect, event, target, at the end, we find the value

so we have it


```js
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = "No server was created!";
  serverName = '';
```

```js
  onUpdateServerName(event: Event) {
    console.log(event);
    this.serverName = event.target.value;
  }
```

he does not like the .value

```js
Property 'value' does not exist on type 'EventTarget'.
```

we can just call .value into an element of htmlinput, so we need to specify it

```js
this.serverName = (<HTMLInputElement>event.target).value;
```


# 29. Important: Formsmodule is Required for Two-Way-Binding

For Two-Way-Binding (covered in the next lecture) to work, you need to enable the ngModel  directive. This is done by adding the FormsModule  to the imports[]  array in the AppModule.

You then also need to add the import from @angular/forms  in the app.module.ts file:

```js
import { FormsModule } from '@angular/forms';
```


# 30. Two-Way-Databinding

we combine property binding and event binding

```js
<input
  type="text"
  class="form-control"
  [(ngModel)]=""
>
```

ngModel is a directive

```js
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName"
>
```

serverName is a property

it will trigger on the input event, and update the value of servername, in ourcomponent, automatically
on the other hand, it will also update the value of the input element, if we change serverName somewhere else

do it in servers.component.ts

```js
  serverName = 'albert';
```

then you see this inside the input and underneath

```html
<input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)"
>
```

this input is empty because its not using two way binding


type in both inputs, then you get it

reacting to events, in BOTH directions

# 31. Combining all forms of Databinding

4 fours of databiding


i want to display the serverName, once we click the Add Server button
comment out p serverName property binding

```js
  onCreateServer() {
    this.serverCreationStatus = "server was created! name is " + this.serverName;
  }
```
































































# 32. Directives!!



directives are instructions in the DOM

with comopnents, with the selector, please add the content in the tag

```html
<p appTurnGreen>Receives a green background</p>
```

```js
@Directive({
	selector: '[appTurnGreen]'
})
export class TurnGreenDirective {

}
```

there are built in directives

# 33. Using ngIf to Output Data Conditionally

structure directives changes the structure of the dom, they have a *

we want to show the name of the message once a server is created

```html
<p *ngIf="">Server was created, server name is {{ serverName }}</p>
```

inside, the directive, a boolean expression

```js
serverCreated = false;
```

```js
  onCreateServer() {
    this.serverCreated = true;
```

when creating the server, a new element in the dom


# 34. enhancing ngif with an else condition

this is # local references, its like a marker

```html
<p #noServer>no server was created</p>
```

ng-template use to mark places in the dom

```html
<ng-template #noServer>
  <p>no server was created</p>
</ng-template>
```

```html
<p *ngIf="serverCreated; else noServer">Server was created, server name is {{ serverName }}</p>
<ng-template #noServer>
  <p>no server was created</p>
</ng-template>
```

# 35. Styling elements dynamically with ngStyle

unlike structural directives, attribte directives don't add or remove elements. They only change the element they were placed on.


server.component.ts

```js
export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';

    constructor() {
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline'
    }
```

we have two different status. we want to change the color

ngStyle is the directive, we want to give him some properties, that's why we use property binding

the square brackets indicates that we want to bind the property, in this case, the property name is ngStyle

we are binding to a property of the directive

we use property binding on the directive ngStyle

the property expects to get a js object

the property name happens to be ngStyle

key value pairs

server.component.html
```html
<h1 [ngStyle]="{backgroundColor: getColor()}">server with iD {{ serverId }} is {{ getServerStatus() }}</h1>

```

server.component.ts

```js
getColor() {
  return this.serverStatus === 'online' ? 'green' : 'red'
}
```


# 36. Applying CSS Classes Dynamically with ngClass

add or remove class dynamically

```js
@Component({
    selector: 'app-server',
    templateUrl: './server.component.html',
    styles: [`
        .online {
            color: white;
        }
    `]
})
```

server.component.html
```js
<h1
    [ngStyle]="{backgroundColor: getColor()}"
    [ngClass]="{online: serverStatus === 'online'}"
>server with iD {{ serverId }} is {{ getServerStatus() }}</h1>
```


# 37. Outputting Lists with ngFor

serveres.component.ts

```js
  servers = ['Testserver', 'Testserver 2']
```

```js
  onCreateServer() {
    this.serverCreated = true;
    this.servers.push(this.serverName);
    this.serverCreationStatus = "server was created! name is " + this.serverName;
  }
```

servers.component.html

```html
<app-server *ngFor="let server of servers"></app-server>
```


# 38. Getting the index when using ngFor

```js
<div *ngFor="let click of clicks; let i = index">

```

index is the keyword, i is our variable, you can name it step



assignment

```js
On each change detection *ngFor newly creates all elements according to the content of the logs array. And you say: if the number of array items is >=5, all elements should get a blue background (no special declaration for individual items).

But you want to style not all elements the same. Thus you have to work with the individual index of each item:

```


in cockpit.component.html

we have two buttons, if we click, we change sth

we want to inform our app.component that we created a new server

we move this two methods of the buttons to app.component.ts

```js
onServerAdded() {
  // this.serverElements.push({
  //   type: 'server',
  //   name: this.newServerName,
  //   content: this.newServerContent
  // });
}

onAddBlueprint() {
  // this.serverElements.push({
  //   type: 'blueprint',
  //   name: this.newServerName,
  //   content: this.newServerContent
  // });
}
```
this methods are called after the button is clicked, when we are done creating the server


in app.component.html

```html
<app-cockpit></app-cockpit>
```

```html
<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>
```

# Custom event,

we want to listen to a custom event, and when the event happens, we want to run a js method
when the server is created

app.component.ts

```js
  onServerAdded(serverData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'server',
      name: serverData.serverName,
      content: serverData.serverContent
    });
  }
```

we expect to get

app.component.html

```html
 <app-cockpit
    (serverCreated)="onServerAdded($event)"
    (blueprintCreated)="onBlueprintAdded($event)"
  >
</app-cockpit>
```

app.component.ts

```js
  onBlueprintAdded(blueprintData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'blueprint',
      name: blueprintData.serverName,
      content: blueprintData.serverContent
    });
  }
```

in the cockpit component we need to emit our own event
we are waiting for events serverCreated and blueprintCreated

we create two new properties, because of the methods in app.component.html

```js
export class CockpitComponent implements OnInit {
  serverCreated;
  blueprintCreated;
```


before we put the @Input, to mark these properties, that they can be set from outside

we want to do the opposite, we want to make sure that this properties are events that we can emit

to make them events, first of all we assign, new EventEmitter
eventemitter needs to be imported

is a generic type, which is indicated <>, in between, you define the type of event data you want to emit

the parenthesis!

to call the constructor of eventemitter, and create a new event emitter object, which is stored in serverCreated



```js
export class CockpitComponent implements OnInit {
  serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```

in cockpit.component.html we have

```html
(click)="onAddServer()">Add Server</button>
```

so, cockpit.component.ts

```js
  onAddServer() {
    this.serverCreated.emit();
  }
```

!!! explanation awesome!

we call the emit method, emit a new event of the type, of serverCreated

we can pass the object, we get from input


whenever i click, i emit, because i specify it in the method that is called when i click, i send the information that the user inputs, because i store it in the properties of the component

in app.component.html

i am listening to this events. whenever they are triggered, i call the onServerAdded, and i update the
serverElements array!!!



ooooh, we need something like the @Input. we want to make the properties serverCreated and blueprintCreated, which are the eventemitters, we want to make them bindable

because we are binding them in app.component.html!

```html
<app-cockpit
  (serverCreated)="onServerAdded($event)"
  (blueprintCreated)="onBlueprintAdded($event)"
>
</app-cockpit>
```

we need the @Output, we are passing our own event out of the component


```js
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output() blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```


# 65. Assigning an alias to custom events

we used @Output


cockpit.component.ts

```js
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```

app.component.html
```html
  (bpCreated)="onBlueprintAdded($event)"
```

# 66. custom property and event binding summary

sometimes we have two compnents next to each other. so this strategy is not the best, because you have to pass the data to the parent, and then down. when the distance is big, we can use services for that

# 67. Understanding view encapsulation

the blue is not there anymore

we had the styling in app.component.css

```css
.container {
  margin-top: 30px;
}

p {
  color: blue;
}
```

but now our p is in server-element.component.html

but, since we are in app.component.css, which is the parent, it should affet all the components in our app right?

angular forces, css encapuslation. just in the component.

now we should copy our p blue, to the components we want to have it

to the server-element!

if we inspect, we have
```js
p[_ngcontent-ejo-2]
```

if we see the html tag, is there

that's how encapsulation works. other p have other attributes

it kind of emulates the shadow dom
each element has its own shadow dom behind it

# 68. more on view encapsulation

you can overwrite the encapsulation

emulated, native, none

none: make sure, we dont see the strange attributes. this component does not use view encapsulation. just this component

if you apply css in this component, it will be applied everywhere

native: uses the shadow dom technology. same result as emulated,

# 69. using local references in templates

we where using output and input to pass data around

in thecockipit, two way data binding to get the server name and content

its good, but we can get the values while clicking.

i just want to save, use, the data, when i click the button, enough to get the value at this point of time

we can do just that

remove the two way binding. place a local reference on the element, it can be placed on any html element

you use # and a name of your choise

this references, will hold, a reference to the element. to the whole element. with all its properties

we can pass this as argument

cockpit.component.ts

```html
<label>Server Name</label>
<!-- <input type="text" class="form-control" [(ngModel)]="newServerName"> -->
<input
  type="text"
  class="form-control"
  #serverNameInput
>
```

we can use local references, only in html, in the template

```html
<button
class="btn btn-primary"
(click)="onAddServer(serverNameInput)">Add Server</button>
```

```js
onAddServer(nameInput) {
  console.log(nameInput);
  this.serverCreated.emit({serverName: this.newServerName, serverContent: this.newServerContent});
}
```

we can also output nameInput.value

you could even put serverNameInput.value in your html, in your template!

```js
onAddServer(nameInput: HTMLInputElement) {
```

```js
  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({serverName: nameInput.value, serverContent: this.newServerContent});
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({serverName: nameInput.value, serverContent: this.newServerContent});
  }
```

and we don't need the newServerName property anymore

# 70. Getting Access to the Template & DOM with @ViewChild

other way!of getting access to local references, or to any element, directly from our ts code

we are passing the references through the method, sometimes we want to have access before we call the method

we can use a decorator

lets do the same for the server content

cockpit.component.html

```html
<input
  type="text"
  class="form-control"
  #serverContentInput
>
```

cockpit.component.ts

```js
 // newServerContent = '';
```

add new property, named: serverContentInput, its just a property, we can add @ViewChild(). we import it from angular core

we need to pass an argument. the selector of the element. how do we want to select the element. not css selector

as a string, the name of the local reference

```js
serverContentInput;
```

```js
@ViewChild() serverContentInput;
```

```js
@ViewChild('serverContentInput') serverContentInput;
```

we could pass a component also

we get access to the local reference

with that, we now get access to our serverContentInput.
what does this hold?

```js
onAddServer(nameInput: HTMLInputElement) {
  console.log(this.serverContentInput);
  // this.serverCreated.emit({serverName: nameInput.value, serverContent: this.newServerContent});
}
```

it holds a ElementRef

```js
  @ViewChild('serverContentInput') serverContentInput: ElementRef;
```

ElementRef has a useful property

the native element property,

```js
onAddServer(nameInput: HTMLInputElement) {
  // console.log(this.serverContentInput);
  this.serverCreated.emit({
    serverName: nameInput.value,
    serverContent: this.serverContentInput.nativeElement.value});
}
```

it will be the input element
we get access to elements directly in our dom, through @viewchild

same for addBlueprint

you should not change the element through this


this.serverContentInput.nativeElement.value = 'Something';

```js
  onAddBlueprint(nameInput: HTMLInputElement) {
    this.serverContentInput.nativeElement.value = 'Something';
    this.blueprintCreated.emit({
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value});
  }
```

don't do that!
angular offeres a better way, with directives

if you want to show sth in the dom, use string interpolation or data binding, not viewchild and local references

# 71. Projecting content into components with ng-content

another way to pass data around

server-element.component.html

```html
<p>
  <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
  <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
</p>
```

stimes you have complex html code, and you want to pass it to a component from outside

maybe we want to put this html inside our component directly

app.component.html

```html
<app-server-element
  *ngFor="let serverElement of serverElements"
  [srvElement]="serverElement"
>
<p>
  <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
  <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
</p>
</app-server-element>
```


there is a special directive

server-element.component.html

```html
<div
  class="panel panel-default"
>
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">

  </div>
</div>
```

in the place i want to render the content. its a directive

i can add ng-contnt

```html
<div
  class="panel panel-default"
>
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <ng-content></ng-content>
  </div>
</div>
```

this serves as a hook you can place in your component, to mark the palce for angular, where it should add any content, it finds between the opening and closing tag

we add the content between our closind and opening tags of our component, and we project the content to our component with the ng-content directive

important for reusing widgets, if the html code is complex, property binding is not the best solution.

we use ng-content. you pass a blog.


# 72. underestanding the component lifecycle

what is this ngOnInit()

is a lifecycle hook, angular supports some of them

if a new component is created by angular, for example, when angular finds a selector, it creates the component

angular goes through a couple of different phases

we can hook in this phases



- ngOnChanges called after a bound input property changes
also when angular creates the component. properties that are decorated with @Input
so, when this properties receive new values

- ngOnInit. executed when the component has been initialized. that does not mean that we can see it, that it was added to the dom
properties can be accessed. the object was created
ngOnInit runs after the constructor

- ngDoCheck. whenever change detection runs. angular determines wheather something change on the template of a component
property value changed, and this property Output
on every checked, not if sth chain. i.e., if you click a btn, which does not change anything

but its an event . on events, angular needs to check if sth changed, otherwise, how would he knows?

angular does that efficiently

ngAfterContentInit, whenever the content which is projected by ng-content has been initialized
not the view of the component itself.

ngAfterContentChecked, called every time the projected content has been checked

ngAfterViewInit called after the component's view (and child views) has been initialized

once our view has been rendered

ngAfterViewChecked called every time the view (and child views) have been checked

once we are sure, all the changes are displayed in the view

ngOnDestroy called once the component is about to be destroyed
you can do some clean up

# seeing lifecycle hooks in action

server-element.component.ts

```js
export class ServerElementComponent implements OnInit, OnChanges {
```

OnChanges import it!

only hook that receives an argument

```js
  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges called!');
    console.log(changes);
  }
```

element is our bound property,


in app.component.html

```html
<button class="btn btn-primary" (click)="onChangeFirst()"></button>
```


app.component.ts

```js
  onChangeFirst() {

  }
```

change the name of the first element

we need to change the way we pass this element to the server-component

server-element.component.ts

we are getting all the object

```js
 @Input('srvElement') element: {type: string, name: string, content: string};
```

now i expect just to get the name

```js
@Input() name;
```

and we output just the name

server-name.component.ts

```js
  <div class="panel-heading">{{ name }}</div>
```

we pass the name to our component with property binding

app.component.html

```html
<app-server-element
  *ngFor="let serverElement of serverElements"
  [srvElement]="serverElement"
  [name]="serverElement.name"
>
```


app.component.ts

```js
  onChangeFirst() {
    this.serverElements[0].name = "Changed";
  }
```

and we see the previous value with TestServer

we couldnt use the element as a object, because objects are reference types

we pass the object via input, therefore both properties in the server element component

and the object in the array of serverelements, they both point to the same place in memory,
and therefore, if we change the name there, it will update in the child component, in the server-eement component


but it will not trigger the ngOnChanges, because technically, the property that we are binding to, we use here with @Input,
that didnt changed, the value didnt change, its the same object in memory

there is differences between references and primitive types


that's why we are binding to a primitive, just name, which is a string

ngOnChanges is fired!

the constructor, ngOnInit were not called




DoCheck, you need to import it

gets called, whenever angular checks for every changes
there are a couple of triggers. an event was called by clicking, a promise is finished

```js
  ngDoCheck() {
    console.log("ngDoCheck called");
  }
```

content are the two ifs

```js
  ngAfterContentInit() {
    console.log('ngAfterContentInit called!');
  }
```

```js
  ngAfterContentChecked() {
    console.log('ngAfterContentChecked called!');
  }
```

```js
  ngAfterViewInit() {
    console.log('ngAfterViewInit called!');
  }

  ngAfterViewChecked() {
    console.log('ngAfterViewChecked called!');
  }
```
server-element.component.ts

```js
  ngOnDestroy() {
    console.log('ngOnDestroy called');
  }
```

do not forget

server-element.component.ts

```js
export class ServerElementComponent implements OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy
```


app.component.html

```js
  <button class="btn btn-danger" (click)="onDestroyFirst()">Destroy first component</button>
```

# delete an element, deleting

app.component.ts

```js
  onDestroyFirst() {
    this.serverElements.splice(0,1);
  }
```

# 74. Lifecycle hooks and templates access

cockpit.component.ts

@ViewChild
we use it in the cockpit, to get access to the element from our dom, template

lets do the same

server-element.component.html


```html
  <div class="panel-heading" #heading>{{ name }}</div>
```

server-element.component.js

```js
@ViewChild('heading') header: ElementRef;
```

```js
  ngOnInit() {
    console.log('ngOnInit called!');
    console.log(this.header.nativeElement.textContent);
  }
```

we have an empty line!

```js
  ngAfterViewInit() {
    console.log('ngAfterViewInit called!');
    console.log("text content", this.header.nativeElement.textContent);
  }
```

ngAfterViewInit gives you access to the elements of the template

but before you reach this hook, you cant do that

# 75. getting access to ng-content with @ContentChild





























































































































































































