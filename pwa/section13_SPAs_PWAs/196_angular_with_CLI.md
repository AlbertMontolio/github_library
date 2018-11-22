196_angular_with_CLI.md

application with one page, and two subpages, blogs and users

angular cli does not do pwa out of the box

we need to add a manifest, and add a sw

lets start with sw

lets go to .angular-cli.json

in the apps array

we add a new key, anywhere,
"serviceWorker": true

this tells the CLI that we want to create a serviceworker through it

we need to install an extra page

```js
npm install --save @angular/service-worker
```

with this, we can run a production build


sw are just added by prod versions

```js
ng build --prod
```


now we have this dist folder

we see a

- worker-basic
- sw-register
- ngsw-manifest.json


```js
{
  "external": {
    "urls": [
      {
        "url": "https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css"
      }
    ]
  },
  "dynamic": {
    "group": [
      {
        "name": "posts-images",
        "urls": {
          "https://firebasestorage.googleapis.com/": {
            "match": "prefix"
          }
        },
        "cache": {
          "optimizeFor": "performance",
          "maxAgeMs": 60000,
          "maxEntries": 40
        }
      }
    ]
  },
  "routing": {
    "index": "/index.html",
    "routes": {
      "/": {
        "prefix": false
      },
      "/users": {
        "prefix": true
      }
    }
  },
  "static": {
    "urls": {
      "/polyfills.b59a8422e26e64d12be4.bundle.js": "7895e804cfc07a59fe72e7e1159e781d95ef20fe",
      "/main.fe939ca49b2fac72f09b.bundle.js": "8c14321b0cdf9b164177c8597013701faba5640d",
      "/sw-register.6819cd2c5fa25470ecf2.bundle.js": "50d549edfbd188ed199d8d0b079afa593202e3ea",
      "/vendor.7fc5bbac6d1c03d26dbd.bundle.js": "a5ca8d7f3ec5f561867ab2ee6802a5152462e7df",
      "/inline.7b46d0d9574fa35568e4.bundle.js": "84dba3238557d833b41df58005b0a4e78ab20181",
      "/styles.d41d8cd98f00b204e980.bundle.css": "da39a3ee5e6b4b0d3255bfef95601890afd80709",
      "/assets/icons/app-icon-144x144.png": "f2cf08ae85af935cd970130a62c606154cda32d2",
      "/assets/icons/app-icon-192x192.png": "fe78f1d949398033a8df44613a4873b639bc9ee8",
      "/assets/icons/app-icon-256x256.png": "80ad5724bec7e84d86b6475e47e33685323e6bce",
      "/assets/icons/app-icon-384x384.png": "970b3aac2a24d54c4e504253cd6379951ed83cb9",
      "/assets/icons/app-icon-48x48.png": "ece59b4925fe04e5603e6db5f5f3cbd225099ba9",
      "/assets/icons/app-icon-512x512.png": "492d5d39521005cab9700c52bd860c49af607cfa",
      "/assets/icons/app-icon-96x96.png": "0aeb78c7a51ab2b690a327696b5ad4449b317151",
      "/assets/manifest.json": "69daffe416df23baed997e77da7355fb353db769",
      "/favicon.ico": "84161b857f5c547e3699ddfbffc6d8d737542e01",
      "/index.html": "eb734a08e88ea8fcc069e365129e08379bb0f578"
    },
    "_generatedFromWebpack": true
  }
}
```

we can see the static assets



this angular package, creates static cache. its a hash, the long number is the version. whenever the file changes, this number changes

sw-register just register the sw

worker-basic

ngsw-manifest.

we are missing the bootstrap import, the fonts etc.

it would be nice to set up our own routes

we can add a file, in our root directory

ngsw-manifest.json

this is a config file for the angular sw package instead

in this file, we can setup some configuration

external dependencies
we specify the url that we want to precache
urls, have to elements, a property, url, and then the actual url

```js
{
  "external": {
    "urls": [
      {"url": "https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css"}
    ]
  },
}
```


what if we have files that we dont want to precache, we do

static.ignore

```js
  "static.ignore": [
    "\\.js\\.map$"
  ],
```


dynamic routing!
dynamic key, set up our own routes and the rules

name, name of the cache you want to use
urls that should be part of this rule

-prefix, exact, or regex

```js
  "dynamic": {
    "group": [
      {
        "name": "posts-images",
        "urls": {
          "https://firebasestorage.googleapis.com/": {
            "match": "prefix"
          }
        },
```


what should happen if this rule is matched?

cache implication, optimieFor, decides the strategy: performance or fresshness

fresshness, reaches out to the network first,
performance reaches out to the cache first

we can limit the age, to define how long the cache value should be valid

```js
"cache": {
  "optimizeFor": "performance",
  "maxAgeMs": 60000,
  "maxEntries": 40
}
```

routing key

we can inform the sw package, our routes

we specify the index file, anda the routes we have

```js
"routing": {
  "index": "/index.html",
  "routes": {
    "/": {
      "prefix": false
    },
    "/users": {
      "prefix": true
    }
  }
}
```

manifest.js is missing

we add this in the assets folder
this folder is copy into dist. the src folder not

add manifest.json in assets


```js
{
  "name": "Angular as PWA",
  "short_name": "PWAngular",
  "icons": [
    {
      "src": "/assets/icons/app-icon-48x48.png",
      "type": "image/png",
      "sizes": "48x48"
    },
    {
      "src": "/assets/icons/app-icon-96x96.png",
      "type": "image/png",
      "sizes": "96x96"
    },
    {
      "src": "/assets/icons/app-icon-144x144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "/assets/icons/app-icon-192x192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/assets/icons/app-icon-256x256.png",
      "type": "image/png",
      "sizes": "256x256"
    },
    {
      "src": "/assets/icons/app-icon-384x384.png",
      "type": "image/png",
      "sizes": "384x384"
    },
    {
      "src": "/assets/icons/app-icon-512x512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/index.html",
  "scope": ".",
  "display": "standalone",
  "orientation": "portrait-primary",
  "background_color": "#fff",
  "theme_color": "#3f51b5",
  "description": "This is Angular as a PWA.",
  "dir": "ltr",
  "lang": "en-US"
}
```

we need to import the manifest.js in the index.html

```js
  <link rel="manifest" href="/assets/manifest.json">
```

```js
npm run build --prod
```

lets serve this in production mode

```js
ng serve --prod
```




















































