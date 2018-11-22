

open the terminal

in the folder project

```js
npm install --save-dev workbox-cli
```


lets write a little script to use it

generate-sw: "workbox"

```js
  "scripts": {
    "start": "http-server -c-1",
    "generate-sw": "workbox generateSW"
  },
```

```js
npm run generate-sw
```

https://developers.google.com/web/tools/workbox/guides/generate-service-worker/cli

workbox wizard


root of our app? public

which kind of file you want to precache? all enabled

where the new sw should be store?

public/service-worker.js

we dont want to overwrite the sw.js that we already have


we have a new file!

workbox-cli-config.s

```js
module.exports = {
  "globDirectory": "public/",
  "globPatterns": [
    "**/*.{html,ico,json,css,png,jpg,js}"
  ],
  "swDest": "public/service-worker.js"
};
```

and inside public, we have service-worker.js
workbox-sw.prod.v2.0.0.js

```js
workbox generateSW workbox-config.js
```


public/service-worker.js

```js
/**
 * Welcome to your Workbox-powered service worker!
 *
 * You'll need to register this file in your web app and you should
 * disable HTTP caching for this file too.
 * See https://goo.gl/nhQhGp
 *
 * The rest of the code is auto-generated. Please don't update this file
 * directly; instead, make changes to your Workbox build configuration
 * and re-run your build process.
 * See https://goo.gl/2aRDsh
 */

importScripts("https://storage.googleapis.com/workbox-cdn/releases/3.4.1/workbox-sw.js");

/**
 * The workboxSW.precacheAndRoute() method efficiently caches and responds to
 * requests for URLs in the manifest.
 * See https://goo.gl/S9QRab
 */
self.__precacheManifest = [
  {
    "url": "404.html",
    "revision": "0a27a4163254fc8fce870c8cc3a3f94f"
  },
  {
    "url": "favicon.ico",
    "revision": "2cab47d9e04d664d93c8d91aec59e812"
  },
  {
    "url": "help/index.html",
    "revision": "ec3f1f60a56f0d8c9e62d61d1ea68afa"
  },
```

in that file, we have a file manifest array, the files referenced by our project, file versions we see

if we change the files, and we run the generate.sw, it changes the id

at the bottom we see

```js
    "revision": "612276accd0c913f6681af4bc52d0ab6"
  },
  {
    "url": "sw.js",
    "revision": "3d92761fd70cf798972360535643ee96"
  }
].concat(self.__precacheManifest || []);
workbox.precaching.suppressWarnings();
workbox.precaching.precacheAndRoute(self.__precacheManifest, {});
```

create a new instance of the new workbox package

this will put all the files in the static cache (what we did manually)

it also makes a fetch listener

in app.js

we can register the new service worker

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
    .register('/service-worker.js')
```



if we clear the data and refresh,

we see in cache storage, that we dont have the cache static and dynamic,

we have workbox-precaching-revision

this caches all the files in the manifest

we lost many features, but we will go to them later



this precache function does a lot of work for us

but we miss the style, fonts etc.

workbox-cli-config.js

```js
module.exports = {
  "globDirectory": "public/",
  "globPatterns": [
    "**/*.{html,ico,json,css,png,jpg,js}"
  ],
  "swDest": "public/service-worker.js"
};
```


globDirectory where are the files that you want to cache live?

globPatterns? in any folder, in any filename, thsi extensions


check the docu to configure this build process




we dont want to cache all the files

so

```js
module.exports = {
  "globDirectory": "public/",
  "globPatterns": [
    "**/*.{html,ico,json,css,js}",
    "src/images/*.{jpg,png}" // <- just files, ** would mean folders
  ],
  "swDest": "public/service-worker.js",
};
```


```js
module.exports = {
  "globDirectory": "public/",
  "globPatterns": [
    "**/*.{html,ico,json,css,js}",
    "src/images/*.{jpg,png}"
  ],
  "swDest": "public/service-worker.js",
  "globIgnores": [
    "../workbox-cli-config.js",
    "help/**"
  ]
};
```



lets re run

```js
npm run generate-sw
```


all the icons are gone

we are still missing cdn, the fonts and styling

we will use a special feature, related to routing. once we navigate, we store stuff in a special cache










































































