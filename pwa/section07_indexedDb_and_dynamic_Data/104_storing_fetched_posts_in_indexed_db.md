104_storing_fetched_posts_in_indexed_db.md


sw has a special syntax to import other packages

```js
importScripts()
```

we can make our sw leaner

we import

```js
importScripts('/src/js/idb.js');

```

we also cache the file, to use it offline

```js
var STATIC_FILES = [
  '/',
  '/index.html',
  '/offline.html',
  '/src/js/app.js',
  '/src/js/feed.js',
  '/src/js/idb.js',
```

when do we put the data in the db?

here

```js
var url = 'https://pwagram-50bea.firebaseio.com/posts';
  if (event.request.url.indexOf(url) > -1) {
    event.respondWith(
      caches.open(CACHE_DYNAMIC_NAME)
        .then(function (cache) {
          return fetch(event.request)
            .then(function (res) {
              // trimCache(CACHE_DYNAMIC_NAME, 3);
              cache.put(event.request, res.clone());
              return res;
            });
        })
    );
  } else if (isInArray(event.request.url, STATIC_FILES)) {
```

instead of put sth in the cache, we put it in the db

first, open indexdb
second, open an object store

we do this on the top

dbPromise gives us access to the db in a promise like way

name, version, pass a function

this function will be executed whenever db has been created

object store is like a table

db.createObjectStore('posts', )

second argument, object we define primary key. later on, we can retrieve obejcts by this key


we only want to createobjectstore when it does not exist. it always created, we need an if statement

we get rid of the cache part

```js
var url = 'https://pwagram-50bea.firebaseio.com/posts';
if (event.request.url.indexOf(url) > -1) {
  event.respondWith( fetch(event.request)
    .then(function (res) {
      return res;
    })

  );
}
```

just returning the fetch is all we need

whenever we hit the url, we fetch the url, we return the res

we also want to put the res in the db

we need a copy of the res. the original response needs to be return

we transform the response, to a json()

json gives us back a promise

we get an object from firebase, i want an array


```js
var url = 'https://pwagram-50bea.firebaseio.com/posts';
if (event.request.url.indexOf(url) > -1) {
  event.respondWith( fetch(event.request)
    .then(function (res) {
      var clonedRes = res.clone();
      clonedRes.json()
        .then(function(data) {
          for (var key in data) {

          }
        });
      return res;
    })

  );
}
```

dbPromise, we get access to the db


```js
clonedRes.json()
  .then(function(data) {
    for (var key in data) {
      dbPromise
      .then(function(db) {
        var tx = db.transaction('posts', 'readwrite');
        var store = tx.objectStore('posts');
        store.put(data[key]);
        return tx.complete;
      });
    }
  });
return res;
```

now i want to use the dbobject to write some data in the db

we need to create a transaction to operate with the db

transaction method, first argument, ('posts'),s
first arg: which store we want to use, in our case, posts

remember:

```js
  db.createObjectStore('posts', { keyPath: 'id' });
```

2nd argument, which kind of transactin

then we decide, which store to open (before we just said, we will work with posts in the transaction, now we explicitely open it)

objectStore

```js
clonedRes.json()
  .then(function(data) {
    for (var key in data) {
      dbPromise
      .then(function(db) {
        var tx = db.transaction('posts', 'readwrite');
        var store = tx.objectStore('posts');
        store.put(data[key]);
        return tx.complete;
      });
    }
  });
return res;
```

in put we put the data, we put first-post, remember, it is all the object inside, id, image, location, title

in this object we have an id property, id was our keypath, so we can retrieve data later on easily

tx.complete,


```js
clonedRes.json()
  .then(function(data) {
    for (var key in data) {
      dbPromise
      .then(function(db) {
        var tx = db.transaction('posts', 'readwrite');
        var store = tx.objectStore('posts');
        store.put(data[key]);
        return tx.complete;
      });
    }
  });
return res;
```




lets check it!

we have an indexedDb in our storage browser


we see a posts-store - http://localhost

version 1

inside, we see posts, we see our first-post!!

now we need to get the posts from the db!













































