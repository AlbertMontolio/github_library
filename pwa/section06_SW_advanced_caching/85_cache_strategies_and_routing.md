85_cache_strategies_and_routing.md

now, we want to see if the request that we are sending is targetint the help page or the root page


in some cases, i want to return the offline.html, in others not

i can still have a look at the request we got

jw.js

```js
.catch(function(err) {
  return caches.open(CACHE_STATIC_NAME)
    .then(function(cache) {
      return cache.match('/offline.html');
    });
});
```

we to the fallback just for the help page, we just put the folder

```js
.catch(function(err) {
  return caches.open(CACHE_STATIC_NAME)
    .then(function(cache) {
      if (event.request.url.indexOf('/help')) {
        return cache.match('/offline.html');
      }
    });
});
```









































