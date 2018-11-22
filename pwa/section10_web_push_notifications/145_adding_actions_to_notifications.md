145_adding_actions_to_notifications.md

actions may not be support by the user

action object takes three properties
1 an id, like confirm
2

```js
  tag: 'confirm-notification',
  renotify: true,
  actions: [
    { action: 'confirm', title: 'Okay', icon: '/src/images/icons/app-icon-96x96.png' },
    { action: 'cancel', title: 'Cancel', icon: '/src/images/icons/app-icon-96x96.png' }
  ]
};
```




