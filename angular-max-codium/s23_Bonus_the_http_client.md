# Section 23: Bonus: HttpClient

with angular 4.2 a new http client was released

the new client is an alternative

the old client is perfectly valid

you can still use it, its not deprecated

the new client it uses a more modern api, it supports more advanced features

it supports interceters

# 295. The documentation

as a side note, 

visit angular.

under fundamentals

you can dive super deep

you can always go to the api doc, this common/http section

lets replace the old methods for the new ones from the new http client




# 296. Unlocking

the new client simply give you additional features.

the old one is perfectly fine

lets use thenew client

we first of all need to unlock it

in the app.module.ts

```js
imports: [
  BrowserModule,
  AppRoutingModule,
  // HttpModule,
  HttpClientModule,
  SharedModule,
  ShoppingLlistModule,
  AuthModule,
  CoreModule
],
```

```js
import { HttpClientModule } from '@angular/common/http'
```

now in the data-storage.service.ts, where we use it

```js
private http: Http,
```

to

```js
private httpClient: HttpClient,
```

```js
import { HttpClient } from "../../../node_modules/@angular/common/http";
```

lets start with the put request

```js
return this.http.put(
	'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
	this.recipeService.getRecipes()
);
```

i replace for httpClient, and it has still the put method

the first argument is still the url, the second argument, is still the bodies

```js
return this.httpClient.put(
```

there is non difference so far

same for get request

```js
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
```

the response is not of type Resopnse anymore

instead, we can now be explicit, which kind of data we getting back. we dont need to extract it from the body

by default, the httpclient, will extract the bdoy from the response

```js
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
	.pipe(
		map(
			(response: Response) => {
				const recipes: Recipe[] = response.json();
				for (let recipe of recipes) {
					if (!recipe['ingredients']) {
						console.log(recipe);
						recipe['ingredients'] = []
					}
				}
				return recipes;
			}
		)
	)
```

we extract directly the body, it assumes that we get a json

```js
.pipe(
	map(
		(recipes) => {
```

we can overwrite this behaviour if we need the headers, for example. but not now

the thing is, we know what is inside of recipes, however, 


```js
(recipes: Recipe[]) => {
```

however, we could type like this, but we can take advantage of a new feature of httpclient

typerequest, 

the get method can be used as a generic method. we can tell the httpclient, which type of data we are getting back

```js
this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
	.pipe(
		map(
			(recipes) => {
				for (let recipe of recipes) {
```

now the httpclient knows the type, and ts nows what recipes is
it will be of Recipe[]

remember, we get a json, and we can see there the type. awesome


everything works

how canwe overwrite this defaults?

# 297. Request Configuration and Response


now lets see what can be configure for the request

we have a url, and the data

what happens if we dont get back json data

```js
// this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
```

we can not assign a type, because we dont get json, we dont have any js

we get sth else like a file or text

say that we get text

we need to pass another argument, in the get the second, in the put the third

in the options argument, 

options is a js object, where we define options for the request

we can define the body in the post

more interesting, the headers, in the get request

 the headers: 

we have also the observe properties

it takes a string, we can sett it to response

it will not automatically the body data of the response, it will give us teh whole response

responseType: 'text'

the default is json

if i say text, i g

lets test it

```js
// this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
	.pipe(
		map(
			(recipes) => {
				console.log(recipes);
				// for (let recipe of recipes) {
				// 	if (!recipe['ingredients']) {
				// 		console.log(recipe);
				// 		recipe['ingredients'] = []
				// 	}
				// }
				// return recipes;
				return [];
			}
		)
	)
```

recipes is now the full response

if we fetch data

```js
// this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token, {
	observe: 'response',
	responseType: 'text'
})
	.pipe(
```

now we see the httpresponse

the body is treated as text

its a json, because its what we get, but its actually text

we see the headers


we can observe the body


```js
// this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token, {
	observe: 'response',
	responseType: 'text'
})
	.pipe(
```


it did extract the body, but as text. ugly

alternative for the responseType, blob, for when you download a file

the most common one is json

```js
// this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token)
this.httpClient.get('https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token, {
	observe: 'body',
	responseType: 'json'
})
```

we leave the defaults, although we dont need them


# 298. Requesting Events

we can configure more

