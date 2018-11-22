# Section 15: Handling Forms in Angular Apps

# 171. Module Introduction

formsss, on your html page. how angular can help

why do we need that. a form we submit to the server.

but angular is a single page application, so there is no submitting to the server

instead, we handle the form with angular, and after that, use http service from angular, to that server

get the value, the user enteres

check if a form is valid. conditionally change how the form is presented, red borderes if the input is wrong

# 172. why do we need angular's help?

```html
<form>
	<label>name</label>
	<input type="text" name="name">
	<button type="submit">Save</button>
</form>
```

is the form valid? all that happen in js, in ts
on the angular side of the app

we need a js object representation of the form, to work with

```js
{
	value: {
		name: 'max',
		email: 'test@test.com'
	}
	valid: true,
}
```

# 173. Template-driven (TD) vs Reactive Approach

two approaches:

template-driven

set up your form in the template, in html form, angular infer the structure of the form, infer which controls, which inputs, makes it easy for you to start quicky

more complex aprpoach

the reactive approach

you define the structure of the form in ts code

form is created programatically and synchronized with the dom

then you manually connect it

greater controll over it


you can fine tune every piece of the form

we start with the template-driven

# 174. An Example form

app.compoment.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form>
        <div id="user-data">
          <div class="form-group">
            <label for="username">Username</label>
            <input type="text" id="username" class="form-control">
          </div>
          <button class="btn btn-default" type="button">Suggest an Username</button>
          <div class="form-group">
            <label for="email">Mail</label>
            <input type="email" id="email" class="form-control">
          </div>
        </div>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select id="secret" class="form-control">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>
```

on the form tag, i dont have the action attribute pointing to some route, or the method, typically post

this form should not be submitted to a server, i dont want to click an make an ahttp request

if i click, nothing happens

how angular infers such a form, create js object

# 175. TD: Creating the form and registering the controls

understand how angular creates such a js object reprsenting our form, in the template driven approach. make sure, than in your app module, you actually import the forms module, 

add it in imports array, this built in module, shipping with angular, it includes a lot of cuntionality

```js
import { FormsModule } from '@angular/forms';
```

```js
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
```

we need this imports

angular will automatically create a form for you, a js representation of a form, when it detects a tag form

form element, serving as a selector, for some angular directives, which then creates a js representation of the form for you

angular will not detect your inputs in this form

not every input, may be a control in your js object
we need to register controls manually

tell angular how does our form look like, which controls do we want to have

you pick the input you want as a control

and then you add, ngModel

we saw it in the two way databinding

```html
<input 
  type="text" 
  id="username" 
  class="form-control"
  ngModel
>
```

in two way data bnding though

we did [(ngModel)]

but now not

this will be enough, to tell angular, hej, this input, is a control of my form

ngModel is a directive, made available in the FormsModule

for this to work, we need to give angular one other piece of information

the name of the control!
til now, angular just nows that this contorl should be part of the representation of the js object of my form

whats the name?

we do this by adding, the normal html attribute
name=""

```html
<input 
  type="text" 
  id="username" 
  class="form-control"
  ngModel
  name="username"
>
```

same for the email

```html
<input 
  type="email" 
  id="email" 
  class="form-control"
  ngModel
  name="email"
>
```

same for select

```html
<select 
  id="secret" 
  class="form-control"
  ngModel
  name="secret"
>
```

that way, we registered all the controls. we cant see that much though

how can we submit such a form

how to see this key value pairs, reprsenting what the user has input

# 176. TD: Submitting and using the form

make the form submittable

for this, i go to app.component.ts

new method, onSubmit()

click listener on the button of submit? no

this button is type submit, if we click it, something else will happen. default behavior of html. if you have. abtn in a form element, will send a request. it will also trigger a js event.

angular takes advantage of this

gives a directive we can place in the form as a hole. it is called, ng submit, it only gives us one event we can listen to

```html
<form (ngSubmit)>
```

this event, will be fired when ever this form is submitted

we can call onSubmit()

on this form object, in html

we want to have access to the form created by angular

local references!

we could place #f

now we can access this in our template, and we can pass f as an argument

```js
<form (ngSubmit)="onSubmit(f)" #f>
```

```js
  onSubmit(form: HTMLFormElement) {
    console.log(form);
  }
