156_improving_our_code.md



index.js

```js
.then(function() {
    webpush.setVapidDetails('mailto:contact@albert.com', "BIT_62ywMv_SkgZGJb1WO3OgqNNgiP_MNtPIhFZynSPL6o5QQX2r2xAPx5jlfl1VcMleKZDuzv_r9xWfutQy4B4", "1Lr1jxV6ueEUov7XJybW059gF8JyhkfpNIKpEzhXLgI");
    return admin.database().ref('subscriptions').once('value');
  })
  .then(function(subscriptions) {
    subscriptions.forEach(function(sub) {
      var pushConfig = {
        endpoint: sub.val().endpoint,
        keys: {
          auth: sub.val().keys.auth,
          p256dh: sub.val().keys.p256dh
        }
      }
      webpush.sendNotification(pushConfig, JSON.stringify({
        title: 'New Post',
        content: 'New Post added!',
        openUrl: '/help' // <---------------------
      }));
    });
```

run in the terminal

in the project folder

```js
firebase deploy
```


lets move to sw.js

in the push listener





data option, you pass some meta data




```js
self.addEventListener('push', function(event) {
  console.log('Push Notification received', event);

  var data = {title: 'New!', content: 'Something new happened!', openUrl: '/'};

  if (event.data) {
    data = JSON.parse(event.data.text());
  }

  var options = {
    body: data.content,
    icon: '/src/images/icons/app-icon-96x96.png',
    badge: '/src/images/icons/app-icon-96x96.png',
    data: {
      url: data.openUrl
    }
  }

  event.waitUntil(
    self.registration.showNotification(data.title, options)
  );
})
```

in notification click, we add data.openUrl

```js

self.addEventListener('notificationclick', function(event) {
  var notification = event.notification;
  var action = event.action;

  console.log(notification);

  if (action === 'confirm') {
    console.log('Confirm was chosen');
    notification.close();
  } else {
    event.waitUntil(
      clients.matchAll()
        .then(function(clis) {
          var client = clis.find(function(c) {
            return c.visibilityState === 'visible';
          });

          if (client !== undefined) {
            client.navigate(notification.data.url);
            client.focus();
          } else {
            clients.openWindow(notification.data.url);
          }
          notification.close();
        })
    );
    console.log(action);
  }
});
```


























