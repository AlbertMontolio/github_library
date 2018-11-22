50_fetch_and_CORS.md

another option when posting data

mode:cors (default one)

the response needs to include cors header

what we did, if we go to network, to post

we see

post, headers,

response headers
- access-control-allow-credentials: true
- access-control-allow-origin: http://localhost:8000

this is set by the browser

you are not allowed to read the content of the response, if you don't send the request from localhost 8080

to avoid this, we can set the mode to mode: 'no-cors'


```js
fetch('https://httpbin.org/post', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json', // what we are sending
    'Accept': 'application/json' // what we accept back
  },
  mode: 'no-cors',
  body: JSON.stringify({ message: 'does this work?' })
})
```

we shouldnt use no-cors, just if we get an error, cuz we cant read the response, from a server

now, we see response bdoy null. we can't access

just if it was an image, we'd do it...




























