# Section 13: Understanding Observables

# 159. Module introduction

what is an observable?

a data source

in angular project, is an object that we import from a third party package, rxjs

an observable follows a pattern, we have an observable and a observer

in between, we have a stream, a time line

in this time line, we can have some events, emitted by the observable, or data packages

emitted by the obesrvable

the observable could emit data because u triggering it to do so

it can be connect to a button

or, as angular http, is connected to an http request. so when the response returns , the response is emiitted as a data package

the other part is the observer


your code

is the subscribe function you saw

there, you have three ways of handling data packages

you can
- handle data
- handle error
- handle completion

these are the 3 types of datapackages you can recieve

in this hooks, in this boxs, your code gets executed

you can determin what should happen if i recieve a data package, or an error, or when the observable event is comlete

an observable can never complete. for example, a button

we use that to handle asyhncronous task

you dont know when they happen, how long they take. the task

you dont want to wait for the completion of such an http request, it would block your programm, your logic

historically, we used callbacks or promises

obervables is a different approach

angular embraces observables

observablse have a big advantgaes: operators

# 160. Angular 6 & RxJS 6

package.json

we see the version of angular

rxjs

```js
"rxjs": "^6.0.0",
```

```js
    "@angular/animations": "^6.0.0",
```

angular uses rxjs 6

starting with rxjs 6, the syntax differs slightly
not the general features, just the importing

to still use the older way of importing stuff through out the course, we need to install one additional packages

```js
npm install --save rxjs-compat
```
this will unlock all the old import syntaxis in this course

there is a modern input syntax

# 161. Analyzing a Built-in Angular Observable

thre links in the header, link to the home page, load user 1, load user 2

the id updates, depending on which routes are use

i am using an observable, the params observable, to handle the change of these router params

angular doe snot rerender the whole user component, just because the params changed

therefore, it uses observable to have a change to react to this changed id

user.component.ts

```js
export class UserComponent implements OnInit {
  id: number;

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
        }
      );
  }

}
```


subscribe to updated params, then you handle the params in this callback we pass to the subscribe method

we just pass an argument to the subscribe method, this anonymous function

where we handle the params

then we simply extract the id

i am using observable

the sending part, or the recieivng part


the code is the reciving part. we handle the data. the angular i ssend by angular

the links just trigger the sending

this is the observer part

```js
(params: Params) => {
  this.id = +params['id'];
}
```

to be precise, rxjs, this is our subscriber

we could implement more callbacks than just one

the subscribe method always takes three arguments

we could implement an anonymous function which gets executed because of an error

and one which gets executed if the obersable complets



```js
  ngOnInit() {
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
        },
        () => {

        },
        () => {
          
        }
      );
  }
```

the params, will not fail, will not complete

so it does not make any sense to put them

we are subscribing to an observable, which wraps a data source. the data source being coded in angular, which emits a new parameter when we click a link. our click is our trigeering event. angular sets up the observable and then pushes it to give us a new data piece or new data package

then we use it in our subscribe function, with aour subscriber. the subscriber is these 3 methods we pass to the subscribe set up, which is the observer

thats how we used it already

lets ipmlement our own observable, to understand how it really works

# 162. bulding & using a first simple observable

home.component.ts

there are different ways of building an observable

should emit some numbers, ascending, in a fix interval

to create the observable, i need to import Observable, from 'rxjs/Observable'

```js
  ngOnInit() {
    const myNumbers = Observable;
  }
```


we are not creating one yet, but we have helper methods, which wll create one for us

interval() -> creates observable, we simple pass a number, and that will be the ms it should wait between emiting data automatically

new piece of dta every second

for that to work, we need toi import rxjs/Rx

rxjs, has its own logic

```js
import 'rxjs/Rx'
```

```js
  ngOnInit() {
    const myNumbers = Observable.interval(1000);
  }
```

observable is set up, we can subscribe to it

we can pass 3 arguments to subscribe. a callback for handling normal data, errors, the completion of the observable

this one, will not fail, wil not complete

we know, we will get a number, and we can simply console log the num to see it

```js
  ngOnInit() {
    const myNumbers = Observable.interval(1000);
    myNumbers.subscribe(
      (number: number) => {
        console.log(number);
      }
    );
  }
```

the data source is a normal timer

setting up an observable from scratch is complex, thats we use interval

# 163. Building & Using a Custom Observable from Scratch

lets buld one from scratch

i want to create an observable, which will fire after 2s, and after 4s, and which also will fail after 5 s.

new observable, myObservable

```js
  const myObservable = Observable.create();
```

we use the Observable object, but now the create method
it takes a function as an argument, this function should hold the asynchronous code

pass an es6 array function, here we will have the logic
this function recieves an observer, so we import it

```js
  ngOnInit() {
    const myObservable = Observable.create(observer: Observer) => {
    }
  }
```

