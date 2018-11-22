155_opening_a_page_upon_user_interaction.md

we are already listening to notificationclick, but we dont do anything

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
  }
});
```

clients, all windows, refered to this sw

promise, clis, all things managed by the sw

to identify the first window of our application

clis is just an array.find, execute
return true, we found the element we are looking for.



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
            client.navigate('http://localhost:8080');
            client.focus();
          } else {
            clients.openWindow('http://localhost:8080');
          }
          notification.close();
        })
    );
    console.log(action);
  }
});
```









we hardcoded the localhost:8080


the url changes, if we have blogs, thte id changes

how to pass data from the server





