152_sending_push_messages_from_the_server.md


we could run this on our own node.js

admin.database() we want to store the data on new post,
when we did so, we send a message to all our subscribe clients

```js
const functions = require('firebase-functions');
var admin = require('firebase-admin');
var cors = require('cors')({origin: true});
var webpush = require('web-push');
```

webpush.setVapidDetails

we dont have the keys

lets grab the private key

```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  admin.database().ref('posts').push({
    id: request.body.id,
    title: request.body.title,
    location: request.body.location,
    image: request.body.image
  })
    .then(function() {
      webpush.setVapidDetails() // <-------
      return response.status(201).json({message: 'Data stored', id:request.body.id })
    })
    .catch(function(err) {
      return response.status(500).json({error: err});
    });
 });
});
```

we pass arguments. identifier of ourselfs

a valid email address, mailto:
next argument, public key, next argument, private key

```js
.then(function() {
  webpush.setVapidDetails('mailto:contact@albert.com', "BIT_62ywMv_SkgZGJb1WOdafsdfafafasfasdfasfasfr2xAPx5jlfl1VcMleKZDuzv_r9xWfutQy4B4", "1Lr1jxadfadfafafasfasfaEzhXLgI");
  return response.status(201).json({message: 'Data stored', id:request.body.id })
})
```

we almost have everything, we wanna send this push notification to all our subscriptions

for that, i need to fetch them

access the db, ref('subscriptons').once, only fetch data one. we want the value

lets chain another then call, will have the subscriptions
now i want to send the response, but before doing thtat, i want to acccess the subscriptions. we have forEach of

sub will be endpoint and keys

new var pushConfig, variable object
we will later pass it on the web push
we need to define the endpoint, we extract it from sub.val() to extract the value, we have a endpoint property

second argumenet keys

```js
.then(function() {
  webpush.setVapidDetails('mailto:contact@albert.com', "BIT_62ywMv_SkgZGJb1WO3OgqNNgiP_MNtPIhFZynSPL6o5QQX2r2xAPx5jlfl1VcMleKZDuzv_r9xWfutQy4B4", "1Lr1jxV6ueEUov7XJybW059gF8JyhkfpNIKpEzhXLgI");
  return admin.database().ref('subscriptions').once('value');
  // return response.status(201).json({message: 'Data stored', id:request.body.id })
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
  })
})
```

sendNotification, we want to pass some payload

```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  admin.database().ref('posts').push({
    id: request.body.id,
    title: request.body.title,
    location: request.body.location,
    image: request.body.image
  })
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
        webpush.sendNotification(pushConfig, JSON.stringify({title: 'New Post', content: 'New Post added!'}));
      });
      response.status(201).json({message: 'Data stored', id:request.body.id })
        .catch(function(err) {
          console.log(err);
        })
    })
    .catch(function(err) {
      return response.status(500).json({error: err});
    });
 });
});
```

let's deploy this solution

in our project folder,

```js
firebase deploy
```

```js
Deploy complete!

Project Console: https://console.firebase.google.com/project/pwagrasdfabea/overview
Hosting URL: https://pwagasfdafaea.firebaseapp.com
```


error:

Failed to load https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:8080' is therefore not allowed access. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

```js
Error: Can't set headers after they are sent.
    at ServerResponse.OutgoingMessage.setHeader (_http_outgoing.js:356:11)
    at applyHeaders (/user_code/node_modules/cors/lib/index.js:153:15)
    at applyHeaders (/user_code/node_modules/cors/lib/index.js:149:11)
    at applyHeaders (/user_code/node_modules/cors/lib/index.js:149:11)
    at cors (/user_code/node_modules/cors/lib/index.js:171:7)
    at /user_code/node_modules/cors/lib/index.js:224:17
    at originCallback (/user_code/node_modules/cors/lib/index.js:214:15)
    at /user_code/node_modules/cors/lib/index.js:219:13
    at optionsCallback (/user_code/node_modules/cors/lib/index.js:199:9)
    at corsMiddleware (/user_code/node_modules/cors/lib/index.js:204:7)

```

anyway, when you make it work

we are not listening to any push notifications











































