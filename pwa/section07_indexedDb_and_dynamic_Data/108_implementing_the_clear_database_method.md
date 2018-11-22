108_implementing_the_clear_database_method.md


utility.js

```js
function clearAllData(st) {
  return dbPromise
    .then(function(db) {
      var tx = db.transaction(st, 'readwrite');
      var store = tx.objectStore(st);
      store.clear();
      return tx.complete;
    })
}
```


```js
self.addEventListener('fetch', function (event) {

  var url = 'https://pwagram-50bea.firebaseio.com/posts';
  if (event.request.url.indexOf(url) > -1) {
    event.respondWith( fetch(event.request)
      .then(function (res) {
        var clonedRes = res.clone();
        clearAllData('posts')
          .then(function() {
            return clonedRes.json()
          })
          .then(function(data) {
            for (var key in data) {
              writeData('posts', data[key]);
            }
          });
        return res;
      })
    );
  } else if (isInArray(event.request.url, STATIC_FILES)) {
```

we clear the storage before we repopulate it















































