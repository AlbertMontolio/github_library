127_storing_our_post_in_indexedDB.md


we dont know which data we want to synchronize

```js
form.addEventListener('submit', function(event) {
  event.preventDefault();

  if (titleInput.nodeValue.trim() === '' || locationInput.nodeValue.trim() === '') {
    alert('Please enter valid data');
    return;
  }

  // close modal
  closeCreatePostModal();
  // register background sync request
  if ('serviceWorker' in navigator && 'SyncManager' in window) {
    navigator.serviceWorker.ready
      .then(function(sw) {
        sw.sync.register('sync-new-post');
      })
  }
})
```

that data, is a title and a location, the two things we get from the input fields

create variable post, with title location, third property, an id, just the date, to be unique

```js
.then(function(sw) {
  var post = {
    id: new Date().toISOString(),
    title: titleInput.value,
    location: locationInput.value
  };
  sw.sync.register('sync-new-post');
})
```

now i need to store it in indexedDB. why?

we cant pass it into the register method

we wrote the utility.js

the method writeData

utility.js

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

write data can be user because we import utility.js in index.html

writeData() takes two arguments, teh store we want to access, and the data we want to pass

which store? we have one already

```js
var dbPromise = idb.open('posts-store', 1, function(db) {
  if (!db.objectStoreNames.contains('posts')) {
    db.createObjectStore('posts', { keyPath: 'id' });
  }
});
```

but that is not the one we want

create a new object store, named sync-posts

```js
var dbPromise = idb.open('posts-store', 1, function(db) {
  if (!db.objectStoreNames.contains('posts')) {
    db.createObjectStore('posts', { keyPath: 'id' });
  }
  if (!db.objectStoreNames.contains('sync-posts')) {
    db.createObjectStore('sync-posts', { keyPath: 'id' });
  }
});
```


```js
if ('serviceWorker' in navigator && 'SyncManager' in window) {
  navigator.serviceWorker.ready
    .then(function(sw) {
      var post = {
        id: new Date().toISOString(),
        title: titleInput.value,
        location: locationInput.value
      };
      writeData('sync-posts', post)
        .then(function() {
          sw.sync.register('sync-new-post');
        });
    })
}
```

id=confirmation-toast comes from material design

```js
writeData('sync-posts', post)
  .then(function() {
    return sw.sync.register('sync-new-post');
  })
  .then(function() {
    var snackbarContainer = documenet.querySelector('#confirmation-toast');
    var data = { message: 'Your post was saved for syncing' };
    snackbarContainer.MaterialSnackbar.showSnackbar(data);
  })
  .catch(function(err) {
    console.log(err);
  });
```


























