148_from_notifications_to_push_messages.md

get a subscription, store it in a server

we can only send to subscriber from the backend, to register devices, to register browsers on devices.

create a new subscription part

app.js

```js
function configurePushSub() {

}

function askForNotificationPermission() {
  // you also get permission for push. for both
  Notification.requestPermission(function(result) {
    console.log('User choice', result);
    if (result !== 'granted') {
      console.log('No notification permission granted!');
    } else {
      // displayConfirmNotification();
      configurePushSub();
    }
  });
}
```

// for push, we need to check if we have access to sw


don't forget to add

```js
if ('Notification' in window && 'serviceWorker' in navigator) {
```

```js
function configurePushSub() {
  if (!('serviceWorker' in navigator)) {
    return;
  }

  navigator.serviceWorker.ready
    .then(function(swreg) {

    })
}
```

subscriptions are managed by the sw

check if sw is ready, which gives me a promise
swreg = swregistration, i want to check for existing subscriptions
it returns a promise


in the function, i have access to the sw registration

inthere, iw ant to access the push manager and check for existing subscriptions




this will return any existing subscription

it return a promise actually.




```js
navigator.serviceWorker.ready
  .then(function(swreg) {
    swreg.pushManager.getSubscription();
  })
```

```js
navigator.serviceWorker.ready
  .then(function(swreg) {
    return swreg.pushManager.getSubscription();
  })
  .then(function(sub) {
    if (sub === null) {
      // create a new subscription
    } else {
      // we have a subscription
    }
  })
}
```









































