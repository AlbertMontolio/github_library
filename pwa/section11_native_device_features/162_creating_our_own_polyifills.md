162_creating_our_own_polyifills.md


check if we have mediadevices in the navigator
access to the camera, to the microphone

if we dont support medadevices, we can create our own polyfills

getUserMedia, it's ok

create our own media devices, so we create our own mediaDevices

we create an empty object

```js
function initializeMedia() {
  if (!('mediaDevices' in navigator)) {
    navigator.mediaDevices = {}
  }
}
```


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

    }
  }
}
```

we can check, if getUserMedia, is not true
if we are in old safari or internet explorer, we are screwed

so, we return a promise, to rebuild this behaviour

return Promise. that immediately rejects, we through an error, getUserMedia is not implemented

if we have webKit or moz

then, we return a promise, this promise will resolve or reject
getUserMedia.call(navigator, constraints, resolve, reject);

uau


this was a polifill, our general fallback, if browser doe snot support






