sofar, we configure the observe of the response

sometimes you are interested in the different events that are fired in such a request

lets do it for the put request

we pass an object, to set some options

we want to observe events!

data-storage.service.ts

```js
storeRecipes() {
	const token = this.authService.getToken();
	return this.httpClient.put(
		'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
		this.recipeService.getRecipes(), {
			observe: 'events'
		}
	);
}
```


header.component.ts

```js
onSaveData() {
	this.dataStorageService.storeRecipes()
		.subscribe(
			(response: Response) => console.log(response)
		);
}
```

i still expect 

the old response
of the old http client

with events is no longer true

```js
(response: HttpEvent) => console.log(response)
```

this is a generic type, we need

```js
(response: HttpEvent<Object>) => console.log(response)
```

we get an object, of type 0

and then the actual full response, body, ok, etc.

the angular httpClient, knows different types of events

the easier type is number 4

type 4 is the normal response.

and we got an object of type 0, with no additional info

this means, this is the http sent event type

from the docu we can know it

we can check the type

```js
onSaveData() {
	this.dataStorageService.storeRecipes()
		.subscribe(
			(response: HttpEvent<Object>) => {
				console.log(response.type === HttpEventType.)
			}
		);
}
```

we have different events

for exampl,e the sent event

it works

we see true and false

this is a bad way of know which event 

check the docu

we have the user event,the response event, the upload progress event

if we check for the Response event, we see false and true

these are the events we are getting back

```js
onSaveData() {
	this.dataStorageService.storeRecipes()
		.subscribe(
			(response) => {
				console.log(response);
			}
		);
}
```

lets elave the default behaviour

```js
storeRecipes() {
	const token = this.authService.getToken();
	return this.httpClient.put(
		'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
		this.recipeService.getRecipes(), {
			observe: 'body'
		}
	);
}
```

we can listen to this events, sometimes we want to do stuff once the request was sent and we are still waiting 

for the response

we can get in this process and do somehting


# 299. Setting Headers

we learnt about the basics

of sending requests and handling responses

we can even listen to the events, instead of the response

we can set headers

```js
storeRecipes() {
	const token = this.authService.getToken();
	return this.httpClient.put(
		'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
		this.recipeService.getRecipes(), {
			observe: 'body',
			headers: new HttpHeaders()
		}
	);
}
```

if we want to set headers

you can simply chain a methoc call to this newly instantiated object

there are methods for gettng the headers, or setting the headers

there are some headers which are by default always there

since we have no custom headers yet, lets set some

```js
storeRecipes() {
	const token = this.authService.getToken();
	return this.httpClient.put(
		'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
		this.recipeService.getRecipes(), {
			observe: 'body',
			headers: new HttpHeaders().set('Authorization', 'Bearer afkdkfasdfa')
		}
	);
}
```

this will not work with firebase of course

```js
	headers: new HttpHeaders().set('Authorization', 'Bearer afkdkfasdfa').append()
```

we can prepare an object in advanced

```js
const headers = new HttpHeaders().set('Authorization', 'Bearer afkdkfasdfa')

```

and then pass this object


# 300. Http Parameters

what about params, query params

we have one!

```js
'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
```

the auth, this is sth we do write in the url. it works, but if you dont want your url that long

there is a more convenient way of setting query params

we can pass them in the config

HttpParams

we can set and append parameters


if we set, the first element is the key and the second one is the value

```js
return this.httpClient.put(
	'https://ng-recipe-book-albert.firebaseio.com/recipes.json?auth=' + token,
	this.recipeService.getRecipes(), {
		observe: 'body',
	}
);
```

to

```js
storeRecipes() {
	const token = this.authService.getToken();
	return this.httpClient.put(
		'https://ng-recipe-book-albert.firebaseio.com/recipes.json',
		this.recipeService.getRecipes(), {
			observe: 'body',
			params: new HttpParams().set('auth', token)
		}
	);
}
```

this is cleaner

# 301. Progress

lets take a step further

comment out the put, how we sent the request

another way of making the request that allows us to listen to the progress a request and a response has made

this is very useful for uploading and downloading files

it can be used on any request

HttpRequest object, 

this is just a more advanced way of creating a request

we can create one from scratch, not like put and get

