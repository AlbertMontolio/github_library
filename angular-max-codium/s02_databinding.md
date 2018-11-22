# S02: Databinding

# 2.1 What is Databinding

We can undesrtand data binding as communication

We communicate the typescript code (business logic, server) with the template (html, what the user sees).

In order to output data we use string interpolation:

```js
output data, ---->
string interpolation {{ data }}
```

We can also bind data to a property. In this case, the user does not see the data directly, but we change attributes from an element, normally style.

```js
Property Binding ( [property]="data" )
```


Or the other direction, the user clicks a button

business logic <------------- template html

We can react to user events, with event binding

```js
(event)="expression"
```

We can combine both with Two-way-binding. For that, we use ngModel

```js
[(ngModel) = "data"]
```


# 2.2 String interpolation

We can have some data in our typescript code, in our business logic. Maybe this data is coming from an API, from our backend.

server.component.ts

```js

export class ServerComponent {
    serverId: number = 10;
    serverStatus: string = 'offline';
}
```

We can show this data in our html

server.component.html

```html
<h1>server with iD {{ serverId }} is {{ serverStatus }}</h1>
```

We reference a property in the component

Inside the curly braces, we can put anything that returns a string.

Therefore, you can call a method, as long as it returns a string.

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

# 2.3 Event Binding

We want to make things happen, once we click a button.

For exmaple, I want to output a value.

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

We want to listen to the click event, we use event binding

```js
(name of the vent)=""
```

```js
(click)="onCreateServer()"
```

This method will be executed whenever we click the button.


# 2.4 Passing and Using Data with Event Binding

We can listen to the input event, for example in a Form.

```html
<label>Server Name</label>
<input
  type="text"
  class="form-control"
  (input)="onUpdateServerName()"
>
```

I want to pass what the user types

```html
(input)="onUpdateServerName($event)"
```

I can pass the whole event, inside we find the data.

```js
  onUpdateServerName(event: any) {
    console.log(event);
  }
```

If we inspect, event, target, at the end, we find the value

Therefore, we can access the value


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

Angular does not like the `.value`

```js
Property 'value' does not exist on type 'EventTarget'.
```

The reason is, we can just call `.value` into an element of HTMLinput, so we need to tell Angular that we are dealing with HTMLinput.

!!!

```js
this.serverName = (<HTMLInputElement>event.target).value;
```


# 2.5 Important: FormsModule is Required for Two-Way-Binding

For Two-Way-Binding (covered in the next lecture) to work, you need to enable the ngModel directive. This is done by adding the FormsModule  to the imports[] array in the AppModule.

You then also need to add the import from @angular/forms  in the app.module.ts file:

```js
import { FormsModule } from '@angular/forms';
```

# 2.6 Two-Way-Databinding

We combine now property binding and event binding. We use ngModel

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

This will be triggered on the input event and update the value of serverName, in our component, automatically.
On the other hand, it will also update the value of the input element, if we change serverName somewhere else, for example, from our backend.

In servers.component.ts

```js
  serverName = 'albert';
```

Then you see 'albert' inside the input and underneath, in the paragraph.

If we use just event binding:

```html
<input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)"
>
```

this input is empty because it's not using two way binding



# 2.7 Combining all forms of Databinding

I want to display the serverName, once we click the Add Server button,
comment out the paragraph serverName property binding

```js
  onCreateServer() {
    this.serverCreationStatus = "server was created! name is " + this.serverName;
  }
```

