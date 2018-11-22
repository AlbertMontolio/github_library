53_fetch_and_service_workers


the fetch in the sw is triggered when we fech images and so on, but, also now with our newly created fetch

sw.js

```js
self.addEventListener('fetch', function(event) {
  console.log('[Service Worker] Fetching something ....', event);
  event.respondWith(fetch(event.request));
});
```

we can catch or listen to our own fetch