```

i see the form object

its not a js object created by angular. the obj is there

in the template

local references equal to something.
something that is exposed by this form

the form element is kind of a selector for a directive, which creates the js object

```js
#f="ngForm"
```

it tells angular, hej, please
give me accesss to this form you created automatically

to the js object, created by angular, automticalyl

```js
<form (ngSubmit)="onSubmit(f)" #f="ngForm">
```

now, we get




```js
  onSubmit(form: NgForm) {
    console.log(form);
  }
```

we need to import it

```js
import { NgForm } from '@angular/forms'
```

we get a ngForm object

we have a lot of properties
we have a value property, we see a cuple of key value pairs

this is how we can submit such a form, and get access to the form object created by angular

setup of controls also worked

# 177. TD: Understanding Form state

we saw the value property

we have a lot of properties in this js object

understand the stat of our form

controls, email secret uesrname

each control has a couple of properties

dirty, disabled, enabled, errors

dirty: true

if we change somehting about this form, its true. if i dont type anything, is false

disabled, if the form is disabeld

enabled, the form is valid

later we add validators

touched. did we click intosome of the fields?

dirty, we have to change the vlaue of the fields

this properties are useful in ux

# 178. TD: Accessing the form with @ViewChild

we submit the form, by passing the form, to the onSubmit method

we always will use that

we dont have to submit it here though

@ViewChild, access a local reference, in our typescript code

we can also use this in the ts

alternative approach

@ViewChild, decorator

i want to get access to the element which has the local reference to the form

@ViewChild('f')

store this in a varaible, signupForm

type ngFOrm of course

on onSubmit, i can out

```js
  onSubmit() {
    console.log(this.signupForm);
  }
```

it also works

there is a usecase, later we see it

this approach is valid

why this may be useful

how can be control the validity of the form?

add validation!

# 179. TD: Adding Validation to check User Input

validate the user input!

make sure, none of the fields is empty, and the email is valid

app.component.html

add such validators

since we are using the TD, we can only add validators in the template

```html
<input 
  type="text" 
  id="username" 
  class="form-control"
  ngModel
  name="username"
  required
>
```

required, is a default html attribute you can add to an input

angular will detect, it acts as a selector to built in directive shiped with angular


it automacitcally configure your form, to take this intoaccount

on the emaill, also add required,

and also, email directive

its not a html attribute, its a directive

makes sure this is a vlaid email address

```html
<input 
  type="email" 
  id="email" 
  class="form-control"
  ngModel
  name="email"
  required
  email
>
```

valid attribute is false!

if we go to the controls, we also have a valid attribute

if we inspect the email in the html code, it adds a couple of classes
ng-dirty ng-touched ng-valid

if i remove the @ in the email, ng-invalid is added

angular dynamically add these classes, giving us info about the state of the contol

with this classes, we can style this inputs conditionally

improve ux

# 180. Built-in validators & using HTML5 Validation

Which Validators do ship with Angular? 

Check out the Validators class: https://angular.io/docs/ts/latest/api/forms/index/Validators-class.html - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: https://angular.io/api?type=directive - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the ngNativeValidate  to a control in your template.

# 181. TD: Using the Form State

we can go back to our form, 

the easiest way, in the submit button, lets disable the btn if the form is not valid

add property binding. disabled property binding.
the condition is: wether the form (f) is valid

```html
<button 
  class="btn btn-primary" 
  type="submit"
  [disabled]="!f.valid"
>Submit</button>
```

now, the css classes

stylesheet

```css
.ng-invalid {
	border: 1px solid red;
}
````


everything is red

even the form

overall form is also invalid. 

a better approach, 

best way is, we want to add in in inputs


