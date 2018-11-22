86_assignment.md

1) we have a fetch listener
which strategy is this?



```js
// dynamic cache
self.addEventListener('fetch', function(event) {

  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        if (response) {
          console.log("response");
          console.log(response);
          return response;
        } else {
          console.log("not rsponse")
          return fetch(event.request)
            .then(function(res) {
              return caches.open(CACHE_DYNAMIC_NAME)
                .then(function(cache) {
                  cache.put(event.request.url, res.clone());
                  return res;
                });
            })
            .catch(function(err) {

            });
        }
      })
  );
});
```

cache fallback to network strategy (with dynamic caching)



2) network only strategy, clear storage

QUESTION!!!


```js
self.addEventListener('fetch', function(event) {
  // The respondWith() method of FetchEvent prevents the browser's default fetch handling, and allows you to provide a promise for a Response yourself.
  event.respondWith(
    fetch(event.request)
      .catch(function(err) {
        caches.match(event.request)
      })
  )
});
```

what does this mean?

we intercept the request, we respond with: first we fetch to the internet, if it works, awesome, if not, we try to get it from the cache right?

from the documentation, this code does the same?

it looks more clean to me. could you comment pros and cons?

```js
addEventListener('fetch', event => {
  // Prevent the default, and handle the request ourselves.
  event.respondWith(async function() {
    // Try to get the response from a cache.
    const cachedResponse = await caches.match(event.request);
    // Return it if we found one.
    if (cachedResponse) return cachedResponse;
    // If we didn't find a match in the cache, use the network.
    return fetch(event.request);
  }());
});
```


with dynamic cache

```js

self.addEventListener('fetch', function(event) {
  // The respondWith() method of FetchEvent prevents the browser's default fetch handling, and allows you to provide a promise for a Response yourself.
  event.respondWith(
    fetch(event.request)
      .then(function(res) {
        return caches.open(CACHE_DYNAMIC_NAME)
          .then(function(cache) {
            // res can be used just once.
            cache.put(event.request.url, res.clone());
            return res;
          })
      })
      .catch(function(err) {
        caches.match(event.request)
      })
  )
});
```

offline is not working




2) max's solution

we just use fetch, directly go to the network

```js

self.addEventListener('fetch', function(event) {
  // The respondWith() method of FetchEvent prevents the browser's default fetch handling, and allows you to provide a promise for a Response yourself.
  event.respondWith(
    fetch(event.request)
  )
});
```

offline does not work, we are not fetching never from the cache

this is task two solved, a network only strategy


















3) same for cache only
3) max solution

```js
// cache only
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(res) {
        return res;
      })
  )
});
```

we only return stuff from the cache. if is not in the cache, we can't use it

the dynamic things fail. only the static things are loaded

i only reach out to the cache.

we just have the static cache, stored in the installation

4) max's solution

network, cache fallback strategy

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
      .catch(function(err) {
        return caches.match(event.request)
      })
  )
});
```

first to the net, if no internet, we see if we have the request in the cache

network fall back to cache

we are not doing dynamic cache

downside is, if we have a network, that is too slow, and then it cancells, we got to the cache to late

therefore, we can add a then

we open the dynamic cache, i chain

i want to put my event rquest url, and store the response

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
      .then(function(res) {
        caches.open(CACHE_DYNAMIC_NAME)
          .then(function(cache) {
            cache.put(event.request.url, res.clone());
            return res;
          })
      })
      .catch(function(err) {
        return caches.match(event.request)
      })
  )
});
```

reload two times, go offline

in dynamic cache, we store the dynamic

5) code also in js

cache, then network

in main.js

```js
var url = 'https://httpbin.org/ip';
var networkResponseReceived = false;

fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkResponseReceived = true;
    console.log(data.origin);
    box.style.height = (data.origin.substr(0, 2) * 5) + 'px';
  });

if ('caches' in window) {
  caches.match(url)
    .then(function(res) {
      if (res) {
        return res.json();
      }
    })
    .then(function(data) {
      console.log('from cache', data);
      if (!networkResponseReceived) {
        box.style.height = (data.origin.substr(0,2) * 20) + 'px';
      }
    })
}
```

thats nice, we access the cache from main.js, but we want to add dynamic cache in our sw

