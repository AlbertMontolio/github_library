166_accepting_file_upload_example_with_firebase.md


backend code

index.js in functions

we need a package, which automacitlly extracts formData

in functions folder!!

```js
npm install --save formidable @google-cloud/storage
```

easy to access the data enclosed in formData

we also need a way to store the images in storage

the admin package (firebase-admin) does not allow this, w need a new package


in index.js

lets pull this packages in

formidable, on the top

```js
var formidable = require('formidable');
```

we need some config


```js
var serviceAccount = require("./pwagram-fb-key.json");

var gcconfig = {
  projectId: 'pwagram-50bea',
  keyFilename: 'pwagram-fb-key.json'
};

var gcs = require('@google-cloud/storage')(config);
```

in our function, to get access to the data we are receiving

inside this cors wrapper

var formData = new formidable



```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  var formData = new formidable.IncomingForm();
  formData.parse(request, function(err, fields, files) {

  });
```


first of all, i want to move the file from a temporary storage to another temporary storage

i import the fs package, its a default node.js

```js
var fs = require('fs');
```
old path, file referes to the name that we put in posData ... 'file'

in google cloud functions we have /tmp/

permanentbly, we move it to a bucket




fs.rename(files.file)


```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  var formData = new formidable.IncomingForm();
  formData.parse(request, function(err, fields, files) {
    fs.rename(files.file.path, '/tmp' + files.file.name);
    var bucket = gcs.bucket();
  });
```

we need to put the bucket name as an input

in storage, we already have a bucket

its the url on thte top

we need to generate a public download url, in case our users dont have google cloud

```js
bucket.upload('/tmp/' + files.file.name, {
  uploadType: 'media',
  metadata: {
    metadata: {
      contentType: files.file.type,
      firebaseStorageDownloadTokens:
    }
  }
});
```

we need a package for that

```js
npm install --save uuid-v4
```

in our index.js, we can import it on the top

before we use this package, we need to initialize it


```js
var UUID = require('uuid-v4');
```


```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  var uuid = UUID();
```





```js
bucket.upload('/tmp/' + files.file.name, {
  uploadType: 'media',
  metadata: {
    metadata: {
      contentType: files.file.type,
      firebaseStorageDownloadTokens: uuid;
    }
  }
});
```

this request does the upload

then we get the eerror or the uploaded file

```js
exports.storePostData = functions.https.onRequest(function(request, response) {
 response.send("Hello from Firebase!");
 cors(request, response, function() {
  var uuid = UUID();
  var formData = new formidable.IncomingForm();
  formData.parse(request, function(err, fields, files) {
    fs.rename(files.file.path, '/tmp' + files.file.name);
    var bucket = gcs.bucket('pwagram-50bea.appspot.com');

    bucket.upload('/tmp/' + files.file.name, {
      uploadType: 'media',
      metadata: {
        metadata: {
          contentType: files.file.type,
          firebaseStorageDownloadTokens: uuid;
        }
      }
    }, function(err, file) {
      if (!err) {
        // reach out the db

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
              webpush.sendNotification(pushConfig, JSON.stringify({
                title: 'New Post',
                content: 'New Post added!',
                openUrl: '/help'
              }));
            });
            return response.status(201).json({message: 'Data stored', id:request.body.id })
              .catch(function(err) {
                console.log(err);
              })
            // return true; // ### be careful
          })
          .catch(function(err) {
            return response.status(500).json({error: err});
          });

      } else {
        console.log(err);
      }
    });
  });
 });
});
```


we no longer use request.body, we use fields instead



```js
admin.database().ref('posts').push({
  id: fields.id,
  title: fields.title,
  location: fields.location,
  image: fields.image
})
```

the image is not correct, needs to be the url

firebase generates the public link for us, and follows us a pattern

```js
admin.database().ref('posts').push({
  id: fields.id,
  title: fields.title,
  location: fields.location,
  image: 'https://firebasestorage.googleapis.com/v0/b/' + bucket.name + '/o/' + encodeURIComponent(file.name) + '?alt=media&token=' + uuid
})
```


lets deploy in the main folder



























