```css
input.ng-invalid {
	border: 1px solid red;
}
````

we have the border right from the start

better aproach

ng-invalid and ng-touched

at least user has clicked

```html
input.ng-invalid.ng-touched {
	border: 1px solid red;
}
```

we could also add a warning message

please enter a valid value, below the input email

```js
add *ngIf, if the input is wrong
```

```html
<div class="form-group">
  <label for="email">Mail</label>
  <input 
    type="email" 
    id="email" 
    class="form-control"
    ngModel
    name="email"
    required
    email
  >
</div>
<p *ngIf="">Please enter a valid value</p>
```

how do we determin if this input is vlaid?

# 182. Outputting Validation Error Messages

help-block

```html
<span class="help-block">Please enter a valid email!</span>
```

show if email is invalid

quick an easy way, to get access to the control, is by adding a local reference

```js
#email, associated this to ngModel
```
the ngModel kind of exposes info about the control, with this, 

```html
<div class="form-group">
  <label for="email">Mail</label>
  <input 
    type="email" 
    id="email" 
    class="form-control"
    ngModel
    name="email"
    required
    email
    #email="ngModel"
  >
  <span class="help-block" *ngIf="!email.valid">Please enter a valid email!</span>
</div>
```

```html
<span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>
```

pleasant ux to the user!! 

# 183. TD: Set default values with ngModel property binding

we want to define some default value in the drop down

now is empty by default. one of the two questions should be selected

ngModel, we can use it with property binding

now we can bind it to a value

the control!

[ngModel]=""

we can hard code a string, with the single quotes

or we set a property in the ocmpont

```js
defaultQuestion = 'pet';
```

pet because

```html
<option value="pet">Your first Pet?</option>
<option value="teacher">Your first teacher?</option>
```

```html
[ngModel]="defaultQuestion"
```

```html
<select 
  id="secret" 
  class="form-control"
  ngModel
  name="secret"
  [ngModel]="defaultQuestion"
>
  <option value="pet">Your first Pet?</option>
  <option value="teacher">Your first teacher?</option>
</select>
```

now pet is selected

# 184. TD: Using ngModel with Two-Way-Binding

you also want to instantially react to any changes

for the question, add a new form grouop

below the dropdown. text area





```html
<div class="form-group">
  <textarea 
    name="questionAnswer" 
    rows="10"
    class="form-control"
    ngModel
  ></textarea>
</div>
```

ngModel to get access to whatever the user enters as a reply

maybe we instantilly want to repeat this reply

```js
your reply: {{ answer }}
```

we can use two way binding on ngmODEL, bind it to the nswer property

```js
answer = '';
```

```js
[(ngModel)]="answer"
```


ngModel

no binding. to tell angular that input is a control

one way binding. to give this control a default value


two way binding to instantaly output with that value

# 185. TD: Grouping Form Controls

on the value object we get

we want to group some things

group email and username, group questionanswer and secret

to have some structure in our object

validate our groups also would be nice

on our first group, you need a div, with an id, user-data

you can place a new directive, ngModelGroup directive, this will group this
ngmoDELgROUP, needs to be set equal to a string, userData

key name for this group

in controls, 

if we inspect html code, the div with user-data, we see the classess

we can also check the validity of the group

we can also get access to the js representation

by adding a local reference


```html
<div 
  id="user-data" 
  ngModelGroup="userData"
  #userData="ngModuleGroup"
>
```
we could output a message if this whole grouop isnot valid

```html
<p *ngIf="!userData.valid && userData.touched">User Data is invalid!</p>
```

# 186. TD: Handling Radio Buttons

app.component.ts

property with some genres

```js
genders = ['malme', 'female']
```

```html
<div class="radio" *ngFor="let gender of genders">
  <label>
    <input 
      type="radio"
      name="gender"
      ngModel
      [value]="gender"
    >
    {{ gender }}
  </label>
</div>
```


if you want to set it to a default value, one way binding to ngmODEL

we can add the required directive

it works just any other input



# 187. TD: Setting and Patching Form Values

suggest name btn

app.component.ts

```js
  suggestUserName() {
    const suggestedName = 'Superuser';
  }
