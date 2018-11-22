# S01: Introduction

We use Angular to build Front End Application. They are very reactive, that means, everything happens in the "same" page.

What is Angular?

Angular is a Javascript framework which allows you to create reactive Single-Page-Applications (SPAs).

It's just one HTML file, we change the url for the user, but we don't actually change the page.

The experience is as mobile applications.

The JS changes the DOM during run time.

In order to understand the Angular versioning, Angular 2 was completed rewrite from Angular 1.

Angular 1 was the first framewrok to make 1 single applications (1 page).

Angular 2 or higher is called just Angular

From Angular 2 on, the versions are just small incremental improvements.

# 1.2: The Command Line Interface (CLI)

From the command line we can create angular apps, build them for production, create components etc.

If you want to dive deeper into the CLI and learn more about its usage, have a look at its official documentation:

https://github.com/angular/angular-cli/wiki

You encountered issues during the installation of the CLI or setup of a new Angular project?

A lot of problems are solved by making sure you're using the latest version of NodeJS, NPM and the CLI itself.

Updating NodeJS:

Go to `nodejs.org` and download the latest version - uninstall (all) installed versions on your machine first.

Updating npm:

Run [sudo]

```js
npm install -g npm  (sudo  is only required on Mac/ Linux)
```

In my machine:

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
In my machine:

```js
+ @angular/cli@6.0.8
```

```js
ng new my-dream-app --style=scss
ng serve
```

Important: to be able to use scss we need the flag:

```js
--style=scss
```

```js
ng new my-first-app
```

To run the server that hosts our Angular App locally:

```js
ng serve
```


Everyting starts in the index.html

Then, app component is inserted in app-root component

# 1.3 First example: Two way binding

With two way binding you can change a variable from the component (backend) and from the browser (input form).
In order to use that, you need to import some modules.

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

While you tip, you see how the text is updated.

#1.4 Course Structure

In this course we are going to tackle:
- the basics
- components & databinding
- directives
- services & dependency injection
- routing
- observables
- forms
- pipes
- http, authentication
- optimizations & ngModules
- deployment
- animations
- testing

# 1.5 Typescript

Typescript has more features than vanilla JS: types, classes, interfaces etc.

Afterwards is compiled to Javascript.

# 1.6 Bootstrap

We need to install the package:

```js
npm install --save bootstrap@3
```

this installs locally:

```js
+ bootstrap@3.3.7
```

make aware that you installed it at the file:

```js
angular.json
```

In the nodes: architecture, build, styles

```js
"styles": [
  "node_modules/bootstrap/dist/css/bootstrap.min.css",
  "src/styles.css"
],
```

You can find the file manually in node_modules. Never touch the content of this folder.

run again

```js
ng serve
```

In the head section, we should see now the two imports of the style files.


# 1.7 how and Angular app gets loaded and started

The `index.html` is the file served by the server. Here we find the app-root

```js
app-root
```

`app-root` is a component, the app component


In `app.component.ts` we find a selector: `app-root`.
It replaces the HTML file app-root for the template of the app component.

How is angular triggered? in `index.html.`

`ng serve` creates the scripts at the end of the `index.html`.

`main.ts` is the first code it's executed.

The last line is the important one,

```js
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
```

We pass the AppModule to the method. Botstrap means start the app, nothing to do with the CSS library.


In app.module.ts we tell which Component to load.

```js
bootstrap: [AppComponent]
```

Sum up:

1 main.ts,
2 app.module.ts
3 bootstrap: [AppComponent] We tell angular, hej, you should know about this appcomponent when you start the app.

Angular analyses the app component.

This way Angular knows how to manage in `index.html` the `app-root` selector.


# 1.8 Creating a new component

Inside folder app, we create a folder server

```js
server.component.ts
```

>A component is just a typescript class, so that angular can create instances.


We use decorators to enhance our classes. We tell Angular this is not a normal class, it is a special one, a component.

At the end everything is packed by Webpack.

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

A component needs to import the module Component from Angular Core. It needs to have the decorator @Component.
You need to export it, to tell Angular that it exists and we want to use it.


# 1.9 Understanding the role of AppModule and component declaration

We created the component, but it is not ready yet to be used.

```js
app.module.ts
```

In the module file, we bundle different pieces of our app

In the app module we see 4 properties: declarations, imports, providers and bootstrap.

We saw bootstrap: which component Angular should recognize in the `index.html` file.

Now we need to tell angular that we have created a component.

In `ngModule`, in declarations array:

```js
@NgModule({
  declarations: [
    AppComponent,
    ServerComponent
  ],
```

We can not forget to import the component, to tell Angular where to find it. This is Typescript syntax.

```js
import { ServerComponent } from './server/server.component';
```

In imports we can add more modules.

# 1.10. Using custom components

app.component.html

```js
<h3>I am in the AppComponent!</h3>
<hr>
<app-server></app-server>
```

If we use the selector to inject our component, we can see it in the browser.


# 1.11 Creating components with the CLI & nesting components

We can use the CLI to create components

```js
ng generate component servers
ng g c servers
```

servers.component.ts

```js
<app-server></app-server>
<app-server></app-server>
```

Don't forget to import `servers.component` in `app.module.ts`

app.component.html

```js
<h3>I am in the AppComponent!</h3>
<hr>
<app-servers></app-servers>
```

# 1.12 Working with Component Templates

We can create files to put all the HTML of the component.

servers.component.ts

```js
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  template: '<app-server></app-server>',
  styleUrls: ['./servers.component.css']
})
```
You can use `this` if you want to write with multiple lines.


# 1.13 Working with Component styles

We installed bootstrap, we can use it.

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

We can write our one style in css files.

app.component.css

```css
h3 {
    color: blue;
}
```

# 1.14 Fully understanding the component selector

The component selector has to be a unique selector

servers.component.ts

```js
@Component({
  selector: '[app-servers]',
```

A selector can even be an attribute, like in css

then, we use it as an attribute

app.component.html

```html
<div app-servers></div>
```

Now angular selects by attribute.

We can also select by class

```js
@Component({
  selector: '.app-servers',
```

```html
<div class="app-servers"></div>
```

As note: selected by id does not work.

For components, we use the normal one


Exercise:

1. Create two new Components (manually or with cli): WarningAlert and SuccessAlert
2. Output them next to each other in the AppComponent
3. Output a warning or success message in the Components
4. Style the components appropriately (maybe some red/ green text?)

