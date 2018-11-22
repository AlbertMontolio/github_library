# Section 17: Using Pipes to Transform Output

# 225. Introduction & Why Pipes are Useful

What are pipes?

pipes are a feature built into angular 2, it allows you to transform output in your template

this is the main purpose of a pipe

it transforms some output.

there are pipes for different types of output, for asnycnrons and sync data

```js
username = 'Max'
```

a property in your componnent

you want to output this username in your comonent

```js
<p>{{ username }}</p>
```

it will be nice if all the output is uppercase

but not in your code, only when you output it

not change the property itself, because maybe you use it throughout your code

you want to change the awy is displayed

we use a pipe. the uppercase pipe. thisis a build in pipe

```js
<p>{{ username | uppercase }}</p>
```

now we see MAX

the pipe transforms the value

we can also buld our own pipes

# 226. Using Pipes

we have

```js
Production Server | medium | Mon Aug 09 1920 00:00:00 GMT+0100 (Central European Summer Time)
stable User Database | large | Mon Aug 09 1920 00:00:00 GMT+0100 (Central European Summer Time)
offline Development Server | small | Mon Aug 09 1920 00:00:00 GMT+0100 (Central European Summer Time)
stable Testing Environment Server | small | Mon Aug 09 1920 00:00:00 GMT+0100 (Central European Summer Time)
```

app.component.html

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <ul class="list-group">
        <li
          class="list-group-item"
          *ngFor="let server of servers"
          [ngClass]="getStatusClasses(server)">
          <span
            class="badge">
            {{ server.status }}
          </span>
          <strong>{{ server.name }}</strong> | {{ server.instanceType }} | {{ server.started }}
        </li>
      </ul>
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
  servers = [
    {
      instanceType: 'medium',
      name: 'Production Server',
      status: 'stable',
      started: new Date(15, 1, 2017)
    },
    {
      instanceType: 'large',
      name: 'User Database',
      status: 'stable',
      started: new Date(15, 1, 2017)
    },
    {
      instanceType: 'small',
      name: 'Development Server',
      status: 'offline',
      started: new Date(15, 1, 2017)
    },
    {
      instanceType: 'small',
      name: 'Testing Environment Server',
      status: 'stable',
      started: new Date(15, 1, 2017)
    }
  ];
  getStatusClasses(server: {instanceType: string, name: string, status: string, started: Date}) {
    return {
      'list-group-item-success': server.status === 'stable',
      'list-group-item-warning': server.status === 'offline',
      'list-group-item-danger': server.status === 'critical'
    };
  }
}
```

this is the project we are starting with

server name, instance tpye, medium large small, and the date

instance type to be all uppercase, and the data is ugly

medium, large, small

to change it

go to the template, to the html file

a pipe, is only responsible for transforming the output

server.instanceType

we use the pipe symbol, henge the name pipes

the uppercase 

```html
{{ server.instanceType | uppercase }} | 
```


without changing the property itself, 
we change the way users can view it

lets fix the dates

we can add the date pipe

```html
{{ server.started | date }}
```

now we have

Aug 9, 1920

# 227. parametrizing pipes

it would be nice if we could configure a pipe

the date may be not the layout, the format, you want to output

on the date, we can add a parameter by adding a colon

```html
date:
```

the date pipe expects to recieve a string, there you set up the date

```js
date: 'fullDate'
```

```html
{{ server.started | date: 'fullDate' }}
```

we pass parameters by simply passing a colon

if we have more params, we add an additional colon

```html
{{ server.started | date: 'fullDate': }}
```

but thats not the case of date

where we can learn more?

# 228. Where to learn more about Pipes

angular.io

under docs, api reference

enter pipe as a filter

you see all the pipes which are bult in to angular

we used uppercasepipe, datepipe

slicepipe, it takes multiple arguments

DatePipe

you can see how you can configure it

or you can format it from scratch, like lower case d, etc.

# 229. Chaining Multiple Pipes

we can combine pipes in angular

date, we also want to make it uppercase

```html
{{ server.started | date: 'fullDate' | uppercase }}
```

from left to right

if i switch position, then it does not work

error, uppercase can not be applied to a date

lets create our own pipe

# 230. Creating a Custom Pipe

creating a new file in your app folder

a pipe that shortens my words

shorten.pipe.ts

this class must habe one special method, to be usable as a pipe

its not necessary, but its good practice to implement a certain interface which requires you to implement this method

its called PipeTransform

```js
import { PipeTransform } from "../../node_modules/@angular/core";

