90_post_request__and_cache_api.md


a case, in the feed.js

we have a fetch request, its a get request

what if its a post request

```js
fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  },
  body: {
    message: JSON.stringify({
      message: 'Some message'
    })
  }
})
```


the post request is stored. theoretically we can't store a post request in the cache

its storing the response of the post request. not the post request itself

if go offline, post fails.

we can't send it, we haveno internet connection

