```

on clicking this button, we populate this input

two way data binding? it could work

we have access to the ngForm, through @ViewChild

it would be nice to set the value of our inputs, we can

we have two methods we can use

1. 

on our signUpForm.setValue()

set the value of the whole form

pass js object, exactly representing our form

```js
  suggestUserName() {
    const suggestedName = 'Superuser';
    this.signupForm.setValue({
      userData: {
        username: suggestedName,
        email: ''
      },
      secret: 'pet',
      questionAnswer: '',
      gender: 'male'
    });
  }
```

```html
<button 
  class="btn btn-default" 
  type="button"
  (click)="suggestUserName()"
>Suggest an Username</button>
```

if we already had data, we delete it

its not the best approach, but it shows how you can edit the whole form

!!! maybe useful to edit formulari

signupForm is like the container

this.signupForm.form.patchValue();
we can pass a js object, where you only overwrites certain controls
like userdata, inside, the username


```js
suggestUserName() {
  const suggestedName = 'Superuser';
  this.signupForm.form.patchValue({
    userData: {
      username: suggestedName
    }
  });
}
```

# 186. TD: Using form data

we only log in

lets add sth, an horizontal line

lets summary our data

```html
<hr>

<div class="row">
  <div class="col-xs-12">
    <h3>your data</h3>
    <p>Username: </p>
    <p>Mail: </p>
    <p>Secret Question: </p>
    <p>Answer: </p>
    <p>Gender: </p>
  </div>
</div>
```

populate this once the form is submitted

app.component.ts

user 

```js
user = {
  username: '',
  email: '',
  secretQuestion: '',
  answer: '',
  gender: ''
}
```







```js

  onSubmit() {
    this.user.username = this.signupForm.value.userData.username;
    this.user.email = this.signupForm.value.userData.email;
    this.user.secretQuestion = this.signupForm.value.secret;
    this.user.answer = this.signupForm.value.questionAnswer;
    this.user.gender = this.signupForm.value.gender;

    console.log(this.signupForm);
  }
```


track if we submited the form

```js
submitted = false;
```

```js
submitted = false;

onSubmit() {
  this.submitted = true;
  this.user.username = this.signupForm.value.userData.username;
  this.user.email = this.signupForm.value.userData.email;
  this.user.secretQuestion = this.signupForm.value.secret;
  this.user.answer = this.signupForm.value.questionAnswer;
  this.user.gender = this.signupForm.value.gender;

  console.log(this.signupForm);
}
```

```js

<div class="row" *ngIf="submitted">
  <div class="col-xs-12">
    <h3>your data</h3>
    <p>Username: {{ user.username }}</p>
    <p>Mail: {{ user.email }}</p>
    <p>Secret Question: {{ user.secretQuestion }}</p>
    <p>Answer: {{ user.answer }}</p>
    <p>Gender: {{ user.gender }}</p>
  </div>
