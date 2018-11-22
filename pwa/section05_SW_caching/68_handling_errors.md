68_handling_errors.md

we have errors, whenever we try to fetch for the assets, if they are not cached

so we add a catch block




sw.js
```js
return fetch(event.request)
.then(function(res) {
  return caches.open('dynamic')
    .then(function(cache) {
      cache.put(event.request.url, res.clone());
      return res;
    })
})
.then(function(err) {

});
```


in app.js, the sw itself its not cached! thats why this is an error








































