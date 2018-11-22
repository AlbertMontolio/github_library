# S3. Directives

Directives are instructions in the DOM

In the comopnents, with the selector, we tell Angular to apply the directive in the tag.

Say we have a directive in a paragraph tag.

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

This is how you create a directive, but, of course, there are built-in directives.

# 3.1 Using ngIf to Output Data Conditionally

We have two types of directives: the ones that change the structure of the DOM, and the ones that just change the attribute of an element (like styling, colors etc.). They are called structural directives and attribute directives.

Structure directives changes the structure of the dom, therefore they have a *

In our server example, let's show the name of the message once a server is created.

We use the `ngIf`directive

```html
<p *ngIf="">Server was created, server name is {{ serverName }}</p>
```

Inside the directive, we place a boolean expression.

```js
serverCreated = false;
```

```js
  onCreateServer() {
    this.serverCreated = true;
```

Now when we create a server, a new element is created in the DOM.


# 3.2 Enhancing ngIf with an else condition

We introduce the concept of local references.

We indicate them witha hasthag #, it's like a marker

```html
<p #noServer>no server was created</p>
```

We use ng-template to mark places in the dom

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

# 3.3 Styling elements dynamically with ngStyle

Unlike structural directives, attribute directives don't add or remove elements. They only change the element they are placed on.

server.component.ts

```js
export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';

    constructor() {
        this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline'
    }
```

Here we have two status. We want to change the color depending on that.

We will use the `ngStyle` directive. We want to give to this directive some properties, that's why we use property binding.

server.component.html
```html
<h1 [ngStyle]="{backgroundColor: getColor()}">server with iD {{ serverId }} is {{ getServerStatus() }}</h1>
```

!!!
The square brackets indicates that we want to bind the property, in this case, the property name is ngStyle (like the directive)

>We are binding to a property of the directive

>We use property binding on the directive ngStyle

The property expects to get a js object

The property name happens to be ngStyle, like the directive

We use key value pairs to specify the characteristics of the element.

server.component.html
```html
<h1 [ngStyle]="{backgroundColor: getColor()}">server with iD {{ serverId }} is {{ getServerStatus() }}</h1>
```

Remember we get the color dynamically:

server.component.ts
```js
getColor() {
  return this.serverStatus === 'online' ? 'green' : 'red'
}
```


# 3.4 Applying CSS Classes Dynamically with ngClass

We can add or remove classes dynamically

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

>If the expression evaluates to true, the class will be applied


# 3.5 Outputing Lists with ngFor

We use ngFor to loop, to repeat elements.

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

We place the `*ngFor` in the element we want to repeat, not in the element that wraps the list.

# 3.6 Getting the index when using ngFor

```js
<div *ngFor="let click of clicks; let i = index">

```

>index is the keyword, `i` is our variable, you can name whatevery you want, like el, step, etc.



## Exercise !!! delete

We want to change the background of elements, if the index is greater than a number.

On each change detection `*ngFor` newly creates all elements according to the content of the logs array. Say that: if the number of array items is >=5, all elements should get a blue background (no special declaration for individual items).

But we don't want to style all elements the same. Thus we have to work with the individual index of each item.

In cockpit.component.html

We have two buttons, if we click, we change sth.

We want to inform our `app.component` that we created a new server.

We move these two methods of the buttons to app.component.ts

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
These methods are called after the button is clicked, when we are done creating the server


in app.component.html

```html
<app-cockpit></app-cockpit>
```

```html
<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>
```

# 3.7 Custom event

We want to listen to a custom event and when the event happens, we want to run a js method, for example,
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

We know what we expect to get. All the info from the inputs. We get it thanks to the $event.

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

In the cockpit component file we need to emit our own event.
We are waiting for events serverCreated and blueprintCreated

We create two new properties, because of the methods in app.component.html

```js
export class CockpitComponent implements OnInit {
  serverCreated;
  blueprintCreated;
```

Before them we put the decorator `@Input`, so that we mark these properties, so that they can be set from outside.

But, actually we want to do the opposite, we want to make sure that these properties are events that we can emit.

To make them events, first of all we assign a `new EventEmitter`.

In order to use EventEmitters, we need to import their module.

An EventEmitter is a generic type, which is indicated by <>. In between, you define the type of event data you want to emit

the parenthesis!

Now, let's call the constructor of the EventEmitter and create a new EventEmitter object, which is stored in serverCreated

cockpit.component.ts

```js
export class CockpitComponent implements OnInit {
  serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```

in `cockpit.component.html` we have the button Add Server

```html
(click)="onAddServer()">Add Server</button>
```

Therefore, in `cockpit.component.ts`, we can emit our Event when we press the button.

```js
  onAddServer() {
    this.serverCreated.emit();
  }
```

