
for post, we past a second argument to fetch

```js
fetch('https://httpbin.org/post', {
  method: 'POST',
  headeres: {
    'Content-Type': 'application/json', // what we are sending
    'Accept': 'application/json' // what we accept back
  },
  body: { message: 'does this work?' }
})
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


the response is empty, cuz we are sending a js object, and we said that we would send a json

so,

```js
  body: JSON.stringify({ message: 'does this work?' })
```




































