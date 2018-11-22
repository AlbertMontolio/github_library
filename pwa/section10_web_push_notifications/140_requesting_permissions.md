140_requesting_permissions.md

index.html


we have two buttons with enable-notifications class

```html
<button class="enable-notifications mdl-button mdl-js-button mdl-button--raised mdl-button--colored mdl-color--accent">
  Enable Notifications
</button>
```

lets work in app.js, since the help page also uses it

add a new variable, enableNotificationsButtons

selectAll elements with class enable-notifications



```js
var enableNotificationsButtons = document.querySelectorAll('.enable-notifications');

```

check if our browser has this option

show the buttons just if we browser is ok

in app.css

```js
.enable-notifications {
  display: none;
}
```

app.js

```js
if ('Notification' in window) {
  for (var i =0; i < enableNotificationsButtons.length; i++) {
    enableNotificationsButtons[i].style.display = 'inline-block';
    enableNotificationsButtons[i].addEventListener('click');
  }
}
```

before if ('Notif')

```js
function askForNotificationPermission() {
  // you also get permission for push. for both
  Notification.requestPermission(function(result) {
    console.log('User choice', result);
    if (result !== 'granted') {
      console.log('No notification permission granted!');
    } else {
      // hide the button
    }
  });
}
```

workinggggg





















