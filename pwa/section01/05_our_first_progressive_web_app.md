http server dependency

```js
"devDependencies": {
    "http-server": "^0.10.0"
  }
```

we need this, so that when we click index.html, we don't do a file protocol, but a http protocol

then we do

```js
npm start
```

and leave it running




browser / dev tools / application /

we find the service worker

we can put it offline, we can unregister, etc.


in app.js

we can add the serviceWorker to our navigator, to our browser

```js
var title = document.querySelector('.title');
var courseFeatureElements = document.querySelectorAll('.course-feature');
var button = document.querySelector('button');

navigator.serviceWorker.register('/sw.js');

function animate() {
  title.classList.remove('animate-in');
  for (var i = 0; i < courseFeatureElements.length; i++) {
    courseFeatureElements[i].classList.remove('animate-in');
  }
  button.classList.remove('animate-in');

  setTimeout(function () {
    title.classList.add('animate-in');
  }, 1000);
```



in application, offline, or undernetwork, offline

always disabled cache!!!














