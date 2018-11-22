161_getting_dom_access.md

now access the camera

for that, we need access to the video elemenet to the canvas

well use the canvas to freeze the image

with input image we could take a picture from the iphone
but the user leavas our app

with the video canvas strategy, user does not leave our app

feed.js, select canvas etc

```js
var videoPlayer = document.querySelector("#player");
var canvasElement = document.querySelector("#canvas");
var captureButton = document.querySelector("#capture-btn");
var imagePicker = document.querySelector("#image-picker");
var imagePickerArea = document.querySelector("#pick-image");
```

in openCreatePostModal

initializeMedia() i want to enable the camera in the devices that support

```js
function initializeMedia() {

}

function openCreatePostModal() {
  createPostArea.style.display = 'block';
  initializeMedia();
```






























