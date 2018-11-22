

1 create a promise that wraps the setTimoout

we'll use resolve

return a fetch call

chain another, extract the data given by this url

extract json data

output.textContent

repeat the exercise, with a put request

solution:

albert's solution

```js
  var promise = new Promise(function(resolve, reject) {
    setTimeout(function() { // <- Store this INSIDE the Promise you created!
      // Resolve the following URL: https://swapi.co/api/people/1
      fetch('https://swapi.co/api/people/1')
        .then(function(response) {
          console.log("responding");
          console.log(response);
          return response.json();
        })
        .then(function(data) {
          console.log(data);
          output.textContent = data.name;
        })
    }, 3000);
  });
  ```


max's solution

```js
  var promise = new Promise(function(resolve, reject) {
    setTimeout(function() { // <- Store this INSIDE the Promise you created!
      // Resolve the following URL: https://swapi.co/api/people/1
      resolve('https://swapi.co/api/people/1');
    }, 3000);
  })
  .then(function(url) {
    return fetch(url);
  })
  .then(function(response) {
    return response.json();
  })
  .then(function(data) {
    output.textContent = data.name;
  });
```

```js
  fetch('https://httpbin.org/put', {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json', // what we are sending
      'Accept': 'application/json' // what we accept back
    },
    body: JSON.stringify({
      name: "Albert",
      age: 28
    })
  })
  .then(function(response) {
    console.log("responding");
    console.log(response);
    return response.json();
  })
  .then(function(data) {
    console.log(data.json.person.name);
  })
```











































