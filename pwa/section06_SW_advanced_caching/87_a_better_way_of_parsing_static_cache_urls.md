

```js
function isInArray(string, array) {
  for (var i = 0; i < array.length; i++) {
    if (array[i] === string) {
      return true;
    }
  }
  return false;
}
```

```js
  } else if (isInArray(event.request.url, STATIC_FILES)) {
```



