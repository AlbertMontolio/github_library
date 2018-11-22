169_implementing_a_fallback.md


in feed.js

in initializeMedia, if we fail, we can show the imagePickerArea, but we dont send it

```js
imagePicker.addEventListener('change', function(event) {
  picture = event.target.files[0];
})
```






