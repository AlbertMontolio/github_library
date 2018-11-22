```js
  "scripts": {
    "start": "http-server -c-1"
  },
```


we add -c-1 cuz , ensure that we don't cache any assets using the normal browser cache

create new sw

where do we put it?

in js folder? no, remember the scope. we want to scope all the pages

therefore, we put it in root folder, public

then, sw applies to all the pages

sw.js

before we put any code inside sw.js, we need to register it

we do this in the normal js code. we could do it in a script in index.html

app.js is perfect, since its required by every page

first, if sw feature is available in the browser.

navigator is the browser

check if navigator has the serviceWorker object

now we access the property, we access register a new sw. we pass the path. slash means we go to the root folder
register tells, hej, this is a special file, nto a normal file to be run now, put it in the background

it returns a promise. we work with asynchronous code

we can put then, the funciton will be executed once the promise is done

app.js

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
    .register('/sw.js')
    .then(function() {
      console.log('Service worker registered!');
    })
}
```



lets see info about the sw in the browser


localhost is the scope, since we put it in the public folder, so, the entire domain

we see activated and running, we can click and see the content











