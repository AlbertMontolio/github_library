141_displaying_notifications.md

```js
function displayConfirmNotification() {

}

function askForNotificationPermission() {
  // you also get permission for push. for both
  Notification.requestPermission(function(result) {
    console.log('User choice', result);
    if (result !== 'granted') {
      console.log('No notification permission granted!');
    } else {
      displayConfirmNotification
    }
  });
}
```

most basic form of notification:

```js
function displayConfirmNotification() {
  new Notification('Successfully subscribed');
}
```

real system notification, not a js alert!

niceeee

but this is boring, we can pass options to it

```js
function displayConfirmNotification() {
  var options = {
    body: 'You successfully subscribed to our Notification service!'
  };
  new Notification('Successfully subscribed', options);
}
```













































