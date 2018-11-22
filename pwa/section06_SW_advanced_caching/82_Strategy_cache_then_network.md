82_Strategy_cache_then_network.md

this strategy is usefull

get an asset as quickly as possible from the cache, and then, try to find an updated one from the network


page, directly access to the cache, not to the sw

like with the save button.

and cache gives the asset back. no sw

we will check with a variable, if the network response is quicker, cuz we wouldnt want to overwrite then

we also, simultaneously, reach the sw. as the page is reaching to the cache

the sw, reach out to the network, and try to get a response from there
the network gives back a response to the sw.
the sw stores the data in the cache (only if we use dynamic cache)

and then, sw, we return the data to the page



lets start in the feed.js

where we fetch some data from the internet


```js
fetch('https://httpbin.org/get')
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    createCard();
  });
```


json should not be stored in cache. later well improve it

we first check if we can access the cache

```js
if ('caches' in window) {

}
```

before fetch('https')

i try to see if i have access in the cache, to the url

```js
var url = 'https://httpbin.org/get';
if ('caches' in window) {
  caches.match(url)
}
```

you check if you have it in your cache


if i have data, i want to use it, ihandle the response of that

if response, it could be null, if we dont find it

i transform the response to json, so i have data
we can create a card

```js
var url = 'https://httpbin.org/get';
if ('caches' in window) {
  caches.match(url)
    .then(function(response) {
      if (response) {
        return response.json();
      }
    })
    .then(function(data) {
      console.log(data);
      createCard();
    })
}
```

we also need to do a fetch to the url, we put it before the if ('caches' in window)

it will not be done immediately, its a request to the network

```js
var url = 'https://httpbin.org/get';

fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    console.log('from web', data);
    createCard();
  });

if ('caches' in window) {
  caches.match(url)
    .then(function(response) {
      if (response) {
        return response.json();
      }
    })
    .then(function(data) {
      console.log('from cache', data);
      createCard();
    })
}
```

we have one issue with the fetch to the url

we see it in the browser

i have two cards!!

one from the cache, and one from the web

in the console

```js
from cache Object
from web Object
```

we append a new card, we dont want that

the biggest problem is, if network is super fast, the cache, later, will overwrite the info

lets create a variable for that

```js
var url = 'https://httpbin.org/get';
var networkDataReceived = false;
```

i set it to true, whenever i get the data from the fetch(url) request

```js
fetch(url)
  .then(function(res) {
    return res.json();
  })
  .then(function(data) {
    networkDataReceived = true;
    console.log('from web', data);
    createCard();
  });
```


in the cache, i just want to create a card, if the networkDataReceived is false

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
        createCard();
      }
    })
}
```

now lets fix the issue of appending twice.

in createCard() we create the card

we should create a method where, before createCard, we clear everything

function clearCards()
use a while loop, if we have childNodes, i remove the last one

```js
function clearCards() {
  while(sharedMomentsArea.hasChildNodes()) {
    sharedMomentsArea.removeChild(sharedMomentsArea.lastChild);
  }
}
```

we call it before createCard()

```js
var url = 'https://httpbin.org/get';
var networkDataReceived = false;


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
        clearCards()
        createCard();
      }
    })
}
```


now we just have two cards.

















































