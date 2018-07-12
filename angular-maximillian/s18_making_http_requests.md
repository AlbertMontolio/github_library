# Section: 18. Making http requests

# 235. Angular 6 and http

Angular 6 is currently the latest version of Angular and it deprecates the Http-access method taught in this module.

What does this mean?

It means that the method still works, still is secure - you can use it! But there is a better Http module to use now: HttpClient.

I added a module (section 23) on that new client months ago, even before Angular 5 was released. You'll meet it later in the course and we'll easily update all our Http calls with the new client there.

So for now, follow along with this module here - the core concepts taught here will still apply (i.e. how it works etc).

And later in the course, we'll revisit this solution and update it to HttpClient.

# 236. Introduction & How Http Requests Work in SPAs

"traditional" web apps vs spas

we dont reload the page upon every request we send

we need to reach out the server from time to time

for example to store some db

we need to cneect to a db, 

or if we want to get some list, or if we want to perform some calculation

connecting to serverse is important

how does it work if we dont send requests

## how to access servers in angular apps

we send a request, but we dong get back a new page

our app is just one page

instead, we senda request, but the response will be send to the very same app

this request is sent via ajax

through the xml http request obect js provides

we dont have to write all the logic for sending this request

angular ships a convient service to make http request

# 237. Example App & Backend Setup

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <input type="text" #serverName>
      <button class="btn btn-primary" (click)="onAddServer(serverName.value)">Add Server</button>
      <hr>
      <ul class="list-group" *ngFor="let server of servers">
        <li class="list-group-item">{{ server.name }} (ID: {{ server.id }})</li>
      </ul>
    </div>
  </div>
</div>

```

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
      name: 'Testserver',
      capacity: 10,
      id: this.generateId()
    },
    {
      name: 'Liveserver',
      capacity: 100,
      id: this.generateId()
    }
  ];
  onAddServer(name: string) {
    this.servers.push({
      name: name,
      capacity: 50,
      id: this.generateId()
    });
  }
  private generateId() {
    return Math.round(Math.random() * 10000);
  }
}
```

simple app

generating all this serveres are nice, but once we leave the app, tehy are gone

instead, we want to store these serveres on the backend, and fetch them for them

we use firebase as a backend

we get a backend out of the box

you can use your own backend of course

the logic from angular 2 perspective will be the same

make sure to sign up
go to your cnsole, create a new project in firebase

udemy-ng-http

this is a dommy project, not dive deeper into firebase

simple backend to use

in the console

on database

in rules, and set red and write both to true

in real data base

```js
{
  "rules": {
    ".read": "true",
    ".write": "true"
  }
}
```


the url to the project is in data

back to angular, lets start building http request and sending them

# 238. Sending requests (Example: Post request)

make some http request
technically is not required

i will do a service for this. you can send http request from everywhere in your app

typically a service centralizes this task

server.service.ts

this is a class, ServerService

this class, does not need a decorator, but here yes. here willl need the @Injectable(), this deco is required if you plan to inject a service in a service

we will inject a service here, the  in angular built in http service. which gives us some methods for sending requests

inject in the constructor the http service, the type is Http

needs to be imported from '@angular/http'




```js
import { Injectable } from '@angular/core';
import { Http } from '@angular/http'

export class ServerService {
	constructor(private http: Http) {

	}
}
```


we add a method, store servers

i want to reach out, send a request, reach out my servers. reach out my backend

i need to get the servers, and then, we can use the

this.http

here we have a couple of methods. each method resembly an http word. which kind of request we send
we can send a post request

withpost, in firebase, we append to any existing elements

a put request would overwrite it

a post request, to store some data


the first argument is the url you want to send

the url, you find in your database

we are not done, a post request, needs to get some data to send with

the data could be our servers

		this.http.post('https://xxxxxxhttpxxxert.firxxxseio.com/')

```js
import { Injectable } from '@angular/core';
import { Http } from '@angular/http'

export class ServerService {
	constructor(private http: Http) {}

	storeServers(servers: any[]) {
		this.http.post('https://xxxxxx.firebaseio.com/', servers);
	}
}
```

we are done? angular using observables behind teh scene, we dont send arequest like this.

this post method, will only create an observable

