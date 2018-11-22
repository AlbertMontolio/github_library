67_implementing_dynamic_caching.md

we have a fetch listener, and we want to cache the response that ocmes back

we have to respond with something in the end

if we do a network request, because is not in the cache yet, we want to make sure that we cache it when it comes back



```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        if (response) {
          return response;
        } else {
          return fetch(event.request);
        }
      })
  );
});
```



we get a res from the actual server, we want to add this response to the cache, dynamically

caches.open() a new cache, the name is up to you
we store it in a separate place

cache.put() i want tto put a new resource
we dont use add, add takes a url, sends a request, and automatically stores the request response key value upair

put, requires you to provide a request a response key value pair

first argument is the event.request.url, the identifier
segond argument is the response,
i want to return the res, and the whole caches.open

we want to give the respons to the html file

the res,


```js
return fetch(event.request)
  .then(function(res) {
    return caches.open('dynamic')
      .then(function(cache) {
        cache.put(event.request.url, res);
        return res;
      })
  });
```


the res is consumed

its empty

responses work like this, you can consume them once. so we consume it in the function(res) already

we should call .clone()
it creates a clone. we store a clone, but we return the original response

we have a setup, that we reach out the network, if we dont find the item in the cache

and then we store it if we got it, and still returning to the original requestor


it's working

holy shit



help is not working, but if we refresh again, and go offline

it is cached in the dynamic one

so in the static, we tell him which to cache
















































