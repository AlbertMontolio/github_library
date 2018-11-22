

fetch will be trigger whenever our application fetches sth

in our index.html ,when we see src=css, src=image etc

its also triggered when we fetch manually, with the fetch method

```js

self.addEventListener('fetch', function(event) {
  console.log('[Service Worker] Fetching something ...', event);
})
```

we can skip waiting in the browser

and

!!!refresh the page you are on!!!

we see all the fetches

we can do more than console log

we can overwrite the data which gets sent back


sw is like a network proxy.

respondwith, with what you wanna response

you can write null, so the page can not be reached

```js
self.addEventListener('fetch', function(event) {
  console.log('[Service Worker] Fetching something ...', event);
  event.respondWith(fetch(event.request));
})
```

later, we will intercept this request to see if we have internet access or not


















