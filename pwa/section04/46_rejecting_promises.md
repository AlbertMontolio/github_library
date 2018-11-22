46_rejecting_promises.md



```js
var promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    // resolve('This is executed once the timer is done!')
    reject({code: 500, message: 'An error ocurred!'})
    // console.log('This is executed once the timer is done!');
  }, 3000);
});
```


we are not catching the error

we could handle this, in the first then, passing a second argument

```js
// called whenever resolve or reject is called
promise.then(function(text) {
  return text;
}, function(err) {
  console.log(err.code, err.message);
}).then(function(newText) {
  console.log(newText);
});
```

one way of handling promise rejection

we add catch just

```js
// called whenever resolve or reject is called
promise.then(function(text) {
  return text;
}).then(function(newText) {
  console.log(newText);
}).catch(function(err) {
  console.log(err.code, err.message);
});
```

we don't see the undefined, cuz the catch catches teh first error
