this observable kind of wraps our configured request, but it is not sending it yet

the reason. for this, since we use an observable , so that we can subscrpe to it and get the response when its ready

we have not subscripe yet

there is no need to send it, cuz no one is listen to it

why would you send it? as long as we dont subscribe, the http rquest is not sent

i will subscripe in the component, not here

if we subscripe, i need to return the observable

```js
storeServers(servers: any[]) {
	return this.http.post('https://udem-ng-http-albert.firebaseio.com/', servers);
}
```

again, its not sending a request yet

app.module.ts to use this service

```js
providers: [ServerService],
```

app.component.ts

inject the service in the constructor

```js
constructor(private serverService: ServerService) {}
```

the goal is to have a btn which allows us to save the servers

app.component.html

```html
<button class="btn btn-primary" (click)="onSave()">Save Servers</button>

```

app.component.ts

```js
onSave() {
  this.serverService.storeServers(this.servers);
}
```



will reach out to the serverService, storeServers, and pass the servers

still, no rquest is being sent, we are passing our servers, the method of the service returns an observable

we should subscribe here.

in the subscribe callback, in the first argument, its executed whenever we recieve some data
we get back teh esponse, lets log it to the console

second argument, error, to fetch any potential errors we may get

```js

onSave() {
  this.serverService.storeServers(this.servers)
    .subscribe(
      (response) => console.log(response),
      (error) => console.log(error)
    );
}
```

there is no need to unsubscribe from this observable

because, we get just one response, angular clears it, once we get this response

we sitll need to do sth in the service

we need to adjust the url of the db cuz we are using firebase

server.service.ts

```js
storeServers(servers: any[]) {
	return this.http.post('https://udem-ng-http-albert.firebaseio.com/', servers);
}
```

we can specify our own endpoint, like /data.json

the .json is important, tells firebase, we are about to work with its db

otherwise we get an error

the data is just a object, name it as you wihs, tehdata will be there

one more thing, in app.module.ts

you have to have the HttpModule in imports!!!

if you add the server to your list,and you click save, you see the servers in your firebase database!

but, if you click again, you add all the servers again

# 239. Adjusting Request Headers

we sent data to the server, the save btn

in our application, with our backend, maybe we need to send some specific headers with the request

our own headers, we generate them with the Headers object angular gives

we can pass a js object, where the key value pairs are the key value pairs of our headers

Content-Type, as a string of cours,e because we have the -

coud set to application/json, this is not necessary, this is the default

and we can pass these headers by adding a third argument, to the post method

we add a js object with the headers


```js
import { Injectable } from '@angular/core';
import { Http, Headers } from '@angular/http'

@Injectable()
export class ServerService {
	constructor(private http: Http) {}

	storeServers(servers: any[]) {
		const headers = new Headers({'Content-Type': 'application/json'});

		return this.http.post('https://udem-ng-http-albert.firebaseio.com/data.json', servers);
	}
}
```


```js
return this.http.post(
	'https://udem-ng-http-albert.firebaseio.com/data.json', 
	servers,
	{headers: headers}
);
```

network tab, if we inspect our request

in response headeres, we see the content-type

lets have a closer look on have to get data back from the server


# 240. Sending GET Requests.

new type of request, work with the data we get back

in my server.services.t

i want to add the getServers methods!

should send a request, or better said, prepare a request, wrap it in an observable

which allows to reach out the backend and get back the data

for this, i should simply return an observable, return what gets created

we will subscripe in the place we use the dta


```js
getServers() {
	return this.http.get();
}
```

we need to use the same url, including the data.json

we need to target our database

we dont need to pass any data, like in the post, we are just getting data


```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json');
}
```

we could do

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json/1');
}
```

whatever you need

we could pass an object, where we configure the request. but we dont need it

in my app.comopnent.html

btn for getting the servers

```html
<button class="btn btn-primary" (click)="onGet()">Get Servers</button>
```

app.comopnent.ts

onGet method, reach out to my server.services, and subscribe again
i want to listen to a response or a potential error

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (response) => console.log(response),
      (error) => console.log(error)
    );
}
```

we see them

