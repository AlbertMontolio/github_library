180_customizing_the_service_worker.md

now lets dive into routing

for certain url, i want to cache the response

we did that before,

in sw.js

addeventlistener fetch
var url = https

we did something special for a particular url

we need to adjust our configuration

i can not write in the service-worker.js, because it will be overwritten

thats why i create a sw-base.js

we can do the same that we did with

```js
  "generate-sw": "workbox generateSW workbox-config.js"
```

we change it for

```js
  "generate-sw": "workbox inject:manifest"
```

this means, take an existing file

analyse all my assets and create const fileManifest = [
  etc.
]

and inject it in my newly created file sw-base.js

we need to add in sw-base.js



```js
module.exports = {
  "globDirectory": "public/",
  "globPatterns": [
    "**/*.{html,ico,json,css,js}",
    "src/images/*.{jpg,png}"
  ],
  "swSrc": "public/sw-base.js",
  "swDest": "public/service-worker.js",
  "globIgnores": [
    "../workbox-cli-config.js",
    "help/**"
  ]
};
```











