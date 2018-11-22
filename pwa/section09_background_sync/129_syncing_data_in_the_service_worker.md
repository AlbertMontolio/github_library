129_syncing_data_in_the_service_worker.md

now lets make sure, that we send data in the service worker

sw.js

register a new event

listen to this sync event, its triggered whenever the sw sees that the connection is reestablished

in this event, pass to the function

send a request to the server, at this point o ftime, i know that we have internet

i want to check the event tag, in feed.js i have a task 'sync-new-post'

i want to handle this specifically

the event has a tag property!

eventwaituntil, make sure that we wait until the data is sent

need to read the data in the 'sync-post' in my indexedDb

we did it in

feed.js

```js
        writeData('sync-posts', post)
```

we read teh data here

sw.js

```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {

        })
    )
  }
})
```

now send it to my server!

maybe we have multiple posts. we will use almost same code as in send data

```js
function sendData() {
  fetch('https://pwagram-50bea.firebaseio.com/posts.json', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      id: new Date().toISOString(),
      title: titleInput.value,
      location: locationInput.value,
      image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
    })
  })
  .then(function() {
    console.log('Sent data', res);
    updateUI();
  });
}
```

we change id, title, location and so on

```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {

          for (var dt of data) {
            fetch('https://pwagram-50bea.firebaseio.com/posts.json', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
              },
              body: JSON.stringify({
                id: dt.id,
                title: dt.title,
                location: dt.location,
                // later we will take an image!
                image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
              })
            })
            .then(function() {
              console.log('Sent data', res);
              // updateUI(); we don't have access to the ui in a sw!
            });

          }

        })
    )
  }
})
```



```js
// updateUI(); we don't have access to the ui in a sw!
```

instead, i want to clean up my sync-posts object store in indexedDb

for that, we use deleteItemFromData



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
function deleteItemFromData(st, id) {
```

res.ok helpe rmethod in res object to see if responses was 200


```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {

          for (var dt of data) {
            fetch('https://pwagram-50bea.firebaseio.com/posts.json', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
              },
              body: JSON.stringify({
                id: dt.id,
                title: dt.title,
                location: dt.location,
                // later we will take an image!
                image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
              })
            })
              .then(function(res) {
                console.log('Sent data', res);
                if (res.ok) {
                  deleteItemFromData('sync-posts', dt.id);
                }
              })
              .catch(function(err) {
                console.log('Error while sending data', err);
              });
          }
        })
    )
  }
})
```



try it! if it does not work, you have to turn off wifi
for sync you need to do that, offline in chrome is not enough

clear the storage, reload the page

we see background syncing

we see the syncEvent, tag: "sync-new-post"

typoooooo

in

```js
form.addEventListener('submit', function(event) {
```



```js
writeData('sync-posts', post)
  .then(function() {
    return sw.sync.register('sync-new-posts');
  })
```


because in sw.js

```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
```




it works, now, we can't write in our firebase db, we dont have permission

but we see the post in indexedDB, posts-store posts, sync-posts



the background sync worked!

```js
{
  "rules": {
    ".read": "true",
    ".write": "auth != null"
  }
}
```

change it for true for a moment




