</div>
```

# 189. TD: Resetting Forms

we dit extract the data, we want to reset the form

on our form, signupForm, onthis form, we can call, reset

```js
onSubmit() {
  this.submitted = true;
  this.user.username = this.signupForm.value.userData.username;
  this.user.email = this.signupForm.value.userData.email;
  this.user.secretQuestion = this.signupForm.value.secret;
  this.user.answer = this.signupForm.value.questionAnswer;
  this.user.gender = this.signupForm.value.gender;

  this.signupForm.reset();
}
```

not only empty value, reset the state, the valid, the touch

reactive approach, even more control for our forms

your own validators and much more



# assignment

angular detects the form element

register to as a control, we add ngModel
we tell angular, this input is an active control, of this form angular created automatically

the name email

ngForm the whole form

ngModel just a control

# 190. Introduction to Reactive Approach


template-driven: angular infers the form object from the DOM

reactive: form is created programatically and synchronized with the DOM

is not that difficult

# 191: Reactive: Setup

the form is created in ts code

in app.component.ts

i want to create a new propery, will hold my form. programatically means not from scratch. angular gives us a lot of tools

```js
signupForm: FormGroup
```

needs to be imported from @angular/forms


it contains a lot of classes we will work with

ngForm, was this automatically created wrapper. it was wraping a form group in the end. a form is just a group of controls

for the reactive approach to work, we need to import in app.module.ts

we dont need the forms module, that is for the td

we need the ReactiveFormsModule


```js
providers: [ReactiveFormsModule],
```

# 192. Reactive: Creating a Form in Code

lets create our form

we could initialize immediately

but, lets have it in a method. its a lot of code

OnInit

ngOnInit() here we initilaize my form

before rendering the template

this.signupForm = ??

to what, is of type form group, we need to create a new FormGroup()

we need to pass a js object.

controls, key value pairs

FormGroup({
	'username': new FormControl()
})

during minifications, sure that this code is kept.


FormGroup({
	'username': new FormControl()
})

we can pass a couple of arguments. the first, the initial state

initial value of the contorl, second value, array of validators
third argument, will be potential asynchro validators


FormGroup({
	'username': new FormControl(null)
})

or a string

```js
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '../../node_modules/@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  genders = ['male', 'female'];

  signupForm: FormGroup;

  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    })
  }
}
```

we need to connect this form, with the form from the HTML code

# 193. Reactive: Syncing HTML and Form

app.module.ts

```js
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    ReactiveFormsModule
  ],
```



we need to sync our html inputs, and our own form

angular sees our html form and tryies to generate a js object form

we dont want that, we need directives to overwrite this behaviour

on the form itselfl, property binding, the formGroup directive, hej, please tell my form Group

we need to set up property binding, we need to pass our form as an argument

we pass the property we created

```html
<form [formGroup]="signupForm">
```

now this form is sync with the form we created in ts

we still need to telll angular which controls should be connected to which inputs

we have a new directive

formControlName directive, to tell angular, whats the name of this input in my ts form

```html
<input
	type="text"
	id="username"
	formControlName="username"
	class="form-control">
```

not property binding, i am passing a string, we could use squre brackets, and single quotes

repeat this for the email and for gender input

lets look at it in the browser


its there, next step, submit the form

# 194. Reactive: Submitting the form

we sync our html form with ts form

in the td approach we used the ngSubmit on the form element

we still want to react to this submit event

```html
<form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
```

onSubmit, the difference to the TD, we dont need to get the form via local reference
it wouldn work cuz we are not getting the form through the creation mechanism

we created the form on our own, in the ts

we can simple console log the singup form

whatever you set up as an obejct you pass to the FormGroup, that is what you get

# 195. Reactive: Adding Validation

in the TD, we just added required in the html form

not inthe reactive approach. we are not configuirng the form in the template, we are just synchronizing it with the ts form thanks to the directive formControlName s

FormControl takes more than one argument, the second argument, oyu can specify more validators

Validators object, we have a couple of built in validators, for example require()

but we dont call it, dont execute, no paranthesis, it is a static method. we just want to pass the reference to this method

angular will execute this method whenever it detects that the input of this form changes
it just needs to have a reference on what it should execute at this point of time

```js
  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    })
  }
```


we can add multiple validators, by passing an array of validators.
in email, required, and email validator

```js
  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null, [Validators.required, Validators.email]),
      'gender': new FormControl('male')
    })
  }
```

no we have validation in place

if we inspect our form, the input email, we see the class ng-invalid

thats how we add validation to this approach

#196. Reactive: Getting Access to Controls

we can use this form status to display messages

we access the controls differently

```html
  <input
    type="text"
    id="username"
    formControlName="username"
    class="form-control">
  <span class="help-block">Please enter a valid username!</span>
</div>
```


in the td, we place #username="ngModel", this would not work

we cansimple add ngIf, and get access to this, by accessing our overall form, and we have a get method, we have access to our controls easily, we can pass the control name, or the path

the path is name, because we have just a level of nesting

```html
<span 
  class="help-block"
  *ngIf="signupForm.get('username')"
