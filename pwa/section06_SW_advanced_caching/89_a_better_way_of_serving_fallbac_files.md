89_a_better_way_of_serving_fallbac_files.md


improvement in this line

```js
if (event.request.url.indexOf('/help')) {
```

if we add more pages, we have to do more if statements

instead of checking the url, lets check the headers

the incoming request, excepts text/html as an answer

```js
.then(function(cache) {
  if (event.request.get('accept').includes('text/html')) {
    return cache.match('/offline.html');
  }
});
```

we can have a dummy image,

























