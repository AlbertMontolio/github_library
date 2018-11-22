126_registering_a_synchronization_task.md

register sync task

check if we have serviceWorker, check if syncManager
this is the api

SyncManager in mdn documentation

just two methods! register, getTags

check if sw is read, returns a promise, we chain then

give me access to sw when its read
i can interact with the sw. i am still interacting in the feed.js, thats why i need access. i am not in sw.js

in sw.js we can not listen to the dom

sw.sync.register();

we need a tag, we give a name you want

we need to know what we should send!

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