>Please enter a valid username!</span>
```

this gives access to the contorl, then we ask if its valid or not with .valid

```html
<span 
  class="help-block"
  *ngIf="!signupForm.get('username').valid && signupForm.get('username').touched"
>Please enter a valid username!</span>
```

repeat this for the email

and for the overal form,before the submit btn

```html
<span 
  class="help-block"
  *ngIf="!signupForm.valid && signupForm.touched"
>Please enter a valid data!</span>
<button class="btn btn-primary" type="submit">Submit</button>
```

using form state, this css classes, are still added, we can still go

```css
input.ng-invalid.ng-touched {
	border: 1px solid red
}
```


we access the controls with the get method helper

# 197. Reactive: Grouping Controls

get also takes the path

we could have a nested form

user name nad email, in a form group, this is another FormGroup. its not jus tthe overall FormGroup

```js
  ngOnInit() {
    this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, Validators.required),
        'email': new FormControl(null, [Validators.required, Validators.email])
      }
      ),
      'gender': new FormControl('male')
    })
  }
```

now we have a nested form

we need to reflect this in our html, now our form breaks

cannot find contorl with the name username

in the overall form, there is no such control, is nested in the userData group

in form html, wrapp the elements in a div

formControlName to tell angular which property of our ts code relis on wich input in our ts form

formGroupName to tell the same for a formGroup

```html
<form [formGroup]="signupForm" (ngSubmit)="onSubmit()">

       <div formGroupName="userData">
         <div class="form-group">
```




almost working. valid can not be read on null

the gets!

```js
*ngIf="!signupForm.get('username').valid && signupForm.get('username').touched"
```

userData.username

you structure your path, separated by dots

```html
*ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched"
```

now the form is working again

nesting inside of our form, formGroups

its all about keeping forms in sync

# 198. Reactive: Arrays of Form Controls (FormArray)

another nice future

add a new area in our form, below radio btns
new did, allow the user to dynamically add form controls

allow the user to allow his hobbies

button type="button", not submit, so that the button does not accidentally submit the form
i want t

```html
<div>
  <h4>Your Hobbies</h4>
  <button class="btn btn-default" type="button">Add Hobby</button>
</div>
```

when the user clicks the btn, i want to dynamically create a control in my form here

i want to add this control in an array of controls, because he can have multiple hobbies

click event listener


```html

<div>
  <h4>Your Hobbies</h4>
  <button 
    class="btn btn-default" 
    type="button"
    (click)="onAddHobby()"
  >Add Hobby</button>
</div>
```

add themethod in ts

i need to add add a new type of control in my ts form

```js
ngOnInit() {
  this.signupForm = new FormGroup({
    'userData': new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null, [Validators.required, Validators.email])
    }
    ),
    'gender': new FormControl('male'),
    'hobbies': new FormArray([])
  })
}
```

i name it hobbies, this should be an array

the tpe is new FormArray

it holds an array of controls, in this array, you can initialize with a newFormContorl, or leave it empty

onAddHobb

```js
this.signupForm.get('hobbies')
```

thats how we access, at the end its just a control

(<FormArray>this.signupForm.get('hobbies'))

tell angular this part is a FormArray


(<FormArray>this.signupForm.get('hobbies')).push()

we store the control in a constant, and the hobby, the user should enter

```js
onAddHobby() {
  const control = new FormControl(null);
  (<FormArray>this.signupForm.get('hobbies')).push();
}
```

the control first with null default

i want to add a validator though

now i can push my control in my array of controls

```js
onAddHobby() {
  const control = new FormControl(null, Validators.required);
  (<FormArray>this.signupForm.get('hobbies')).push(control);
}
```

with this, we are able to create controls but we dont see them

we need to synnc with the html code

on the outer div, add a directive, formArrayName

the name was hobbies

this tells angular that somewhere in this div
our formArray will leave

```html
<div formArrayName="hobbies">
  <h4>Your Hobbies</h4>
