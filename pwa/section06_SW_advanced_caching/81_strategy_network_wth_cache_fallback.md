81_strategy_network_wth_cache_fallback.md

page invokes sw. this tries to reach out the network. if it fails, sw tries to reach out the cache
and then the cache gives the info to the page

we take the best from both worlds. we use the cache only if we dont have network access

we dont take advantage of the faster response of cached things

we will improve this strategy later on, but its a good beginning for the perfect cache strategy

we will use this one, and fine tun it

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

a network first cache strategy:

we fetch sent, we sent event.request as a fetch request

and then we handle any errors.
we dont need the then, if response is successfull, we dont do anything

in catch, we can reach out our caches (our local storage)

now,

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
      .catch(function(err) {

        caches.match(event.request)
          .then(function(response) {
            if (response) {
              return response;
            }
          })
      })
  );
});
```

we dont need to reach to the network in the catch (we didn't reach the network already)

no then

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
      .catch(function(err) {
        caches.match(event.request)
      })
  );
});
```



if i try to go to the network, it fails. if i try to go to the cache and not find anything, it fails
so it be, we tried it. we can't do anything

this strategy is bad in poor connections, because if the connection fails in 60s. we have to wait 60s until we do the catch. until we try to cache

using this strategy, is not that common.

this is not useful for things that the user needs immediately
this may work for assets that the user does not need immediately, you load them on the background

sidenote: you can connect this with a dynamic cache strategy

if we have internet ocnnection, we can open the cache, and put the request we made

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
      .then(function(res) {
        return caches.open(CACHE_DYNAMIC_NAME)
          .then(function(cache) {
            cache.put(event.request.url, res.clone());
            return res;
          })
      })
      .catch(function(err) {
        caches.match(event.request)
      })
  );
});
```























