84_cache_then_network_with_offline_support.md


our cache is filled with stuff, but if i go offline, page does not work

in the sw.

we dont have any code, where we tryy to fetch sth from the cache

it works fine, if we have internet, since we load directly from the cache

we need to ensure that our app shell, the core files, and also the file that kicks of our cache then network strategy (feed.js)

this files are loaded first

in the fetch listener, we need to fetch which kind of request we are making

we check if we find the url in one of the request that we make

in the other case, we dont want to use cache then networ, we use the old strategy

we need to wrap it in event.respondWith, otherwise we are not responding

```js
self.addEventListener('fetch', function(event) {
  var url = 'https://httpbin.org/get';

  if (event.request.url.indexOf(url) > -1) {
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
  } else {
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
  }
});
```


if do cache then network to the url, and we do the old strategy for the other assets
















