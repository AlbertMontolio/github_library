44_async_code_in_javascript.md


javaascript is single-threaded!

fetch data from server, wait 3 seconds (setTimeout)

generic long runing operation





these proceses, one after the other

we just have on thread available

how to handle async code

we start the actions, and we wait for the respones

```js
setTimeout(function() {
  console.log('This is executed once the timer is done!');
}, 3000);

console.log('This is executed right after setTimeout()');
```

first console.log executed after the timer

this is good, when soon we can get into callback hell. nesting a lot of callbacks

that's why promises came up










