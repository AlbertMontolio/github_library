165_storing_the_image_on_a_server.md

the image that we have right now, only lives in the canvas


we need a file, where we can upload



utility.js

```js
function dataURItoBlob(dataURI) {
  var byteString = atob(dataURI.split(',')[1]);
  var mimeString = dataURI.split(',')[0].split(':')[1].split(';')[0]
  var ab = new ArrayBuffer(byteString.length);
  var ia = new Uint8Array(ab);
  for (var i = 0; i < byteString.length; i++) {
    ia[i] = byteString.charCodeAt(i);
  }
  var blob = new Blob([ab], {type: mimeString});
  return blob;
}
```

convert to a file

in feed.js


```js
var picture;
```

```js
captureButton.addEventListener('click', function(event) {
  canvasElement.style.display = 'block';
  videoPlayer.style.display = 'none';
  captureButton.style.display = 'none';
  var context = canvasElement.getContext('2d');
  context.drawImage(videoPlayer, 0, 0, canvas.width, videoPlayer.videoHeight / (videoPlayer.videoWidth / canvas.width));
  videoPlayer.srcObject.getVideoTracks().forEach(function(track) {
    track.stop();
  });
  picture = dataURItoBlob(canvasElement.toDataURL()); // <-------------
});
```

how do we upload an image to our server?

we are interacting with our server with sw

in sw.js

in self.AddEventListener('sync', function)

we send data to our storePostData function


image will no longer be a string, it will be a file



```js
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {

          for (var dt of data) {
            fetch('https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
              },
              body: JSON.stringify({
                id: dt.id,
                title: dt.title,
                location: dt.location,
                // later we will take an image!
                image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
              })
            })
```

formData we will send

in feed.js, we are also sending data


we have the same

```js
function sendData() {
  fetch('https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      id: new Date().toISOString(),
      title: titleInput.value,
      location: locationInput.value,
      image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
    })
  })
```

lets adjust our code in the service worker

no longer the headers, we get rid of them

```js
headers: {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
},
```




postData new FormData, is a constructor, gives us an object. it allows to send this object to a backend through ajax

we append data to this object
for example, the id, the id, didnt chagne, so dt.id
we also want to send the title, the location, also, what we passesd with json before

we also pass an image, 'file', dt.picture, dt.id + '.png'


now we can replace our body
body: postData


```js
for (var dt of data) {
  var postData = new FormData();
  postData.append('id', dt.id);
  postData.append('title', dt.title);
  postData.append('location', dt.location);
  postData.append('file', dt.picture, dt.id + '.png');

  fetch('https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData', {
    method: 'POST',
    body: postData
  })
```



lets do the same in feed.js


```js
function sendData() {
  var id = new Date().toISOString();
  var postData = new FormData();
  postData.append('id', id);
  postData.append('title', titleInput.value);
  postData.append('location', locationInput.value);
  postData.append('file', picture, id + '.png');
```


```js
function sendData() {
  var id = new Date().toISOString();
  var postData = new FormData();
  postData.append('id', id);
  postData.append('title', titleInput.value);
  postData.append('location', locationInput.value);
  postData.append('file', picture, id + '.png');

  fetch('https://us-central1-pwagram-50bea.cloudfunctions.net/storePostData', {
    method: 'POST',
    body: postData
  })
  .then(function() {
    console.log('Sent data', res);
    updateUI();
  });
}
```



in feed.js, in addEventListener('submit')

we have writeData, there we just store id, title and location

we can also stoer files in indexedDb too

```js
form.addEventListener('submit', function(event) {
  event.preventDefault();

  if (titleInput.value.trim() === '' || locationInput.value.trim() === '') {
    alert('Please enter valid data');
    return;
  }
  // close modal
  closeCreatePostModal();
  // register background sync request
  if ('serviceWorker' in navigator && 'SyncManager' in window) {
    navigator.serviceWorker.ready
      .then(function(sw) {
        var post = {
          id: new Date().toISOString(),
          title: titleInput.value,
          location: locationInput.value
        };
        writeData('sync-posts', post)
          .then(function() {
            return sw.sync.register('sync-new-posts');
          })
          .then(function() {
            var snackbarContainer = document.querySelector('#confirmation-toast');
            var data = { message: 'Your post was saved for syncing' };
            snackbarContainer.MaterialSnackbar.showSnackbar(data);
          })
          .catch(function(err) {
            console.log(err);
          });
      })
  } else {
    // send data direct to the server
    sendData();
  }
})
```



we add the picture (file)

```js
if ('serviceWorker' in navigator && 'SyncManager' in window) {
navigator.serviceWorker.ready
  .then(function(sw) {
    var post = {
      id: new Date().toISOString(),
      title: titleInput.value,
      location: locationInput.value,
      picture: picture
    };
```

we need to adjust the code in the backend. its expecting json



























































































































