76_Offering_cache_on_demand.md

we want to store in our cache

```js
fetch('https://httpbin.org/get')
```

and the image we use in this card, the sf-boat in images

in the onSaveButtonClicked, we save sth up on user input

we can access the cache storage, the caches object, from our normal js

we can call the same methods

we can open the same cache, but we can't access the variables.

i name it user-requested.

we need to check if browser allows caches api

we open user-requested, if not, we created it

we can add the boat image, and the url



```js
function onSaveButtonClicked(event) {
  console.log('clicked');
  if ('caches' in window) {
    caches.open('user-requested')
      .then(function(cache) {
        cache.add('https://httpbin.org/get');
        cache.add('/src/images/sf-boat.jpg')
      })
  }
}
```

don't forget to change version in sw

we allowed to save assets in cache on demand

save article save articles offline
























