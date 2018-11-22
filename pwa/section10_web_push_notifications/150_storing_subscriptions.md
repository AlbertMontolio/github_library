150_storing_subscriptions.md

our backend is now our function in firebase

any of your node.js, for rails, web push

in the terminal, in the functions folder!!!! --save puts it in the package.json

```js
npm install --save web-push
```

functions/package.json

```js
  "dependencies": {
    "cors": "^2.8.4",
    "firebase-admin": "^6.0.0",
    "firebase-functions": "^2.0.3",
    "web-push": "^3.3.2"
  },
```



web app keys, and send the web push messages itself

create a scripts in our package.json from the folder functions!!!

```js
  "scripts": {
    "lint": "eslint .",
    "serve": "firebase serve --only functions",
    "shell": "firebase functions:shell",
    "start": "npm run shell",
    "deploy": "firebase deploy --only functions",
    "logs": "firebase functions:log",
    "web-push": "web-push"
  },
```

with web-push, we can create keys from the terminal

if we run `npm run web-push`, error

it gives us a hint, how we may use it

```js
npm run web-push generate-vapid-keys
```

this gives us a public key and a private key

for now, we need the public key


leets go to our app.js, and store it in a varaible

```js
if (sub === null) {
  // create a new subscription
  var vapidPublicKey = "BIT_xxxxxQy4B4";
  reg.pushManager.subscribe();
} else {
```

the subscribe object expects to get a ute8array

we add a function to our utility.js file

```js
function urlBase64ToUint8Array(base64String) {
var padding = '='.repeat((4 - base64String.length % 4) % 4);
var base64 = (base64String + padding)
  .replace(/\-/g, '+')
  .replace(/_/g, '/');

var rawData = window.atob(base64);
var outputArray = new Uint8Array(rawData.length);

for (var i = 0; i < rawData.length; ++i) {
  outputArray[i] = rawData.charCodeAt(i);
}
return outputArray;
}
```

convert a urlBase64 string, (the public vapid key)
to an array of uint8 values

this is a helper function, that we use in app.js




now i have the value that i need to pass to the subscribe object


```js
if (sub === null) {
  // create a new subscription
  var vapidPublicKey = "BIT_62ywMv_SkgZGJb1WO3OgqNNgiP_MNtPIhFZynSPL6o5QQX2r2xAPx5jlfl1VcMleKZDuzv_r9xWfutQy4B4";
  var convertedVapidPublicKey = urlBase64ToUint8Array(vapidPublicKey);

  reg.pushManager.subscribe();
```

we add a second property,


```js
.then(function(sub) {
  if (sub === null) {
    // create a new subscription
    var vapidPublicKey = "BIT_62ywMv_SkgZGJb1WO3OgqNNgiP_MNtPIhFZynSPL6o5QQX2r2xAPx5jlfl1VcMleKZDuzv_r9xWfutQy4B4";
    var convertedVapidPublicKey = urlBase64ToUint8Array(vapidPublicKey);

    return reg.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: convertedVapidPublicKey
    });
  } else {
    // we have a subscription
  }
})
```

just our server is allowed to send push messages

now we need to make sure that we need to pass our subscription to our backend

i continue with then, subscribe returns a promise, we get this new subscription

what i want to pass to my server now

to my firebase server

we have our db, we also want to store our news subscriptions

we do a post request to this url

```js
.then(function(newSub) {
  fetch('https://pwagram-50bea.firebaseio.com/subscriptions.json', {

    })
})
```

```js
.then(function(newSub) {
  return fetch('https://pwagram-50bea.firebaseio.com/subscriptions.json' {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'appplication/json'
    },
    body: JSON.stringify(newSub)
  })
})
```

we return what fetch gives us, is a promise of course

another then

i want to display the confirm notification

```js
.then(function(res) {
  displayConfirmNotification();
})
```

lets check first if we have a 200 response

```js
.catch(function(err) {
  console.log(err);
})


```

workinggg

in our db, we see the newly created notification

the endpoint, is a a google server, for chroome, browser vendor server, its not our server
this is the url we need to send pushs to

the keys. we have two keys, and auth, p256dh, these keys have our vapid information

we need to pass both keys to the endpoint

with our private key.

lets work on the server-side now


































