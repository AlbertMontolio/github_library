125_adding_the_basic_setup_to_our_project.md

we have to connect the post button with our js file

```html
<button class="mdl-button mdl-js-button mdl-button--raised mdl-button--colored mdl-color--accent"
        type="submit" id="post-btn">Post!
</button>
```

submit listener on the form

feed.js, handles all the post related things

```js
var form = document.querySelector('form');
```

```js
var form = document.querySelector('form');
var titleInput = document.querySelector('#title');
var locationInput = document.querySelector('#location');
```



register an eventlistener


```js
form.addEventListener('submit', function(event) {
  event.preventDefault();

  if (titleInput.nodeValue.trim() === '' || locationInput.nodeValue.trim() === '') {
    alert('Please enter valid data');
    return;
  }

  // close modal
  closeCreatePostModal();
  // register background sync request
})
```




