dont mistake this to the subscriber in the subscription function. the observer we pass here will be our final observer, but we pass it to the these anonymous funciton, which will make up our observable.

baiscally, tell the observer, when he will resieve which data. sth that we need to do

when we then subscribe to it, and use the observer to react to the data, the observer will know when to react, because the observer tells him

this the gneral set. up, normally is hidden, because usually we subscribe to an osbervable which already exists

but if u build one from scratch, you need to build this bridge frm observable to observer

in this callbck function, in the body, we set a normal timeout, after 2 s i want to emit my data package

```js
  ngOnInit() {
    const myObservable = Observable.create(observer: Observer) => {
      setTimeout(() => {

      }, 2000);
    }
  }
```


in the callback, executed after 2 s, i want to use the observer, and call next. next emits a normal data package

next pushes the next data package

here you can pass any data you want. i just want to pass a string ' first package'


```js
  ngOnInit() {
    const myObservable = Observable.create(observer: Observer) => {
      setTimeout(() => {
        observer.next('first package');
      }, 2000);
    }
  }
```

i add more

```js
const myObservable = Observable.create((observer: Observer) => {
  setTimeout(() => {
    observer.next('first package');
  }, 2000);
  setTimeout(() => {
    observer.next('second package');
  }, 4000);
  
});
```

after 5 seconds it should fail

now i call error

```js
    const myObservable = Observable.create((observer: Observer) => {
      setTimeout(() => {
        observer.next('first package');
      }, 2000);
      setTimeout(() => {
        observer.next('second package');
      }, 4000);
      setTimeout(() => {
        observer.error('this does not work');
      }, 5000);
    });
```

lets subscribe to it

set up the subscribe function on my own observable

i will pass all 3 arguments

first argument, being the callback which gets triggered on a normal data package, which doesnot fail

we get a string. i will then simply log the data

the scond callback willl hold the error, i know that error will be a string

completed, we wont recieve any data, we will say completed, this one wont be triggered



```js
  ngOnInit() {

    const myObservable = Observable.create((observer: Observer) => {
      setTimeout(() => {
        observer.next('first package');
      }, 2000);
      setTimeout(() => {
        observer.next('second package');
      }, 4000);
      setTimeout(() => {
        observer.error('this does not work');
      }, 5000);
    });

    myObservable.subscribe(
      (data: string) => { console.log(data); },
      (error: string) => { console.log(error); },
      () => { console.log('completed'); },
    )

  }
```

i need to do, on the obesrver, i need to define which kind of data it will emit, i know its string, because its the data i am passing all the time. first package, soncd package etc

```js
const myObservable = Observable.create((observer: Observer<string>) => {
```

we see the console

this is the data our own observable emits

comment out 

```js
    const myObservable = Observable.create((observer: Observer<string>) => {
      setTimeout(() => {
        observer.next('first package');
      }, 2000);
      setTimeout(() => {
        observer.next('second package');
      }, 4000);
      setTimeout(() => {
        // observer.error('this does not work');
        observer.complete();
      }, 5000);
      setTimeout(() => {
        observer.next('third package');
      }, 6000);
    });
```

we never see the third package

# 164. Unsubscribe!

what happens if we have the first observable, myNumbers, it goes infinetely

```js
const myNumbers = Observable.interval(1000);
myNumbers.subscribe(
  (number: number) => {
    console.log(number);
  }
);
```

if i swicth the page, it stills consoelogs

the home component was destroy in the background

this can be a sevear problem. if the observable has an active subscription, and the observable is never completed automtically

you are creating a memory leak

therefore, you should always make sure, you unsubscribe, if you leave the area of the observable

we should implement OnDestroy cycle hook

```js
export class HomeComponent implements OnInit, OnDestroy {

  constructor() { }

  ngOnDestroy() {

  }
```



in ngOnDestroy
we should unsubscribe

we store the subscriptions in propertsies of our class HomeComponent

```js
export class HomeComponent implements OnInit, OnDestroy {

  numbersObsSubscription: Subscription;
  customObsSubscription: Subscription;

```

```js
this.customObsSubscription = myObservable.subscribe(
  (data: string) => { console.log(data); },
  (error: string) => { console.log(error); },
  () => { console.log('completed'); },
)
```

```js
const myNumbers = Observable.interval(1000);
this.numbersObsSubscription = myNumbers.subscribe(
  (number: number) => {
    console.log(number);
  }
);
```

```js
ngOnDestroy() {
  this.numbersObsSubscription.unsubscribe();
  this.customObsSubscription.unsubscribe();
}
```

the observables built in in angular, clearup themselves automacilcy (unsubscribe)

# 165. Where to learn more

reactivex.io/rxjs

click observable

we saw create, interval

there are many more

subject, many operators you can add to your observable, to transform your data you get back

# 166. Using subjects to Pass AND Listen to Data


subject, is like an observable, but it allows you to push it to emit a new data during your code

