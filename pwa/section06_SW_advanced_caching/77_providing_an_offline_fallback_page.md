
if we are offline, navigate to help, its not working
cuz the user was no there, it was not dynamically cached

we need a fallback for this pages


hej sorry, this is not offline, but provide a link to go to a site that was cached

we created a file for that, in public

offline.html

we copy the index.html

cuz we want to stay in the same frame, same app shell

i dont need the feed.js

```html
<script src="/src/js/feed.js"></script>
```



we delete the create post div

```html
<div id="create-post">
```

delete:

```html
<img src="/src/images/main-image.jpg"
  alt="Explore the City"
  class="main-image">
```

delete

```html
<div id="shared-moments"></div>
```

```html
<div class="floating-button">
  <button class="mdl-button mdl-js-button mdl-button--fab mdl-button--colored"
          id="share-image-button">
    <i class="material-icons">add</i>
  </button>
</div>
<div id="confirmation-toast" aria-live="assertive" aria-atomic="true" aria-relevant="text" class="mdl-snackbar mdl-js-snackbar">
  <div class="mdl-snackbar__text"></div>
  <button type="button" class="mdl-snackbar__action"></button>
</div>
```



now we have a bare minimum page


```html
<div class="page-content">
  <h5 class="text-center mdl-color-text--primary">We're sorry, this page hasn't been cached yet :/</h5>
  <p>But why don't you try one of our <a href="/">other pages</a>? </p>
  <div id="shared-moments"></div>
</div>
```


we want to show this page if the user visits a page that was not cached

we can control this in the sw

we need a new static cache version

and add the offline.html to the precache files

```js
cache.addAll([
  '/',
  '/index.html',
  '/offline.html',
```

we need to change sth in our fetch response

we try to fetch sth from the cache, and if it fails, we try to fetch from the network (in the eventlistener fetch). if there is no network though, and it fails, we catch, but we dont do anything

i want to return sth

i want to open my static cache, in the then function i have access to the cache
here i want to get the offline.html, we use match for that

this returns the value if it finds it

```js
.catch(function(err) {
  return caches.open(CACHE_STATIC_NAME)
    .then(function(cache) {
      return cache.match('/offline.html');
    });
});
```








































