168_testing_the_camera_upload.md

testing the camera, while clicking capture


```js
feed.js:58 Uncaught ReferenceError: dataURItoBlob is not defined
    at HTMLButtonElement.<anonymous> (feed.js:58)
```

in feed.js, we call the method

```js
  picture = dataURItoBlob(canvasElement.toDataURL());

```

is in our utility.js defined

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

in sw.j swe nevered precached this file

we need to added in our precache

```js
var CACHE_STATIC_NAME = 'static-v8';
var CACHE_DYNAMIC_NAME = 'dynamic-v3';
var STATIC_FILES = [
  '/',
  '/index.html',
  '/offline.html',
  '/src/js/app.js',
  '/src/js/utility.js',
```

still the same error, eventhought we are preacaching the file

in the caches, we have the static and the dynamic

we have utility.js in both. in the dynamic one, we have the old one. unfortunately, thats the one that is picked up

workingggggg

at the end, we can send this to any server, we are just sending an http request



















































