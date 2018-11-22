# 94. Building and using a dropdown directive

we want to open the drop downs!!!

they dont work because we didn import bootstrap childs script code

angular should be the only thing interacting with my dom

crazy

add a directive in the share folder

dropdown.directive.ts

```js
import { Directive } from "@angular/core";

@Directive({
    selector: '[appDropdown]'
})

export class DropdownDirective {

}
```

what should this directive do

add some functionality

add css class, to the element it sits on, when we click

which class we need to attach

open

recipe-detail.component.html

```html
<div class="row">
  <div class="col-xs-12">
    <div class="btn-group open">
      <button type="button" class="btn btn-primary dropdown-toggle">Manage Recipe <span class="caret"></span></button>
      <ul class="dropdown-menu">
```

# exercise 

to listen to a click, hostlistener

i want to listen to a click event, toggleOpen(), i want to toggle if its opened or not

isOpen directive, set false to initaal

dropdown.directive.ts

```js
export class DropdownDirective {
	
	isOpen = false;
	@HostListener('click') toggleOpen() {
		this.isOpen = !this.isOpen;
	}
}
```

to dynamically attach or detacch a css class, depending on this, i need to add hostbinding

@HostBinding to isOpen

allows us to bind to properties of the element where the directive is placed on
i want to bind to the property class of that element, class is an array of classes.

here, its all about the open class.

```js
export class DropdownDirective {

	@HostBinding('class.open') isOpen = false;
	@HostListener('click') toggleOpen() {
		this.isOpen = !this.isOpen;
	}
}
```

the rest is handled by angular, since the bind to is opened, this will not be attached initiall, whenever isopen changes, it will updated

to use it, i need to add it in app.module.ts

on the div, 

recipe-detail.component.html

```html
<div class="btn-group" appDropdown >
```

header.component.html


```html
<li class="dropdown" appDropdown>
```





































