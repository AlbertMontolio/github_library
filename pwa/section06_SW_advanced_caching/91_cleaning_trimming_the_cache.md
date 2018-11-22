91_cleaning_trimming_the_cache.md

general cache management

how can we clean our cach up?
specially the dynamic one

 we have more pages

 sometimes you want to clean up your dynamic cache

 sw.js

 new function

 at the very top

cache you want to trim, maxItems

recursive, stops calling itself, when the condition is no longer true

```js

function trimCache(cacheName, maxItems) {
  caches.open(cacheName)
    .then(function(cache) {
      return cache.keys()
        .then(function(keys) {
          if (keys.length > maxItems) {
            cache.delete(keys[0])
              .then(trimCache(cacheName, maxItems))
          }
      })
    })

}
```

where do we call it?

we could call it in every fetch.

```js
self.addEventListener('fetch', function(event) {
  var url = 'https://httpbin.org/get';
  if (event.request.url.indexOf(url) > -1) {
    event.respondWith(
      caches.open(CACHE_DYNAMIC_NAME)
        .then(function(cache) {
          return fetch(event.request)
            .then(function(res) {
              trimCache(CACHE_DYNAMIC_NAME, 3);
              cache.put(event.request, res.clone());
              return res;
            });
        })
    );
```

3 is very aggresive. we can have more!

```js
return caches.open(CACHE_DYNAMIC_NAME)
  .then(function(cache) {
    trimCache(CACHE_DYNAMIC_NAME, 3);
    cache.put(event.request.url, res.clone());
    return res;
  })
```
















































