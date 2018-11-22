164_hooking_up_the_capture_button.md

lets hoopo up capture button

feed.js

after initializeMedia

captureButton.listen to a click

take an image

we need the canvas element

get the stream of the video element, send it to the canvas, since the canvas is there to display static content
canvas will take the last screenshoot available

on the click listener, i want to show the canvasElement
then we disable the videoplayer, we do it with display none, but the stream is running

i want to set captureButton. none

take the sscreen

var context = canvaselement getContext

how do we wanna draw? we want a 2d
we can use this canvas context to draw an image
draw an image is easy, use videoPlAYER , we need to define boundaries
regarding the height, need a calculation

videoPlayer still has the screen going on,

i need to stop the video
videoPlayer.srcObject.getVideoTracks(); access to all the video that we have access

we loop over it, for each track, we call stop();

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
});
```

now lets try to put the pic in the server

