this consctructor takes a couple of inputs

the second argument, is the url i want to target

the third argument is the data i want to send

the fourth, the object to configure it

with put and get, look what we spare

```js
storeRecipes() {
	const token = this.authService.getToken();
	// return this.httpClient.put(
	// 	'https://ng-recipe-book-albert.firebaseio.com/recipes.json',
	// 	this.recipeService.getRecipes(), {
	// 		observe: 'body',
	// 		params: new HttpParams().set('auth', token)
	// 	}
	// );
	const req = new HttpRequest('PUT', 'https://ng-recipe-book-albert.firebaseio.com/recipes.json', this.recipeService.getRecipes(), {reportProgress: true})
}
```

in the options

i set a different property, reportProgress

this gives me feedback about the progress of the rquest and response

useful if we are uploading or downloading sth


with that, i also pass, the params, i still need to pass the auth

```js
const req = new HttpRequest('PUT', 'https://ng-recipe-book-albert.firebaseio.com/recipes.json', this.recipeService.getRecipes(), {reportProgress: true, params: new HttpParams().set('auth', token)})
```

we are listening, we are getting information about the progress

nothing happened

we are just creating the request, not executing it

to send it, i need to call my http cilent, there is a request method, where we pass a configured request

```js
storeRecipes() {
	const token = this.authService.getToken();

	const req = new HttpRequest('PUT', 'https://ng-recipe-book-albert.firebaseio.com/recipes.json', this.recipeService.getRecipes(), {reportProgress: true, params: new HttpParams().set('auth', token)})

	return this.httpClient.request(req);
}
```

we get the object of type 0,

the sent event, then we get the type1, and the 3

1 is the upload progress event, 3 is the download progress event

object 0
object type 1
headerresponse

object type 3

httpresponse

total is the amount of data which needs to be transfrared, loaded, the data that was actually transformed

we can devide the values and get a progress bar

# 302. Interceptors

one nice feature, 

interceptors. we are always sending the request and it just leaves our application like that

its a typically usecase that you want to do sth with every request your app is sending though

for example, in our application, set this quary params with auth and token

we have to do this on every request we send, manually!

in the put, in the get

it would be nice to send a request just with the url

not fetch teh token here, instead

send a request without it

we could have some place in our app which checks every outgoing request and manipulates it

we could manipulate the headers for authorization or maniuplate the params

we are sending

we can do this with an intersator

we create a new file for this, in the sahred folder

auth.interceptor.ts


```js
import { HttpInterceptor, HttpHandler } from "../../../node_modules/@angular/common/http";
import { HttpRequest } from "../../../node_modules/@types/selenium-webdriver/http";

export class  AuthInterceptor implements HttpInterceptor {
   intercept(req: HttpRequest<any>, next: HttpHandler) 
}
```



the httphandler is an object which will give you a special method you can execute to let your request continue his journey

if you dont give this, teh request will not go

the intercept method returns an observable, cuz angular uses observable to wrap the http requests

observable is a generic type, it gives us back an httpevent

and which also is a generic type, it could return any event, upload event, download event, sent event


```js
import { HttpInterceptor, HttpHandler } from "../../../node_modules/@angular/common/http";
import { HttpRequest } from "../../../node_modules/@types/selenium-webdriver/http";

export class  AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		
  } 
}
```

the next method

```js
import { HttpInterceptor, HttpHandler, HttpEvent, HttpRequest } from "../../../node_modules/@angular/common/http";
import { Observable } from "../../../node_modules/rxjs";

export class  AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
  	console.log("intercepted", req)
		return next.handle(req);
  } 
}
```

we call the handle method, we pass the request we got to it.

it will let the request continue its journey




with this setup, we are intercepting any http request we want to send, we just let them continue, 

we are log in

nothing is happening, just creating this file and this class, angular does not add this automatically

we have to tell angular to use this interceptors

weneed to provide. its interjected into the app by angular

for the full app

in the core module!

core.module.ts

we dont add, AuthInterceptor in the providers, as usual

instead, different way of providing things

we pass a js object, 

we need a special token, angular needs to understand

```js
providers: [
		ShoppingListService, 
		RecipeService, 
		DataStorageService, 
		AuthService, 
		AuthGuard,
		{provide: HTTP_INTERCEPTORS}
	]
```

