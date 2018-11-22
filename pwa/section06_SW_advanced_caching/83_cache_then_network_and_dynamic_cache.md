83_cache_then_network_and_dynamic_cache.md

we didn't implement the store fetched data in cache.

the sw reached the network, and return fetch data to page, in case it was after the cache

but we are not storing this information into the cache. we are not doing dynamic cache

in sw.js

we had the a strategy, lets comment it

cache then network

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        if (response) {
          return response;
        } else {
          return fetch(event.request)
            .then(function(res) {
              return caches.open(CACHE_DYNAMIC_NAME)
                .then(function(cache) {
                  cache.put(event.request.url, res.clone());
                  return res;
                })
            })
            .catch(function(err) {
              return caches.open(CACHE_STATIC_NAME)
                .then(function(cache) {
                  return cache.match('/offline.html');
                });
            });
        }
      })
  );
});
```

we use cache then network as a base

i want to implement the cache then network strategy also from the sw side
we did it in the feed.js

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(

  );
});
```

we get rid of the old code

we response the caches.open, which one we open? the dynamic one

in there, i can call then function, with the cache it was opene

return event.request, the fetch request i wanna make
chain event, i want to use the response.
remember, event.request intercepts any request made by my frontend

so this gets intercepted, and we have a res

i want to put it in my cache, cloning it

we need to return the res, so that it reaches the feed.js

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open(CACHE_DYNAMIC_NAME)
      .then(function(cache) {
        return fetch(event.request)
          .then(function(res) {
            cache.put(event.request, res.clone());
            return res;
          });
      })
  );
});
```


app does not work offline. why? next lecture






















