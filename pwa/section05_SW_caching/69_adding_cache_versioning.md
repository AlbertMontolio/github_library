69_adding_cache_versioning.md

static cache is preocuppied with things we precache

dynamic cache, things that we store after we fetch them from the network

imagine that you cache a json, that changes over time

you dont want to have a old version

we dont want to cache jsons, cuz this reason

we want to cache files

we will fix this later

feed.js

where we create the card

if you change sth, like sanfranfico tip, we add some stlye color white

```js
var cardTitleTextElement = document.createElement('h2');
cardTitleTextElement.style.color = 'white';
```

it stays black

no new service worker is waiting to be stored. cuz we didnt change anything in the sw

we are fetching tthe asset from the cache, and in the cach, we still have the feed.js from before

we will change this assets, whenever we install a new sw, because we will do the precache again

but this is not happening now

one thing we can do is, add code to the sw to force the update

this is not what we want to do

# we want to manage cache versions

give to our cachces, names

we can name our cache static-v2

we store the precaches into our new sub cache

sw.js

```js
  caches.open('static-v2') // name we like
```

we dont want to break the application, thats why we change the name
the old one remains

later on we can delete the subcaches



we still have a problem, we have now multiple caches, static and static-v2, feed.js for example is in both

in the fetch,
```js
catches.match(event.request)
```

we check if the file is present in any of our caches













































