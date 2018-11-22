48_fetch_basics.md


fetch allows you to make http request

app.js


fetch page: https://httpbin.org


```js
fetch('https://httpbin.org/ip')
  .then(function(response) {
    console.log(response);
  });
```

if we want to consume the body, we need to do

.json, provide by fetch, it converts into js object



```js
fetch('https://httpbin.org/ip')
  .then(function(response) {
    console.log(response);
    return response.json();
  })
  .then(function(data) {
    console.log(data)
  });
```

if we misspell the url

```js

fetch('https://httpbin.org/ips')
  .then(function(response) {
    console.log(response);
    return response.json();
  })
  .then(function(data) {
    console.log(data)
  })
  .catch(function(err) {
    console.log(err);
  });
```














