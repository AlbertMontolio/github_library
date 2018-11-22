163_getting_the_video_image.md


lets access the camera

navigator.mediaDevices.getUserMedia

either natively, or from our custom implementation

this takes an argument, the constraints. video: true, audio: true
give me access to the video!

then we can do sth if we succeed, it gives us a stream object
what about permissions?
it will ask me the first time i access the camera

if user declines, we will go to catch

in catch, we have error, that the user says no permission, or that browser has no camera

imagePickerArea, we want to show it, if we are in a device that we dont have access to a camera

```js
function initializeMedia() {
  if (!('mediaDevices' in navigator)) {
    navigator.mediaDevices = {}
  }

  if (!('getUserMedia' in navigator.mediaDevices)) {
    // implement own solution, older cameras implementation
    // we just put the old implementation with the new syntax, so that we can use the new syntax throughout the application
    navigator.mediaDevices.getUserMedia = function(constraints) {
      var getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
      if (!getUserMedia) {
        return Promise.reject(new Error('getUserMedia is not implemented!'));
      }
      return new Promise(function(resolve, reject) {
        getUserMedia.call(navigator, constraints, resolve, reject);
      });
    }
  }

  navigator.mediaDevices.getUserMedia({video: true})
    .then(function(stream) {
      // access to the camera
      // output the stream we got, in the video player
      videoPlayer.srcObject = stream;
      videoPlayer.style.display = 'block';
    })
    .catch(function(err) {
      imagePickerArea.style.display = 'block';
    })
}
```

when we close


```js
function closeCreatePostModal() {
  createPostArea.style.transform = 'translateY(100vh)';
  createPostArea.style.display = 'none';
  imagePickerArea.style.display = 'none';
  videoPlayer.style.display = 'none';
  canvasElement.style.display = 'none';
}
```

if we click i in the browser, next to refresh, you can reset settings



















































