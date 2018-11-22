youtube.md

```js
ng new jokes
```

```js
ng g s data
```



add pwa functionality

```js
ng add @angular/pwa
```

you need:

Name                               Version                  Command to update
     --------------------------------------------------------------------------------
      @angular/cli                       6.1.5 -> 6.2.1           ng update @angular/cli
      @angular/core                      6.1.6 -> 6.1.7           ng update @angular/core
      rxjs                               6.3.1 -> 6.3.2           ng update rxjs



ng update

https://github.com/ReactiveX/rxjs/issues/4070#issuecomment-419041214

```js
Installing packages for tooling via npm.
+ @angular/pwa@0.8.1
updated 1 package and audited 29123 packages in 11.208s
found 12 vulnerabilities (9 low, 3 high)
  run `npm audit fix` to fix them, or `npm audit` for details
Installed packages for tooling via npm.
CREATE ngsw-config.json (441 bytes)
CREATE src/manifest.json (1081 bytes)
CREATE src/assets/icons/icon-128x128.png (1253 bytes)
CREATE src/assets/icons/icon-144x144.png (1394 bytes)
CREATE src/assets/icons/icon-152x152.png (1427 bytes)
CREATE src/assets/icons/icon-192x192.png (1790 bytes)
CREATE src/assets/icons/icon-384x384.png (3557 bytes)
CREATE src/assets/icons/icon-512x512.png (5008 bytes)
CREATE src/assets/icons/icon-72x72.png (792 bytes)
CREATE src/assets/icons/icon-96x96.png (958 bytes)
UPDATE angular.json (3800 bytes)
UPDATE package.json (1414 bytes)
UPDATE src/app/app.module.ts (3037 bytes)
UPDATE src/index.html (1608 bytes)
added 1 package from 1 contributor and audited 29125 packages in 10.258s
found 12 vulnerabilities (9 low, 3 high)
  run `npm audit fix` to fix them, or `npm audit` for details
```


new files

- manifest.json
you control the name, the theme color

- ngsw-config.json

we will use this to cache our api endpoints

we run ng build for production again


```js
ng build --prod
```

we need

```js
npm install http-server -g
```

in dist/jokes

```js
http-server -o
```

if we change sth, we can use sw-update


provided from the sw library

in app.component.ts

```js
import { SwUpdate } from '@angular/service-worker';
```


```js
  update: boolean = false;

  constructor(updates: SwUpdate) {
    updates.available.subscribe(event => {
      this.update = true;
    })
  }
```

or we refresh the browser

```js
constructor(updates: SwUpdate) {
  updates.available.subscribe(event => {
    // this.update = true;
    updates.activateUpdate().then(() => document.location.reload());
  })
}
```

how do we cache an api file ---------------------------------------------

data.service.ts

```js
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  constructor(
    private http: HttpClient
  ) { }

  gimmeJokes() {
    return this.http.get('https://api.chucknorris.io/jokes/random');
  }
}
```

app.module.ts

```js
import { HttpClientModule } from '@angular/common/http';
import { DataService } from './data.service';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production })
  ],
  providers: [DataService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

app.component.ts

```js
import { DataService } from './data.service'
```

```js
ngOnInit() {
  this.data.gimmeJokes().subscribe(res => {
    this.joke = res;
  })
}
```

app.component.html

```js
<!--The content below is only a placeholder and can be replaced.-->
<h1>
  Jokes
</h1>

<p *ngIf="joke">{{ joke.value }} </p>
```


how to import google fonts to cache ---------------------------


index.html

```html
<link href="https://fonts.googleapis.com/css?family=Montserrat" rel="stylesheet">
```

style.css

```css
body, html {
  height: calc(100% - 4em);
}

body {
  background: brown;
  color: white;
  text-align: center;
  font-family: 'Montserrat';
  display: grid;
  align-items: center;
  font-size: 3em;
  padding: 2em;
}
```


we add this entry under resources, after files

ngsw-config.json

```js
{
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "app",
      "installMode": "prefetch",
      "resources": {
        "files": [
          "/favicon.ico",
          "/index.html",
          "/*.css",
          "/*.js"
        ],
        "urls": [
          "https://fonts.googleapis.com/**"
        ]
      }
    },
```

that way, this will be cached by the sw

dataGroups, where your api endpoints will be stored

first name, jokes api
urls,

options, cacheConfig
strategy freshness

caching strategy. will check first the api endpoint, the url, and then fallback to cache

or performance

maxSize:
maxAge
timeout: 5s, with fresshness,


```js
,
  "dataGroups": [
    {
      "name": "jokes-api",
      "urls": ["https://api.cucknorris.io/jokes/random"],
      "cacheConfig": {
        "strategy": "freshness",
        "maxSize": 20,
        "maxAge": "1h",
        "timeout": "5s"
      }
    }
  ]
```


























































