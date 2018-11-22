170_getting_the_user_position.md

geolocation developer mozilla


get current option

we need to get first the button

index.html

```html
<button class="mdl-button mdl-js-button mdl-button mdl-button--colored" type="button" id="location-btn">
  Get location
</button>
```


location-loader is a spinner,


feed.js

```js
var locationBtn = document.querySelector('#location-btn');
var locationLoader = document.querySelector('#location-loader');
```

hide the loader

```css
#create-post #pick-image, #create-post #location-loader {
  display: none;
}
```

```js
function closeCreatePostModal() {
  createPostArea.style.transform = 'translateY(100vh)';
  createPostArea.style.display = 'none';
  imagePickerArea.style.display = 'none';
  videoPlayer.style.display = 'none';
  canvasElement.style.display = 'none';

  locationBtn.style.display = 'inline'; // <----
  locationLoader.style.display = 'none'; // <---
}
```


```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';
  initializeMedia();
  initializeLocation();
```



```js
locationBtn.addEventListener('click', function(event) {
  if (!('geolocation in navigator')) {
    return;
  }
});

function initializeLocation() {
  if (!('geolocation in navigator')) {
    locationBtn.style.display = 'none';
  }
}
```


```js
locationBtn.addEventListener('click', function(event) {
  if (!('geolocation in navigator')) {
    return;
  }

  // get current position
  // this asks for permission
  // we have two callbacks

  // the first one is a function, the second is an error
  navigator.geolocation.getCurrentPosition(function(position) {

  }, function(err) {
    console.log(err);
  }, {
    timeout: 7000
  })
});
```


we need the spinner

```js
locationBtn.addEventListener('click', function(event) {
  if (!('geolocation in navigator')) {
    return;
  }

  locationBtn.style.display = 'none';
  locationLoader.style.display = 'block';

  navigator.geolocation.getCurrentPosition(function(position) {

  }, function(err) {
    console.log(err);
    locationBtn.style.display = 'inline';
    locationLoader.style.display = 'none';
    alert('Couldnt fetch location');
  }, {
    timeout: 7000
  })
});
```


on the top

```js
var fetchedLocation;
```

```js
  alert('Couldnt fetch location');
  fetchedLocation = null;
```

```js

navigator.geolocation.getCurrentPosition(function(position) {
  locationBtn.style.display = 'inline';
  locationLoader.style.display = 'none';
  fetchedLocation = position.coords.latitude;
  locationInput.value = 'In Munich';
  locationInput.classList.add('is-focused');

}, function(err) {
```






```js
locationBtn.addEventListener('click', function(event) {
  if (!('geolocation in navigator')) {
    return;
  }

  locationBtn.style.display = 'none';
  locationLoader.style.display = 'block';

  navigator.geolocation.getCurrentPosition(function(position) {
    locationBtn.style.display = 'inline';
    locationLoader.style.display = 'none';
    fetchedLocation = {lat: position.coords.latitude, lng: 0};
    locationInput.value = 'In Munich'; // you could use google api
    locationInput.classList.add('is-focused');

  }, function(err) {
    console.log(err);
    locationBtn.style.display = 'inline';
    locationLoader.style.display = 'none';
    alert('Couldnt fetch location');
    fetchedLocation = {lat: null, lng: null};
  }, {
    timeout: 7000
  })
});
```

now to the backend



in addEventListener submit


```js
if ('serviceWorker' in navigator && 'SyncManager' in window) {
  navigator.serviceWorker.ready
    .then(function(sw) {
      var post = {
        id: new Date().toISOString(),
        title: titleInput.value,
        location: locationInput.value,
        picture: picture,
        rawLocation: fetchedLocation
      };
```

```js
function sendData() {
  var id = new Date().toISOString();
  var postData = new FormData();
  postData.append('id', id);
  postData.append('title', titleInput.value);
  postData.append('location', locationInput.value);
  postData.append('rawLocationLat', fetchedLocation.lat);
  postData.append('rawLocationLng', fetchedLocation.lng);
  postData.append('file', picture, id + '.png');
```


in the sw well also use the values of indexeddb

so, in the sync listener

```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {

          for (var dt of data) {
            var postData = new FormData();
            postData.append('id', dt.id);
            postData.append('title', dt.title);
            postData.append('location', dt.location);
            postData.append('rawLocationLat', dt.rawLocation.lat);
            postData.append('rawLocationLng', dt.rawLocation.lng);
            postData.append('file', dt.picture, dt.id + '.png');
```


lets test it!

not working cuz of the fucking

Failed to load https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:8080' is therefore not allowed access. The response had HTTP status code 500. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
sw.js:218 Error while sending data TypeError: Failed to fetch
sw.js:204 Cross-Origin Read Blocking (CORB) blocked cross-origin response https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData with MIME type text/plain. See https://www.chromestatus.com/feature/5629709824032768 for more details.



anyway, if it happens to work

we are not storing the fetchedLocation in our db

so lets go to index.js in functions

```js
function(err, uploadedFile) {
  if (!err) {
    admin
      .database()
      .ref("posts")
      .push({
        id: fields.id,
        title: fields.title,
        location: fields.location,
        rawLocation: {
          lat: fields.rawLocationLat,
          lng: fields.rawLocationLng
        },
```


deploy to firebase deploy

```js
  document.querySelector('#manual-location').classList.add('is-focused');
```



















































































































