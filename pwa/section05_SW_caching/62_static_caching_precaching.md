62_static_caching_precaching.md

Static Caching at Installation (of the sw)

only we have a new sw, if we change it

its perfect at this time to cache static content

we'll use the cache api

we have index.html

- app.js
- app.css
- image.png

service worker installation

access Cache API

we store this files in cache api

these files can later be fetch

let's implement this static cache of the app

```html
<script defer src="/src/js/material.min.js"></script>
<script src="/src/js/promise.js"></script>
<script src="/src/js/fetch.js"></script>
<script src="/src/js/app.js"></script>
<script src="/src/js/feed.js"></script>
```

```html
  <link rel="stylesheet" href="/src/css/app.css">
  <link rel="stylesheet" href="/src/css/feed.css">
```

```html
  <link rel="stylesheet"
        href="https://cdnjs.cloudflare.com/ajax/libs/material-design-lite/1.3.0/material.indigo-pink.min.css">
```


you don't want to cache everything,

different cache strategies later

in the addEventListener, we want to access the cache api

caches.open, open a new cache in there

aplication, cache storage. one per page
we can have multiple subpages

in a sw, we work with async, so, we addeventlistener, caches.opn, and code continues

this can lead to problems, since you can fetch sth from the cache, but the cache is not ready yet

sw.js

```js
self.addEventListener('install', function(event) {
  console.log('[Service Worker] Installing Service Worker ...', event);
  caches.open();
});
```

we do, it wont finish the installation event, until the caches.open is done


```js
self.addEventListener('install', function(event) {
  console.log('[Service Worker] Installing Service Worker ...', event);
  event.waitUntil(caches.open());
});
```


so, open returns a promise

```js
self.addEventListener('install', function(event) {
  console.log('[Service Worker] Installing Service Worker ...', event);
  event.waitUntil(
    caches.open('static') // name we like
      .then(function(cache) { // add content to the cache
        console.log('[Service Worker] Precaching App Shell');
        cache.
      })
  );
});
```

we can access some methods for the newly created instance of cache
developer.mozilla web api cache


match checks if cache contains a certain key


add array of items

which keys we have in the cache



add -> add new resource. the url we want to send the request to

relative to our root

```js
cache.add('/src/js/app.js')
```



in the browser, application, cache storage, we see our cache named static

we see the path src/js/app.js

if we put offline, the page still fails, cuz we are caching it, but we arenot fetching the cached file























































