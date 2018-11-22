92_getting_rid_of_a_service_worker.md

we will do it in openCreatePostModal() just for practicing

inside

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.getRegistrations()
    .then(function(registrations) {
      for (var i = 0; i < registrations.length; i++) {
        registrations[i].unregister();
      }
    })
}
```

the sw its switch to deleted

the cache is also cleared. this is done for you, when you unregister a sw





































