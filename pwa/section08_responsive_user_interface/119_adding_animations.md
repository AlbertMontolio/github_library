119_adding_animations.md

we want to slide up

index.html

id=create-post

we want to slide up the whole div

```html
<div id="create-post">
  <form>
    <div class="input-section mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
      <input class="mdl-textfield__input" type="text" id="title">
      <label class="mdl-textfield__label" for="title" name="title">Title</label>
    </div>
    <div class="input-section mdl-textfield mdl-js-textfield mdl-textfield--floating-label" id="manual-location">
      <input class="mdl-textfield__input" type="text" id="location">
      <label class="mdl-textfield__label" for="location" name="location">Location</label>
    </div>
    <br>
    <div>
      <button class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored mdl-color--accent"
              type="submit" id="post-btn">Post!
      </button>
    </div>
    <br>
    <div>
      <button class="mdl-button mdl-js-button mdl-button--fab" id="close-create-post-modal-btn" type="button">
        <i class="material-icons">close</i>
      </button>
    </div>
  </form>
</div>
```



in feed.js

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';
  if (deferredPrompt) {
    deferredPrompt.prompt();

    deferredPrompt.userChoice.then(function(choiceResult) {
      console.log(choiceResult.outcome);

      if (choiceResult.outcome === 'dismissed') {
        console.log('User cancelled installation');
      } else {
        console.log('User added to home screen');
      }
    });

    deferredPrompt = null;
  }

}
```

we use to unhide it

```js
createPostArea.style.display = 'block';
```

in feed.css

```css
#create-post {
  z-index: 1001;
  position: fixed;
  width: 100%;
  min-height: 100vh;
  overflow-y: scroll;
  bottom: 0;
  top: 56px;
  background: white;
  text-align: center;
  display: none;
}
```


```css
transform: translateY(100vh);
```

transition, we animate a given property over the time

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';
  createPostArea.style.transform = 'translateY(0)';
```

animation is not played

```js
function openCreatePostModal() {
  createPostArea.style.display = 'block';
  setTimeout(function() {
    createPostArea.style.transform = 'translateY(0)';
  })
```

we see now the animation, but just once

we never animate back

```js
function closeCreatePostModal() {
  createPostArea.style.transform = 'translateY(100vh)';
  createPostArea.style.display = 'none';
}
```


we can do

```js
function openCreatePostModal() {
  // createPostArea.style.display = 'block';
  // setTimeout(function() {
    createPostArea.style.transform = 'translateY(0)';
  // })
```

```js
function closeCreatePostModal() {
  createPostArea.style.transform = 'translateY(100vh)';
  // createPostArea.style.display = 'none';
}
```







































