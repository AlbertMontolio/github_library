

we have

```js
self.addEventListener('fetch', function(event) {
  event.respondWith(fetch(event.request));
});
```













we use match to see if we find a given resource there

we match a request, request are our keys

```js
cache.add('/src/js/app.js')
```

this calls a server, and stores the response in cache, so we have always a request, not a string

if we find it, we continue

if response (if its not null)

we return the value of the cache. we are intercepting the request

if we dont find it, we continue with the original request

with matcheventrequest we just check if the request is in the cache, if yes, we return it, if not, we fetch it

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

we are online, we rfresh

network,

app.js is fetched from service worker

the promise.js
the fetch.js etc, all of this also from service worker, because they are funnel by our fetch Listener



at the end they are all fetched from the internet
we know that because they have file size attached to them

but one file is missing with the size

because this request was never made to the internet

it's cached through the sw and then its returned from there too

app still does not work, since we are not caching index.html, the localhost, the main file

we fail at the very beginning

lets cache our entire app shell




































