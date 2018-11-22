70_different_cache_versions_cleanup.md


we need to clean up the old cache

in the activate evente, we do the clean up

this only applies, when the user closes all pages, and opens a new app


event.waitUntil();

if we dont wait, we might fetch to an old cache

then we get the keys

i chain then, keyList, is an array of strings, the names of caches

i return Promise.all()

takes an array of promises, and wait to all of them to finish !!!!!!!!!!!!!

when we are done with the clean up

with map, we transform an array of strings into an array of promises

map takes a function, that will be executed on every key

in there, i want to check if the key is equal to the current chache static name

(static-v2)

and also, its not equal to dynamic



delete is another method we can use on the cache storage



```js

self.addEventListener('activate', function(event) {
  console.log('[Service Worker] Activating Service Worker ....', event);
  event.waitUntil(
    caches.keys()
      .then(function(keyList) {
        return Promise.all(keyList.map(function(key) {
          if (key !== 'static-v2' && key !== 'dynamic') {
            console.log('[Service Worker] Removing old cache...', key);
            return caches.delete(key);
          }
        }));
      })
  );
  return self.clients.claim();
});
```

this works, but its pretty demanding to write twice the name

so, lets add a variable on the top







































































