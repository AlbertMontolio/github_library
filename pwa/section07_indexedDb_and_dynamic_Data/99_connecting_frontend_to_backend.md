99_connecting_frontend_to_backend.md


lets fetch data from database

we want to fetch all posts, and loop

feed.js

```js
var url = 'https://pwagram-50bea.firebaseio.com/posts.json';
```



sw.js

```js
self.addEventListener('fetch', function(event) {
var url = 'https://pwagram-50bea.firebaseio.com/posts.json';
```

feed.js

```js
fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkDataReceived = true;
    console.log('from web', data);
    clearCards()
    createCard();
  });
```


let's create a card with the server data

lets create dafür a new function

```js
function updateUI(data) {
  for (var i = 0; i < data.length; i++ ) {
    createCard(data[i]);
  }
}
```

```js
function createCard(data) {
```

```js
  cardTitle.style.backgroundImage = 'url(' + data.image + ')';
```

```js
  cardTitleTextElement.textContent = data.title;
```

```js
  cardSupportingText.textContent = data.location;
```

```js
fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkDataReceived = true;
    console.log('from web', data);
    clearCards()
    updateUI(data);
    // createCard();
  });
```

but data is not an array, is an object

two ways of fixing this

we can use a for loop

convert data into an array

```js
fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkDataReceived = true;
    console.log('from web', data);
    var dataArray = [];
    for (var key in data) {
      dataArray.push(data[key]);
    }
    clearCards()
    updateUI(dataArray);
    // createCard();
  });
```

you can move clearCards to updateUI()

```js
if ('caches' in window) {
  caches.match(url)
    .then(function(response) {
      if (response) {
        return response.json();
      }
    })
    .then(function(data) {
      console.log('from cache', data);
      if (!networkDataReceived) {
        var dataArray = [];
        for (var key in data) {
          dataArray.push(data[key]);
        }
        updateUI();
      }
    })
}
```



