```js
// dynamic caching for cache, then network strategy
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open(CACHE_DYNAMIC_NAME)
      .then(function(cache) {
        return fetch(event.request)
          .then(function(res) {
            cache.put(event.request.url, res.clone());
            return res;
          })
      })
  )
});
```

```js
  return res;
```
this is for the fetch request coming from the main.js
we need to give back something

6) routing, url parsing


cache, then network, cache with network fallbac, cache only

all three of them in this project. if elsif , parse different urls

we want first "cache, then network"

which kind of request will this make sense?

https://httpbin.org, in the main.js

this block, is the cache, then network

```js
var url = 'https://httpbin.org/ip';
var networkResponseReceived = false;

fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkResponseReceived = true;
    console.log(data.origin);
    box.style.height = (data.origin.substr(0, 2) * 5) + 'px';
  });

if ('caches' in window) {
  caches.match(url)
    .then(function(res) {
      if (res) {
        return res.json();
      }
    })
    .then(function(data) {
      console.log('from cache', data);
      if (!networkResponseReceived) {
        box.style.height = (data.origin.substr(0,2) * 20) + 'px';
      }
    })
}
```

we should look for the url in the sw too

in sw.js, i will just use this

i just want to store logic, for this url

so here

```js
// dynamic caching for cache, then network strategy
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open(CACHE_DYNAMIC_NAME)
      .then(function(cache) {
        return fetch(event.request)
          .then(function(res) {
            cache.put(event.request.url, res.clone());
            return res;
          })
      })
  )
});
```

if we can identify the
if eventrequest url if we can identify the string, in the rquest url

if we found sth >- 1

```js
// dynamic caching for cache, then network strategy
self.addEventListener('fetch', function(event) {
  if (event.request.url.indexOf('https://httpbin.org/ip') > -1) {

    event.respondWith(
      caches.open(CACHE_DYNAMIC_NAME)
        .then(function(cache) {
          return fetch(event.request)
            .then(function(res) {
              cache.put(event.request.url, res.clone());
              return res;
            })
        })
    )
  }
});
```

else case, imlement the cach or network fallback we had at the beginning

```js
// dynamic caching for cache, then network strategy
self.addEventListener('fetch', function(event) {
  if (event.request.url.indexOf('https://httpbin.org/ip') > -1) {

    event.respondWith(
      caches.open(CACHE_DYNAMIC_NAME)
        .then(function(cache) {
          return fetch(event.request)
            .then(function(res) {
              cache.put(event.request.url, res.clone());
              return res;
            })
        })
    )
  } else {

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
                  });
              })
              .catch(function(err) {

              });
          }
        })
    );

  }
});
```

this makes sure, only for the url, for all other rquest, we try to find response in the cache

if we dont find one, we reach out to the network

also, a "cache only"

it makes sense
for all the things we cache in the installation, the static thing

we dont expect regular updates in this static assets

the things that we want to cache dynamically are the dynamc.css, the dynamic page

they are not part of the static part

we want to make sure, that all the static content, is load with a cache only strategy

```js
var CACHE_STATIC_NAME = 'static-v2';
var CACHE_DYNAMIC_NAME = 'dynamic-v1';

var STATIC_FILES = [
  '/',
  '/index.html',
  '/src/css/app.css',
  '/src/css/main.css',
  '/src/js/main.js',
  '/src/js/material.min.js',
  'https://fonts.googleapis.com/css?family=Roboto:400,700',
  'https://fonts.googleapis.com/icon?family=Material+Icons',
  'https://cdnjs.cloudflare.com/ajax/libs/material-design-lite/1.3.0/material.indigo-pink.min.css'
]
```

```js
  } else if (new RegExp('\\b' + STATIC_FILES.join('\\b|\\b') + '\\b').test(event.request.url)) {
    event.respondWith(
      caches.match(event.request)
    )
  } else {
```


now this files are only loaded from the cache

some things are failing

the way the else. if works, wrong

right answer

```js
function isInArray(string, array) {
  for (var i = 0; i < array.length; i++) {
    if (array[i] === string) {
      return true;
    }
  }
  return false;
}

// dynamic caching for cache, then network strategy
```

```js
  } else if (isInArray(event.request.url, STATIC_FILES)) {
    event.respondWith(
      caches.match(event.request)
    )
  } else {
```













































