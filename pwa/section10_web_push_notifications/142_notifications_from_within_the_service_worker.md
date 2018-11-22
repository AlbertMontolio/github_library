142_notifications_from_within_the_service_worker.md

check if sw is supported in the given navigator

swreg is not only the sw, where we can listen to events, but with the registartion, we can do notification

```js
function displayConfirmNotification() {
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.ready
      .then(function(swreg) {

      });
  }
```

so, we are showing the notification from the sw.
this will become important later, when we get info from the server