```js
"{"-LGyTj8fTKW4_IuEYP-9":[{"capacity":10,"id":1953,"name":"Testserver"},{"capacity":100,"id":9089,"name":"Liveserver"}],"-LGyTvGrNoytMRWFFezQ":[{"capacity":10,"id":1953,"name":"Testserver"},{"capacity":100,"id":9089,"name":"Liveserver"},{"capacity":50,"id":9179,"name":"Albertserver"}],"-LGyW7abNdRLA-_r0EMB":[{"capacity":10,"id":7566,"name":"Testserver"},{"capacity":100,"id":5355,"name":"Liveserver"}]}"
```

the body is a string. it makes sense, data is sent as json

in the post request it was also teh case

we have 4 elements, because we post some esrvers.

with a put request we can overwrite the data

how can we unwrap the data, this json string

we can easily unwrap it, instead of console log in

on the response, i want to get the data in the body property

and then turn it form json to js object

the response is of type Response, this object is imported from @angular/http

```js
import { Response } from '@angular/http'
```

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (response: Response) => {
        const data = response
      },
      (error) => console.log(error)
    );
}
```

it offers the json() method

```js
(response: Response) => {
  const data = response.json()
},
```

it will have a look at our body property, get the data from in there, and turn it into a js object

if we print the data now

lets make sure, that we overwrite existing data, instead of appending them


# 241. Sending PUT request

server.service.ts

```js
storeServers(servers: any[]) {
	const headers = new Headers({'Content-Type': 'application/json'});

	// return this.http.post(
	// 	'https://udem-ng-http-albert.firebaseio.com/data.json', 
	// 	servers,
	// 	{headers: headers}
	// );
	return this.http.put(
		'https://udem-ng-http-albert.firebaseio.com/data.json', 
		servers,
		{headers: headers}
	);
}
```

put request also allows us to send some data

on firebase, this leads to a diffferent behaviour. we dont append anymore, it will overwrite the existing data

how we can now work with the data we get back, how to make transformation processes more convenient, using observables power

# 242. RxJS without rxjs-compat

Don't forget - if you're using Angular (and therefore also RxJS 6+) and you're NOT using rxjs-compat  (npm install --save rxjs-compat  - you may ignore this lecture then, use the code as shown in the videos!), you have to use operators like map()  differently:

Instead of

....map(...) 

use

....pipe(map(...)) 

map also needs to be imported:

Instead of 

import 'rxjs/Rx'; 

use

import { map } from 'rxjs/operators'; 

# 243. Transform Responses Easily with Observable Operators (map())

we are sending a lot of requests to our server

we get back some data

but right now, i am extracting the data in my app.component.ts, in my callback

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (response: Response) => {
        const data = response.json();
        console.log(data);
      },
      (error) => console.log(error)
    );
}
```

we can do better

it would be nice, if we can step in the getServers from the server.service.ts

and do this data extraction here

server.service.ts

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json');
}
```

because maybe we want to call getServeres from different components
like now, we should repeat the code all over our application

better to centralize this

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json')
		.map();
}
```

the map operator will take the old observable and wrap the data we get back, into some tranformed data, and wraped this transformed dta into another observable. in the end we get back an observale, of course, we need one, because we are subscring to it

but the response, the data, the observable submits, will transform in between

for map, we need to import

```js
import'rxjs/Rx'
```

notice previous section


in map, we know, that we get a response which will be of tyye response. we need to import from http

now, i can do the same we did in the app component

we return the data, this is not an observbale, this is just an object

the map operator will take the data, an wrap it in a new obesrvable

```js
import { Http, Headers, Response } from '@angular/http';
```

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json')
		.map(
			(response: Response) => {
				const data = response.json();
				return data;
			}
		);
}
```

our code in app.component.ts is still valid. almost, we are not getting back a response anymore

instead, it will be an array of servers, our data

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (response: Response) => {
        const data = response.json();
        console.log(data);
      },
      (error) => console.log(error)
    );
}
```

to

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (servers: any[]) => console.log(servers),
      (error) => console.log(error)
    );
}
```

we got the same, but now, we are doing the transformation (from json to js obejct) in a central place, and with the map

in the service.servers.ts

next section, how to unpck data from observables


we can reuse this code in multiple places of our app. before, we coudlnt



# 244. Using the Returned Data


just to demosntrat how to transform the data we get

server.service.ts

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json')
		.map(
			(response: Response) => {
				const data = response.json();
				return data;
			}
		);
}
```