Let's understand what happens again.

We call the emit method, this emits a new event of the type that we decied on serverCreated.

To this emit action, we can pass the object that we get from the input.


Whenever I click the button, I emit the event, because I specified that in the method that is called when I click the button. Then, I send the information that the user inputs, because I store it in the properties of the component.

But just emiting is not enough.

In `app.component.html` we are listening to this event. Whenever this event is triggered, I call the onServerAdded, and I update the
serverElements array.

It's always about emiting and listening to.

We still need one thing: we need something like the `@Input`. We want to make the properties `serverCreated` and `blueprintCreated`, which are the EventEmitters, we want to make them bindable.

because we are binding them in app.component.html, look:

```html
<app-cockpit
  (serverCreated)="onServerAdded($event)"
  (blueprintCreated)="onBlueprintAdded($event)"
>
</app-cockpit>
```

Instead of the `@Input` decorator, now we need the `@Output`, we are passing our own event out of the component

```js
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output() blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```

It's like you have to inform Angular that this Event Emitter is going to the outside, someone is listening to it.

!!! improve explanation with an example


# 3.8 Assigning an alias to custom events

So far we used the `@Output` decorator. We can pass an alias to it.

cockpit.component.ts

```js
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```

app.component.html
```html
  (bpCreated)="onBlueprintAdded($event)"
```

# 3.9 Custom property and event binding summary

Sometimes we have two components next to each other. Therefore, this strategy is not the best, because you have to pass the data to the parent, and then down. When the distance is big, we can use services for that. We will cover this later on the course.

# 3.10 Understanding view encapsulation

At the beginning, we had the styling in app.component.css

```css
.container {
  margin-top: 30px;
}

p {
  color: blue;
}
```

But what if we have our paragraph in a component, not in the app.component, for example in `server-element.component.html`.

We could think that, since we have the styling in `app.component.css`, which is the parent, it should affect all the components in our app. But does not the behaviour.

Angular forces CSS encapsulation. Style just applies in the component.

Therefore, we should copy our paragraph blue style to the components where we want to have this style, to the server-element.

if we inspect the browser, we see
```js
p[_ngcontent-ejo-2]
```

Angular places own ids to track where to apply the style.

That's how encapsulation works. Other paragraphs will have other attributes.

This principle kind of emulates the shadow DOM.
Each element has its own shadow DOM behind it. You can learn more about shadow DOM.

# 3.11 More on view encapsulation

We can overwrite the encapsulation. There are three modus of encapsulation:

- emulated,
- native,
- none

none: we make sure, we don't see the strange attributes. This component does not use view encapsulation. It just applies for this particular component.

If you apply CSS in this component, it will be applied everywhere. Be careful with this approach. Angular wants to work with isolated components. If you want general style, move it to style.scss from the app folder.

native: uses the shadow DOM technology. Same result as emulated, the default one.

# 3.12 Using local references in templates

Before, we where using `@Output` and `@Input` to pass data around.

In the cockpit.component, we used two way data binding to get the server name and content

This works perfectly, but we can get the values while clicking.

I just want to save, use the data, when I click the button. This is enough to get the value at this point of time

For that, we remove the two way binding, we place a local reference on the element. Remember, a local reference can be placed on any HTML element. We just need to use # and a name of your choice.

This reference will hold a reference to the element. To the whole element, with all its properties.

We can pass this as argument

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

>We can use local references only in HTML, in the template.

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

We can also output the `nameInput.value`.

You could even put `serverNameInput.value` in your HTML, in your template.

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

This way, we don't need the `newServerName` property anymore.

# 3.13 Getting Access to the Template & DOM with @ViewChild

There is another way of getting access to local references, or to any element, directly from our ts code.

So far, we were passing the references through the method. But sometimes we want to have access before we call the method

For that, we can use a decorator.

Let's do the same for the server content.

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

We add a new property, named: serverContentInput. Its just a property, we can add @ViewChild().
We need to import it from Angular Core.

We need to pass an argument: the selector of the element.

How do we want to select the element? now, not with a CSS selector.

We pass as a string, the name of the local reference

```js
serverContentInput;
```

```js
@ViewChild() serverContentInput;
```

```js
@ViewChild('serverContentInput') serverContentInput;
```

We could pass a component also.

We get access to the local reference.

With that, we now get access to our serverContentInput.
What does it hold?

```js
onAddServer(nameInput: HTMLInputElement) {
  console.log(this.serverContentInput);
  // this.serverCreated.emit({serverName: nameInput.value, serverContent: this.newServerContent});
}
```

It holds a ElementRef, therefore, we need to specify it:

```js
  @ViewChild('serverContentInput') serverContentInput: ElementRef;
```

ElementRef has a useful property: the native element property,

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
