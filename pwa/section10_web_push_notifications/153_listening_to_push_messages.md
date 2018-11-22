153_listening_to_push_messages.md

in our sw file, lets start listening

the sw, is always running on the background.

they work even if the page is closed

selfaddeventlistern, if we receive a push

check if our event data object exists

sw.js

```js
self.addEventListener('push', function(event) {
  console.log('Push Notification received', event);

  var data = {title: 'New!', content: 'Something new happened!'};

  if (event.data) {
    data = JSON.parse(event.data.text());
  }
})
```

in the payload, title content, we cant send images, since the size is limited. but we can send links

```js
self.addEventListener('push', function(event) {
  console.log('Push Notification received', event);

  var data = {title: 'New!', content: 'Something new happened!'};

  if (event.data) {
    data = JSON.parse(event.data.text());
  }

  var options = {
    body: data.content,
    icon: '/src/images/icons/app-icon-96x96.png',
    badge: '/src/images/icons/app-icon-96x96.png'
  }

  event.waitUntil(
    self.registration.showNotification(data.title, options)
  );
})
```


don't clear site data, you would unregister the sw, and you have subscriptions in the backend pointing to this sw

interact with the notification, so that we can open a page










