79_strategy_cache_online.md

page reaches the sw, or the sw intercepts the requests

we have a look at the cache, if we have the resource, we return it to the page, we ignore the network

lets implement a cache only strategy

copy paste the fetch event listener and comment it out

sw.js

```js
self.addEventListener('fetch', function(event) {
event.respondWith(
  caches.match(event.request)
    .then(function(response) {
      if (response) {
        return response;
      }
    })
);
```

we delete the else, in case we dont find the response, that reaches the network


we can delete the if also

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        return response;
      })
  );
});
```
we can delete even the then

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
  );
});
```


we will never use that, except for some particular assets

























