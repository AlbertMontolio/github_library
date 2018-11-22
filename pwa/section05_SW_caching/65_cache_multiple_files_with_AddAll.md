65_cache_multiple_files_with_AddAll.md

addAll, takes an array of strings

where each string represents a url of the request we wanna send

the sw, make the http request and store the request response key value pair in the cache storage


in the given cache we are opening here (static)

```js
'/src/js/promise.js',
'/src/js/fetch.js'
```

it does not make sense to cache this polyiflls, cuz we use this to support old browsers, and old browsers do not suppot sw


we leave them, cuz perfomrnace reasons

we have them already. with if statements we could solve this issue

```js
cache.addAll([
  '/',
  '/index.html',
  '/src/js/app.js',
  '/src/js/feed.js',
  '/src/js/promise.js',
  '/src/js/fetch.js',
  '/src/js/material.min.js',
  '/src/css/app.css',
  '/src/css/feed.css',
  '/src/images/main-image.jpg',
  'https://fonts.googleapis.com/icon?family=Material+Icons',
  'https://fonts.googleapis.com/css?family=Roboto:400,700',
  https://cdnjs.cloudflare.com/ajax/libs/material-design-lite/1.3.0/material.indigo-pink.min.css
])
```


we just load main-image.jpg in index.html

oh man, its working

just the fonts are missing

the icons are fetch differently

if we see our resources, icon?family=Mat

they have a

```css
@font-face {
  font-family: 'Roboto';
  font-style: normal;
  font-weight: 400;
  src: local('Roboto'), local('Roboto-Regular'), url(https://fonts.gstatic.com/s/roboto/v18/KFOmCnqEu92Fr1Mu72xKKTU1Kvnz.woff2) format('woff2');
  unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
}
```

they are calling a new url

we are caching the font defintion,but not this new call

would be nice that when the user visits a page, things are cached, we do now, dynamic caching











































































