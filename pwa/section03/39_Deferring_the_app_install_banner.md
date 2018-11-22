39_Deferring_the_app_install_banner.md

when to show the banner, hej, download my app

maybe the best time to show the banner is when the user clicks the plus button to add a post

app.js

beforeinstallprompt

right before chrome wants to show the banner

we do a preventDefault
we store the event in a variable

```js
var deferredPrompt;

window.addEventListener('beforeinstallprompt', function(event) {
  console.log('beforeinstallprompt fired');
  event.preventDefault();
  deferredPrompt = event;
  return false;
});
```

in feed.js, we have the event who listens to click the plus

```js
shareImageButton.addEventListener('click', openCreatePostModal);
```

here we want to show the banner

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';
}
```

we need to check if we had a change to show it, we can show it on our own, chrome has to show it


```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';

  if (deferredPrompt) {
    deferredPromp.prompt();
  }
}
```

now check what the user picked

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';

  if (deferredPrompt) {
    deferredPromp.prompt();
  }

  deferredPrompt.userChoice.then(function(choiceResult) {
    if (choiceResult.outcome === 'dismissed') {
      console.log('User cancelled installation');
    } else {
      console.log('User added to home screen');
    }
  });

  deferredPrompt = null;
}
```

null, cuz user has just one chance to decide




























