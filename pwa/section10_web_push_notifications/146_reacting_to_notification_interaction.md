146_reacting_to_notification_interaction.md

clicking in a button from the action or the notification itself

a notification is a system feature, is not displayed in our webapp

so we put it in sw.

user may click, even if our page is not opened. thats what sw do. thread in the background

sw.js

new event listener
'notificationclick'

whenever user clicks a notfication
we can get info about the notification

we can also find out which action was clicked

we can also checked if the action was confirmed 'confirm' is what you put as a name when u created the notification

sw.js

```js
self.addEventListener('notificationclick', function(event) {
  var notification = event.notification;
  var action = event.action;

  console.log(notification);

  if (action === 'confirm') {
    console.log('Confirm was chosen');
    notification.close();
  } else {
    console.log(action);
    notification.close();
  }
});
```

lets find out what a close is