how it works

i will create a new service for this

users.service.ts


new property, userActivated, this will be a new subject. should be imported from , rxjs/Subject

```js
import { Subject } from "../../node_modules/rxjs";

export class UsersService {
  userActivated = new Subject();
}
```

this subject allows us to do cool things

first of all, i will provide this service. in app.module.ts

```js
providers: [UsersService],
```

user.component.html

new button, activate

```html
<button class="btn btn-primary" (click)="onActivate()">Activate !</button>
```

user.component.ts

onActivate, i want to use my service, i need to inject it

i dont need to provide here because we provide it in the appmodule

with this, i can use my usersService.userActivated, i can call next!

```js
  onActivate() {
    this.usersService.userActivated.next();
  }
```

we used next in the home comonent,

we created an observbale, by also stablisingh the bridge to the observable

a subect is an observable and a observer at the same time

thats why i can conventially call next here

i can pass a value, the user id

i am pushing a new data package, which contains this id. 

```js
  onActivate() {
    this.usersService.userActivated.next(this.id);
  }
```

in my app.component i wantt o display activated after the user, if the user was activated

i will use string intrpoation, to check if the user was activated

the property does not exist yet

```html
<div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
  <a routerLink="/">Home</a>
  <a [routerLink]="['user', 1]">User 1 {{ user1Activated ? '(activated)' : '' }}</a>
  <a [routerLink]="['user', 2]">User 2 {{ user2Activated ? '(activated)' : '' }}</a>
</div>
```

app.component.ts

```js
export class AppComponent {
  user1Activated = false;
  user2Activated = false;
}
```

implmenet onint

ngOnit, i want to use my service, so i will inject it.

usersService, with that injected, i can now use my userAvicated subject, its observable and observer

we used the observable part, by pushing the new value, 

now lets use the observer part, and subscribe to it

i can use the data, i will get an ide, check if id == 1, then user1Activated to true

check if === 2, then activate user 2

```js
ngOnInit() {
  this.usersService.userActivated.subscribe(
    (id: number) => {
      if (id === 1) {
        this.user1Activated = true;
      } else if (id === 2) {
        this.user2Activated = true;
      }
    }
  );
}
```

now we are using a subject, to have observable and observer in one convenient object

its like the event emitter, eent emitter is built on such a subject

its a good practice to use subjct instead of event emitter.so next time you want to implement cross component communication

consider using a subject instead of the event emitter. next instead of emit, is the function to push a new value,a nd subscribe us to consume it

last but not least, dont forget to unsubscribe

# 167. understanding observable operators

operators rxjs offers

we can find a list in the official documentation

what are operators. rxjs, this operators transform the data you recieveto sth else and still remain in the observable world

home.component.ts

where i subscribe to my numbers

```js
const myNumbers = Observable.interval(1000);
```

i could also chain an operator, new line

chained to the observable, the operator can be used on any observable. import rxjs/Rx

i can add map. what does the map operator do?

maps the data we get back, into a new observable

with any transformation of your choice

it takes a function as an argument, we get the data, a number as we know, it should return the transform data

the return is important

```js
const myNumbers = Observable.interval(1000)
.map(
  (data: number) => {
    return data * 2;
  }
);
```

the subscription still works



since operators simple return new observables, you can of course chian these operators



# 168. RxJS 6 without rxjs-compat

its working because of the package, in package.json

```js
    "rxjs-compat": "^6.2.1",
```

lets remove it from our packages, so its not available anymore

```js
npm uninstall --save rxjs-compat
```


ng serve, fails

```js
ERROR in node_modules/rxjs/Rx.d.ts(1,15): error TS2307: Cannot find module 'rxjs-compat'.
```

we just need to adjust our imports, we need to remember it

in the users.service.ts, where we import

```js
import { Subject } from "rxjs";
```

home.component.ts

```js
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Observable, Subscription } from '../../../node_modules/rxjs';

import 'rxjs/Rx'
import { Observer } from 'rxjs/Rx';
```

to

```js
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Observable, Observer, Subscription } from 'rxjs';
import 'rxjs/Rx'
```

isntead of

```js
import 'rxjs/Rx'
```

we do

```js
import { map } from 'rxjs/operators'
```

also change the way we call the map operator

we dont call map directly to the observable
instead, we use a special method, the pipe method, and we pass map

pipe allows to put more operators

we do that with any operator we use



```js
const myNumbers = Observable.interval(1000)
.pipe(
  map(
    (data: number) => {
      return data * 2;
    }
  )
);
```

interval method, you rarely create observables yourself

interval is imported directly from rxsj, so we can omit Observable

```js
import { Observable, Observer, Subscription, interval } from 'rxjs';
```

```js
  const myNumbers = interval(1000)
  .pipe(
    map(
      (data: number) => {
        return data * 2;
      }
    )
  );
```

ng serve

yeah

# 169. Wrap up








































































