demonstrate the power of map

lets loop through the servers of our data

for each server, we want to change the name, set it equal to FETCHED_ + server.name

```js
getServers() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/data.json')
		.map(
			(response: Response) => {
				const data = response.json();
				for (const server of data) {
					server.name = 'FETCHED_' + server.name;
				}
				return data;
			}
		);
}
```

!!! not working !!!

lets output it herein our list

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (servers: any[]) => this.servers = servers,
      (error) => console.log(error)
    );
}
```

# 245. Catchin errors without rxjs-compat

Are you using Angular 6 (and therefore RxJS 6+) and you're NOT using rxjs-compat  (npm install --save rxjs-compat  - you may ignore this lecture then, use the code as shown in the videos!)?

You then have to use the catch()  operator you'll see in the next lecture a bit differently.

Instead of

```js
....catch(error => {
    return Observable.throw(...)
}) 

```


use

```js
....pipe(catchError(error => {
    return throwError(...)
}))
```




And make sure to import it:

Instead of 

```js
import 'rxjs/Rx'; 
```



and 

```js
import { Observable } from 'rxjs/Observable'; 
````


use

```js
import { catchError } from 'rxjs/operators'; 
```



and

```js
import { throwError } from 'rxjs'; 
```

# 246. Catching Http Errors

i want to dive, error handling

server.service.ts

lets remove the json of the get request. for firebase, this is an invalid url

we see now an error

we also get a response from firebase

imagine that you want to catch errors like this

we can add the catch operator.


```js
.map(
	(response: Response) => {
		const data = response.json();
		console.log("data");
		console.log(data);
		for (const server of data) {
			server.name = 'FETCHED_' + server.name;
			console.log(server.name);
		}
		// console.log(data);
		return data;
	}
)
.catch(
	(error: Response) => {
		console.log(error);
	}
);
```

this would not work. we still need to return an observable

we are subscribing to it. in the map we returned one, now we need also

in the catch operator. we are subscribing to it.

return error does not work, for some reason, the catch operator does not wrap our data into an observbale automatically

we need to create one on our own

return Observable.throw(error);


```js
.catch(
	(error: Response) => {
		console.log(error);
		return Observable.throw(error);
	}
);
```

we can transform this error in a more user friendly message

```js
.catch(
	(error: Response) => {
		console.log(error);
		return Observable.throw("something went wrong");
	}
);
```

in app.component.ts

```js
onGet() {
  this.serverService.getServers()
    .subscribe(
      (servers: any[]) => this.servers = servers,
      (error) => console.log(error)
    );
}
```

now an error is just a string


# 247. Using the 'async' Pipe with Http Requests

it helped us to transform dta that we get async and output it in the template

imagine, in our db, in fb, we add a new key, with a value, and we get a url for that

appName: "Http Test"
+ data

we can get this key with the url above

in the app i want to output it


```html
<div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
  <h1>{{ appName }}</h1>
```

in ts

```js
appName = "";
```

in server.service.ts

new method

getAppName()

we could do it in a new service

```js
getAppName() {
	return this.http.get('https://udemdfafa-albert.firebaseio.com/appName.json');
}
```

this generates an observable

typically, we would subscribe to it in the appcomponent

not in this case

i want to set this observable as a value for that property appName

```js
appName = this.serverService.getAppName();
```

now appName is bound to the observable

i will add the map method, to transform the response, 

i simply want to return response.json();

```js
getAppName() {
	return this.http.get('https://udem-ng-http-albert.firebaseio.com/appName.json')
		.map(
			(response: Response) => {
				return response.json();
			}
		);
}
```

i see this in the h1

[object Object]



this makes sense, because our appName property is just an observable. we bind it to whatever getAppName returns

and it returns an observable

in order to unpack it, we normally need to subscripe

but, with the async pipe this is all

```js
<h1>{{ appName | async }}</h1>
```


it takes a myliecson, and then we see the name

the async pipe is subscribing to the observable, hence the request is sent. hence, it is transforemd with the map method. and we display the result, the data we get in the end. which is simply this string

















































