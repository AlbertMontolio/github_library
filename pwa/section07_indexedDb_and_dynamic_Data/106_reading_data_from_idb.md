106_reading_data_from_idb.md


readAllData, we need the st where we want to read data

we create a transaction, we define which store we want to open, the type of transaction

open the store, tx.objectstore(st)

then we getAll

```js
function readAllData(st) {
  return dbPromise
    .then(function(db) {
      var tx = db.transaction(st, 'readonly');
      var store = tx.objectStore(st);
      return store.getAll();
    })
}
```


in feed.js, we need to check if we have access to indexedDb, maybe the browser is old

we will change this now, no cache, now indexedDb!

```js
if ('caches' in window) {
  caches.match(url)
    .then(function(response) {
      if (response) {
        return response.json();
      }
    })
    .then(function(data) {
      console.log('From cache', data);
      if (!networkDataReceived) {
        var dataArray = [];
        for (var key in data) {
          dataArray.push(data[key]);
        }
        updateUI(dataArray)
      }
    });
}
```

```js
if ('indexedDB' in window) {
  readAllData();
}
```

so that we get access to this method, we need to import utiliy.js in our index.html, before feed.js!

getAll is a promise

```js

if ('indexedDB' in window) {
  readAllData('posts')
    .then(function(data) {
      if (!networkDataReceived) {
        console.log('From indexedDB', data);
        updateUI(data);
      }
    })
}
```

its working! why is the image working?

we are just storing the link in indexedDB...

the reason is our dynamic caching!

the image is fetch just like the other assets in our application!

we find the image in cache, cache storage, dynamic-v2


















