what you want to provide

special placeholder or token, angular understands, http_interceptors

all uppercase, this needs to be imported

this tells angular, what wewill provide here, is an http interceptor so please add it to the pipeline of intercetors you are aware of and you send every outgoing request through

ng will do it automatically but only if we registere it

we need to say which intercetptor it is
we use the useClass


```js
providers: [
	ShoppingListService, 
	RecipeService, 
	DataStorageService, 
	AuthService, 
	AuthGuard,
	{provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true}
]
```


multi, you can have multi of these

we try it

we get the intercepted log

and we get the normal response data

if i dont 


auth.interceptor.ts in shared

```js
export class  AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		console.log("intercepted", req);
		// return next.handle(req);
		return null;
  } 
}
```

it does not work

i get an error, when i subscribe to my observable, i subscribe to null.

# 303. Modifying Requests in Interceptors

we are not really using the interceptor

lets modify the request

bydefault, the request are immutable. you can't edit them



what you want to do, you want to clone a request before you send it


copiedReq = req.clone();

exact copy of the incoming request

now you can edit this copy

copiedReq.we can read headers, params, etc. only read

you can not write. it sblock, also for the clone

the clone function, we can pass a config object

we can update the request, by changing the headers config etc.

you can set the headeres, to thenew headers you want to set

the horiginal request headers as starting pont, by overwriting, or appending more

```js
export class  AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		console.log("intercepted", req);
		const copiedReq = req.clone({headers: req.headers.append('', '')});
		return next.handle(req);
  } 
}
```

or, instead of append, set

i dont want to set headers, i want to set params

```js
const copiedReq = req.clone({params: req.params.set('auth', '')});
```

the token was retrieve from the auth service

no problem, we can inject services in intersectors

auth.interceptor.ts

```js
@Injectable()
export class  AuthInterceptor implements HttpInterceptor {
	constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		console.log("intercepted", req);
		const copiedReq = req.clone({params: req.params.set('auth', this.authService.getToken())});
		return next.handle(copiedReq);
  } 
}
```

dont foreget to remove the params auth in the data-storage.service.ts

```js
this.httpClient.get<Recipe[]>('https://ng-recipe-book-albert.firebaseio.com/recipes.json', {
```

no ?auth = token

how can we intersesct incming responses?

right now, we are just intersecitn requests

# 304. Modifying Requests in Interceptors

lets create a new interceptor in shared

```js
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from "../../../node_modules/@angular/common/http";
import { Observable } from "../../../node_modules/rxjs";

export class LoggingInterceptor implements HttpInterceptor {
	intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		return next.handle(req);
	}
}
```

we dont want to do anything with the equest, we just want to intercept the rsponse

this actually gives us an observable

so now we can just subscribe to this. but that would consume it, so just do

we can call the do ooperator


```js
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from "../../../node_modules/@angular/common/http";
import { Observable } from "../../../node_modules/rxjs";
import 'rxjs/add/operator/do'

export class LoggingInterceptor implements HttpInterceptor {
	intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		return next.handle(req).do();
	}
}
```

this simply allows us to execute some code on any data which goes through the observable without consuming it. thats the difference with subscribe.

we dont consume it, we just have some inbetween step, with do

indo, i know that i have some event, that i can handle here

 

using rxjs 6+?

if youre not using rxjs-compat, this then is

```js
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
```

```js
next.handle(req).pipe(tap(...))
```

do() was renamed to tap()



```js
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from "../../../node_modules/@angular/common/http";
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';


export class LoggingInterceptor implements HttpInterceptor {
	intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
		return next.handle(req).pipe(tap(
			event => {
				console.log("logging interceptor", event)
			}
		));
	}
}
```

we have to provide our interceptor!

to add it to the interceptor chain


core.module.ts


```js
providers: [
	ShoppingListService, 
	RecipeService, 
	DataStorageService, 
	AuthService, 
	AuthGuard,
	{provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true},
	{provide: HTTP_INTERCEPTORS, useClass: LoggingInterceptor, multi: true}
]
```

which one gets executed first

if we have multiple interceptors, the order matters

in the logging interceptor, once we got the response

it worked!

# 305. Wrap up






















































