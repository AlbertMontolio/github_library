64_adding_and_retrieving_multiple_files_to_from_cache.md



thus far, we are caching one file, the app.js

lets precache all important files

index.html,


```html
self.addEventListener('install', function(event) {
  console.log('[Service Worker] Installing Service Worker ...', event);
  event.waitUntil(
    caches.open('static') // name we like
      .then(function(cache) { // add content to the cache
        console.log('[Service Worker] Precaching App Shell');
        cache.add('/index.html');
        cache.add('/src/js/app.js');
      })
  );
});
```

now, if we do offline, it does not work at

```js
http://localhost:8080
```

but, at

```js
http://localhost:8080/index.html
```

it works! we are offline and we see stuff!!!


we have to add /
as a request, not as a paths!!

we are caching request, url

```js
cache.add('/');
cache.add('/index.html');
cache.add('/src/js/app.js');
```






this cache.add is cool, but we dont want to put one by one

lets use the cache.addAll















