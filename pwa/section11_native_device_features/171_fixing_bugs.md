171_fixing_bugs.md


stop camera from recording us

if we close the modal

closeCrea

```js
function closeCreatePostModal() {
  createPostArea.style.transform = 'translateY(100vh)';
  createPostArea.style.display = 'none';
  imagePickerArea.style.display = 'none';
  videoPlayer.style.display = 'none';
  canvasElement.style.display = 'none';

  locationBtn.style.display = 'inline';
  locationLoader.style.display = 'none';

  if (videoPlayer.srcObject) {
    videoPlayer.srcObject.getVideoTracks().forEach(function(track) {
      track.stop();
    });
  }
}
```


the animation was wierd

```js
  if (videoPlayer.srcObject) {
    videoPlayer.srcObject.getVideoTracks().forEach(function(track) {
      track.stop();
    });
  }

  setTimeout(function() {
    createPostArea.style.transform = 'translateY(100vh)';
  }, 1);
}
```


fix animation while openeing the modal

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';

  setTimeout(function() {
    createPostArea.style.display = 'block';
  }, 1)
```



offline geolocation

we go offline

feed.js

```js
  alert('Couldnt fetch location');
  fetchedLocation = {lat: 0, lng: 0}; // <------
```

we saw the alert twice

```js
locationBtn.addEventListener('click', function(event) {
  if (!('geolocation in navigator')) {
    return;
  }

  var sawAlert = false;

  locationBtn.style.display = 'none';
  locationLoader.style.display = 'block';

  navigator.geolocation.getCurrentPosition(function(position) {
    locationBtn.style.display = 'inline';
    locationLoader.style.display = 'none';
    fetchedLocation = {lat: position.coords.latitude, lng: 0};
    locationInput.value = 'In Munich'; // you could use google api
    document.querySelector('#manual-location').classList.add('is-focused');

  }, function(err) {
    console.log(err);
    locationBtn.style.display = 'inline';
    locationLoader.style.display = 'none';
    if (!sawAlert) {
      alert('Couldnt fetch location');
      sawAlert = true;
    }
```

if we add a post again, we dont have a capture button

```js
var fetchedLocation = {lat: 0, lng: 0};
```

in closeCreatePostModal()

```js
  captureButton.style.display = 'inline';
```




