```

we add a form-group with an input, which allows the user to enter a hobby

```html
<div formArrayName="hobbies">
  <h4>Your Hobbies</h4>
  <button 
    class="btn btn-default" 
    type="button"
    (click)="onAddHobby()"
  >Add Hobby</button>
  <div class="form-group">
    <input type="text">
  </div>
</div>
```

i want to loop through all the controls that are in thsi array

ngFor, loop thorugh the controls in hobbies array

i also want to extract the index in every iteration

i need this, to assign this input to one of this dynamic created controls

on this input, i need to add formControlName!

we need to sync this input, with the dynamic created input

```html
<div 
  class="form-group"
  *ngFor="let hobbyControl of signupForm.get('hobbies').controls; let i = index"
>
  <input type="text">
</div>
```



the name will be the index of this array

```html
<input type="text" class="form-control" [formControlName]="i">
```


i need to tell ts that is is of type FormArray, to not get an error

# 199. Reactive: Creating Custom Validators

adding our own validators

we used built-in ones. they are enough

some usernames, we dont want the user to use

ts, new properties

```js
forbiddenUsernames = ['Chris', 'Anna'];
```

create my own validator

a validator, is just a function, which gets executed when it checks the validity of the control form. it checks it whenever you change this control

lets add a method, forbiddenNames()

neds to recieve an argument, the control it should check

it needs to return sth. this sth, should be a js object.it should have any key


```js
forbiddenNames(control: FormControl): {[s: string]}
```

it should have any key, interpreted as a string.
thats how we declare it, more important, it has to have the value, the boolean

```js
  forbiddenNames(control: FormControl): {[s: string]: boolean}
```

this function should return

{nameIsForbidden: true}

the key name is upto us

we can add the logic in this function

check if the value of the control is inside the array forbiddenNames

```js
  forbiddenNames(control: FormControl): {[s: string]: boolean} {
    if (this.forbiddenUsernames.indexOf(control.value)) {
      
    }
  }
