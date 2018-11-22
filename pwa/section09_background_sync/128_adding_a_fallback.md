128_adding_a_fallback.md



what happens if we dont support service worker and syncmanager?

else statement,

```js
  } else {
    // send data direct to the server
    sendData();
  }
})
```


new function, sendData() sends directly the data to the backend.

we have a firebase project, a backend, we post data there

```js
var url = 'https://pwagram-50bea.firebaseio.com/posts.json';
```

```js
function sendData() {
  fetch('https://pwagram-50bea.firebaseio.com/posts.json', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      id: new Date().toISOString(),
      title: titleInput.value,
      location: locationInput.value,
      image: 'http://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/1966.png&w=350&h=254'
    })
  })
  .then(function() {
    console.log('Sent data', res);
    updateUI();
  });
}
```


