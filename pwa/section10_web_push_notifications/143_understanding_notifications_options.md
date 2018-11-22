143_understanding_notifications_options.md


the two options that we implemented, body and the title

are supported by eveery device (not browser)

the features that you display in a notification depends on the device,not the browser

more options

icon, and path to the icon



/ absoltue url


```js
function displayConfirmNotification() {
  if ('serviceWorker' in navigator) {
    var options = {
      body: 'You successfully subscribed to our Notification service!',
      icon: '/src/images/icons/app-icon-96x96.png',
      image: '/src/images/sf-boat.jpg',
      dir: 'ltr',
      lang: 'en-US',
      vibrate: [100, 50, 200],
      badge: '/src/images/icons/app-icon-96x96.png'
    };
```

connect mobile to mac android emulator