export class ShortenPipe implements PipeTransform {
    
}
```

i get an error, because, we need to provide the transform method

stil, we need, transform, needs to recive the value which it should get ransformed

and a list of arguments., we can ommit it for now

transform needs to return always sth

we return the old value, but shorten

we can use the substr()
we define how long the substr will be, we start at the index 0, and we want to have 10 charcters


```js
import { PipeTransform } from "../../node_modules/@angular/core";

export class ShortenPipe implements PipeTransform {
  transform(value: any) {
		return value.substr(0, 10);
	}
}
```

now to use this pipe

we need to add it to declarations in app.module.ts

like modules and components


```js
@NgModule({
  declarations: [
    AppComponent,
    ShortenPipe
  ],
```

we make this pipe available

now, app.component.html

now we want to use it here, aber how?

in our shorten.pipe.ts

this class, is implementing the interfacew we need to ipmlement

we need to add a special decorator
the pipe decorator

in the pipe decorator we can specify a name for the pipe, but jsust specifiy name


```js
import { PipeTransform, Pipe } from "../../node_modules/@angular/core";

@Pipe({
	name: 'shorten'
})

export class ShortenPipe implements PipeTransform {
  transform(value: any) {
		return value.substr(0, 10);
	}
}
```

app.component.html

```html
{{ server.name | shorten }}</strong> | 
```

```html
Testing En
```

we can improve our pipe, adding a new string, with three dots

check if the string is longer than 10 charc



```js
import { PipeTransform, Pipe } from "../../node_modules/@angular/core";

@Pipe({
	name: 'shorten'
})

export class ShortenPipe implements PipeTransform {
  transform(value: any) {
		if (value.length > 10) {
			return value.substr(0, 10);
		}
		return value;
	}
}
```

improve the pipe in the next lecture

# 231. Parametrizing a custom pipe

we created our first pipe, the shorten pipe

it would be nice if we could allow the user to specify the number of chars at which point we shorten the value

we are using 10 hardcoded

lets recieve a second argument, limit, a number

```js
export class ShortenPipe implements PipeTransform {
  transform(value: any, limit: number) {
		if (value.length > limit) {
			return value.substr(0, limit) + '...';
		}
		return value;
	}
}
```

you allow the user to pass a parameter to the pipe

app.component.html

if we dont pass anything, nothing happens. limit is undefined, we never pass the if

```html
{{ server.name | shorten: 5 }}</strong> | 
```


we can add more arguments

# 232. Example: Creating a Filter Pipe

now i wnt to create a new pipe

lets add anew element above our unorded list

i want to allow the user to filter our service

by the status

app.component.html

```html
<input type="text" [(ngModel)]="filteredStatus">
```

app.component.ts

```js
filteredStatus = '';
```

i want to build a pipe, that filters

add a new pipe, with the cli

```js
ng generate pipe

ng g p filter
```


filter.pipe.ts

```js
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'filter'
})
export class FilterPipe implements PipeTransform {

  transform(value: any, args?: any): any {
    return null;
  }

}

