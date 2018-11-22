109_deleting_single_items_from_the_database.md


utility.js

```js
function deleteItemFromData(st, id) {
  dbPromise
    .then(function(db) {
      var tx = db.transaction(st, 'readwrite');
      var store = tx.objectStore(st);
      store.delete(id);
      return tx.complete;
    })
    .then(function() {
      console.log('Item deleted!');
    })
}
```

```js
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
            writeData('posts', data[key])
              .then(function() {
                deleteItemFromData('posts', key);
              });
          }
        });
      return res;
    })
  );
}
```

this code does not make sense, just to test how to delete




















