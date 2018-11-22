


```js
var promise = new Promise(function(resolve, reject) {

});
```

promises gives us back weather resolve or reject

```js
var promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('This is executed once the timer is done!')
    console.log('This is executed once the timer is done!');
  }, 3000);
});
```


```js
var promise = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('This is executed once the timer is done!')
    // console.log('This is executed once the timer is done!');
  }, 3000);
});

// called whenever resolve or reject is called
promise.then(function(text) {
  console.log(text)
});
```

we can chain more then, with promises

```js
// called whenever resolve or reject is called
promise.then(function(text) {
  return text;
}).then(function(newText) {
  console.log(newText);
});
```
































