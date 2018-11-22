149_creating_a_push_subscription.md

we create a variable
var reg,

so that we can access it everywhere


we create a new subscription

```js
function configurePushSub() {
  if (!('serviceWorker' in navigator)) {
    return;
  }

  var reg;
  navigator.serviceWorker.ready
    .then(function(swreg) {
      reg = swreg;
      return swreg.pushManager.getSubscription();
    })
    .then(function(sub) {
      if (sub === null) {
        // create a new subscription
        reg.pushManager.subscribe();
      } else {
        // we have a subscription
      }
    })
}
```

this is not how we do it, a subscription contains the endpoint of that browser render server where we push the messages
anyone can push messages to this server,
if anyone finds the endpoint... he can send messages to our user
we need to protect this

we need to pass options
userVisibleOnly: true, not really security, just one user

we need to identify our server. passing the ip, is not secure

VAPID with WebPush

two keys, a public and a private one

the key is in our js, therefore, anyone has accept. therefore, we put the public key

the private one can be derivated by the public one, and stored in our backend

only the two together work

we use a package to generate them.

we will use a web push library

lets install web push, to generate the keys





























































