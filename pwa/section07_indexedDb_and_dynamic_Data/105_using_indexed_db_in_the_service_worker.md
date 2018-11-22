105_using_indexed_db_in_the_service_worker.md


fetching should happen in the feed.js

if we fetch in the sw, like in cache with network fallback


we need the promise first, to get access to the db

we dont want to copy paste the method

```js
var dbPromise = idb.open('posts-store', 1, function(db) {
  if (!db.objectStoreNames.contains('posts')) {
    db.createObjectStore('posts', { keyPath: 'id' });
  }
})
```

we create a new file

```js
var dbPromise = idb.open('posts-store', 1, function(db) {
  if (!db.objectStoreNames.contains('posts')) {
    db.createObjectStore('posts', { keyPath: 'id' });
  }
});

function writeData() {
  dbPromise
    .then(function(db) {
      var tx = db.transaction('posts', 'readwrite');
      var store = tx.objectStore('posts');
      store.put(data[key]);
      return tx.complete;
    });
}
```


```js
function writeData(st, data) {
  return dbPromise
    .then(function(db) {
      var tx = db.transaction(st, 'readwrite');
      var store = tx.objectStore(st);
      store.put(data);
      return tx.complete;
    });
}
```



back to sw, import utility

```js
importScripts('/src/js/idb.js');
importScripts('/src/js/utility.js');
```

get rid of:

```js
var dbPromise = idb.open('posts-store', 1, function(db) {
  if (!db.objectStoreNames.contains('posts')) {
    db.createObjectStore('posts', { keyPath: 'id' });
  }
})
```

and we can call the write method

```js
var url = 'https://pwagram-50bea.firebaseio.com/posts';
if (event.request.url.indexOf(url) > -1) {
  event.respondWith( fetch(event.request)
    .then(function (res) {
      var clonedRes = res.clone();
      clonedRes.json()
        .then(function(data) {
          for (var key in data) {
            writeData('posts', data[key]);
          }
        });
      return res;
    })

  );
}
```

after this refactor, lets now read the data


















