```

in here, i want to allow the user to filter, i wll get an argument, the firt argument, will be what the user enters

i want toimplement soe logic which returns just the elements of the array that match the filters string

value is the array of servers

check if value.length is zero, if its empty

after checking this

i simply want to loop through all my items, in my value, value will be an array.
check if my status matchces the filterString

```js
transform(value: any, filterString: string): any {
  if (value.length === 0) {
    return value;
  }
  for (const item of value) {
    if (item.status)
  }
}
```

but we can write this more generic

we cna pass the to be filtered property as an argument

```js
transform(value: any, filterString: string, propName: string): any {
  if (value.length === 0) {
    return value;
  }
  for (const item of value) {
    if (item[propName] === filterString) {
      
    }
  }
}
```

```js
transform(value: any, filterString: string, propName: string): any {
  if (value.length === 0) {
    return value;
  }
  const resultArray = [];
  for (const item of value) {
    if (item[propName] === filterString) {
      resultArray.push(item);
    }
  }
  return resultArray;
}
```

dont forget to

app.module.ts

import it

```js
@NgModule({
  declarations: [
    AppComponent,
    ShortenPipe,
    FilterPipe
  ],
```

pipes transform your outuptu. the ngfor is part of your output

we can add a pipe there!

```js
*ngFor="let server of servers | filter:filteredStatus:'status'"
```

no servers are displayed by default, because no servers match the criteria

if i type stable, it works

our filter pipe is working as expected

if we want to see all the serveres, 

```js
if (value.length === 0 || filterString === '') {
```

there is one issue

# 233. Pure and Impure Pipes (or: How to "fix" the filter pipe)

it seems to work great

we have a certain issue

if we add a new server

```html
<input type="text" [(ngModel)]="filteredStatus">
<br>
<button class="btn btn-primary" (click)="onAddServer()">Add Server</button>
<hr>
```

```js
onAddServer() {
  this.servers.push({
    instanceType: 'small',
    name: 'New Server',
    status: 'stable',
    started: new Date(15, 1, 2017)
  });
}
```

now, if we add the server, and we filter by stable, we dont see this newly created server

it is there, 

this is not a bug

if we add the server while filtering, we dont see it. but if we remove the filter, its there

the reason is, angular thankfully not rerun our pipe on the data whenever this data changes

as soon as we change the filter word, we update the pipe

changing the input of the pipe, will trigger a recalculation, the pipe being apply again

changing the data dont trigger the pipe

to be prcise: updating arrays or objects does not trigger it

this is a good behaviour, otherwise, angular hsould rerun this pipe whenever any data on the page changes

this would be very bad, a lot of perfomance cost

thats the reason why no filter built in pipe exists

angular team decided no to implement it, because you have a high performance cost if you want to enforce to be udpated , the pipe, even if you are in filter mode

we can force it to work. be aware, the following change, will make sure, that whenever we change data in the page, our pipe will be recalculated

this might be to performance reasons

you can force this pipe to be updated, whenever the data changes, by adding a second property

it scalled, pure, and we set it to false, by default is true

```js
@Pipe({
  name: 'filter',
  pure: false
})
```

now, if we filter by stable, and we add a server, we see it

this can be very bad, bad performance, becarefull
try to avoid this pure: false. usually, if you filter, you already have the data

you dont want to add new data

# 234. Understanding the "async" Pipe


one built in pipe, that does sth different . it helps us handling async datta

to demonstrate how it works, lets say we have a nother property, which says, appStatus

could be offline, whatever

app.component.ts

```js
appStatus = "offline";
```

this should be loaded after 2 seconds


we can use a promise, 





in the callback i just want to set my appStatus

only after two seconds

```js
appStatus = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('stable');
  }, 2000);
});
```

if we try to output this appstatus in the template html

```html
<h2>App Status: {{ appStatus }}</h2>
```

in the browser, we see

App Status: [object Promise]

this is correct, it is an object

after two seconds, we know, this is no longer an object. its a string

we know, but angular dont

angular doesnot watch our object, angular doesnot see if this object transforms to sth else

or it returns us a value

angular just knows, its apromise, iam done

save us performance

thankfully, there is a nice little pipe, we can use here, to make this transformation of this data, easier

we want to output the string

we can add the pipe symbol and the async

```html
<h2>App Status: {{ appStatus | async }}</h2>
```

now is empty, and after two seconds

App Status: stable

async recognizes that this is a promise (it also work for observbales)
after 2 s it recognizes that sth changes, that the promise was resolved, and it willl print this data to the screen

# assignment 8: practicing pipes

two tasks

1- build areverse pipe

there is a reverse method. it only works in real arrays

split the array, by an empty char first, then join it

2 - sort this list,built a sort pipe


lets start with the reverse pipe

ng g p reverse

we dont need the argument

instead of returning null

the sort pipe

ng g p sort

```js
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'sort'
})
export class SortPipe implements PipeTransform {

  transform(value: any, propName: string): any {
    return value.sort();
  }

}
```

in order to be specific about how to sort it, we need to pass some argument

an ordering function! which gets two arguments, a and b

in the function body, we determin how we ewant to compare them

i want to compare them by the name

if a[propName] > b[propName]

in this case, i return 1. means, this is greater, this should come first
otherwise, i return -1

```js
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'sort'
})
export class SortPipe implements PipeTransform {

  transform(value: any, propName: string): any {
    return value.sort((a, b) => {
      if (a[propName] > b[propName]) {
        return 1;
      } else {
        return -1;
      }
    });
  }
}
```

this is the sort pipe, to sort it





































































