```

if this is the case, i want to return an object, where i say

nameIsForbidden is true, in the other case, iwant to return null

if validation is succesfull, you have to pass nothing or null. you should not pass the object with false.

it should be null or we omit the return

```js
forbiddenNames(control: FormControl): {[s: string]: boolean} {
  if (this.forbiddenUsernames.indexOf(control.value)) {
    return {'nameIsForbidden': true};
  }
  return null;
}
```

this is how we tell angular that our formcontrol is valid

we created our form validator

it recives the control, it returns an object with the error code

we need to add this, in the username, now an array of validators


```js
'username': new FormControl(null, [Validators.required, this.forbiddenNames]),
```

error

```js
Cannot read property 'forbiddenUsernames' of undefined
```

has sth to do with js handles this

in forbiddenNames, this, who is calling this forbiddenNames. not the class, angular is calling it when it checks the validity

at this point of time

this will not refer to our class

to fix this, i need to bind this, to our class

```js
'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
```

the good old trick js to make sure we refer to what we want

```js
if (this.forbiddenUsernames.indexOf(control.value)) {
```

this returns minus one is always true

check if it nots equal to -1

```js
forbiddenNames(control: FormControl): {[s: string]: boolean} {
  if (this.forbiddenUsernames.indexOf(control.value) !== -1) {
    return {'nameIsForbidden': true};
  }
  return null;
}
```

# 200. Reactive: Using Error Codes

this form is invalid, with username anna

we see the errors property in our form

errors: null

if we see our controls, userdata, and then contorls, username, errors: nameIsForbidden: true

here is where angular add the errors. on the individual controls

we can take advantage of this

in the html

our current check is not enough, we just check if its valid

```html
<input
  type="text"
  id="username"
  formControlName="username"
  class="form-control">
<span 
  class="help-block"
  *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched"
>
  <span *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched" class="help-block">
    This name is invalid
  </span>
  <span *ngIf="signupForm.get('userData.username').errors['required']">
    This field is required
  </span>
</span>
````

we check the errors, for every control, the key, we check if we have it in the errors array, and we show a sentence

# 201. Reactive: Creating a Custon Asnyc Validator

we check if the name is invalid

you may need to reach a web server to check this, this is an asyncronous operation, the response is not coming back instantly

we need async validators, which are able to wait for a response, returning true or false

in the appcomponent

forbiddenEmails, takes control as an argument, we need to return sth

this will be a promise, which wraps anything, or an observable

```js
forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {}
```

two contracts, which handles async data.

here, i want to create a promise, which returns as anything, as all promises, recieves a function, with resolve and reject, as argument, we can execute in that function

in that function i want to set a timeout

after 1500 i want to return a response, to simulate we reach a server

the function, an anonimous, i just check if control.value === 'test@test.com'

if this is the case, validation fails, i will return an object with a key value pair, with an error 
we are in a promsie, we dont return, we resolve

!!! learn promises

if we pass the check
i will resolve null

then, we need to return this promise in the end

```js
forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {
  const promise = new Promise<any>((resolve, reject) => {
    setTimeout(() => {
      if (control.value === 'test@test.com') {
        resolve({'emailIsForbidden': true});
      } else {
        resolve(null);
      }
    }, 1500);
  });
  return promise;
}
```

now we can add it to the email

we dont add it in the normal array of validators, we add it in the third argument

this.forbiddenEmails

```js
'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails)
```

it changed to ng-pending, then to ng-valid

# 202. Reactive: Reacting to Status or Value Changes

the status of the input switched
invalid - pending - valid

there is a forms state that we can track in general

in ngOnInit

on my signUpform, on each control, we have two observables we can listen to, statusChanges, and valueChanges

we can subscribe, first argument, we pass a callback which is executed when new data arrives

```js
this.signupForm.valueChanges.subscribe(
  (value) => console.log(value)
);
```

with every keystroke

i get a new object, the value of the form

we also have statusChanges, now we recieve the status

```js
this.signupForm.statusChanges.subscribe(
  (status) => console.log(status)
);
```

we see invalid - pending - valid

two nices observables, to watch what happens in our form or an individual control

# 203. Reactive: setting and patching values

not only can we listen to the updates on our form
we can also update the form on our own

setValue and patchValue 

in ngOnInit

setValue, pass the js object we created before

```js
this.signupForm.setValue({
  'userData': {
    'username': 'Max',
    'email': 'max@test.com'
  },
  'gender': 'male',
  'hobbies': []
});
```

our form is prepopulated with the values

we have also patchValue, if we want to add just a part of the form

```js
this.signupForm.patchValue({
  'userData': {
    'username': 'Anna',
  },
});
```

we can reset the form

```js
onSubmit() {
  console.log(this.signupForm);
  this.signupForm.reset();
}
```

we can pass an object to the reset method to not delete the radio buttons


# assignment

create your own validators bundle

create new file

custom-validators.ts

```js
import { FormControl } from "../../node_modules/@angular/forms";

export class CustomValidators {
	static invalidProjectName(control: FormControl): {[s: string]: boolean} {
		if (control.value === 'Test') {
			return {'invalidProjectName': true};
		}
		return null;
	}
}
```

in ProjectName

```js
[Validators.required, CustomValidators.invalidProjectName]
```

we need to import it

```js
import { FormControl } from "../../node_modules/@angular/forms";
import { resolve } from "../../node_modules/@types/q";

export class CustomValidators {
	static invalidProjectName(control: FormControl): {[s: string]: boolean} {
		if (control.value === 'Test') {
			return {'invalidProjectName': true};
		}
		return null;
	}

	static asyncInvalidProjectName(control: FormControl): Promise<any> | Observable<any> {
		const promise = new Promise((resolve, reject) => {
			setTimeout(() => {
				if (control.value === 'Test') {
					resolve({'invalidProjectName': true});
				} else {
					resolve(null);
				}
			}, 2000);
		});
		return promise;
	}
}
```

i can attach this in my validators, as a third argument

```js
CustomValidators.asyncInvalidProjectName
```













































































































