
```js
{
  "name": "Instagram as Progressive Web App", // long name of app (e.g. on splashscreen)
  "short_name": "PWAGram", // short name of app (e.g. below icon)
  "icons": [ // configure icons (e.g. on homescreen)
    {
      "src": "/src/images/icons/app-icon-48x48.png",
      "type": "image/png",
      "sizes": "48x48"
    },
    {
      "src": "/src/images/icons/app-icon-96x96.png",
      "type": "image/png",
      "sizes": "96x96"
    },
    {
      "src": "/src/images/icons/app-icon-144x144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "/src/images/icons/app-icon-192x192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/src/images/icons/app-icon-256x256.png",
      "type": "image/png",
      "sizes": "256x256"
    },
    {
      "src": "/src/images/icons/app-icon-384x384.png",
      "type": "image/png",
      "sizes": "384x384"
    },
    {
      "src": "/src/images/icons/app-icon-512x512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/index.html", // which page to load on startup
  "scope": ".", // which pages are included in PWA Experience
  "display": "standalone", // should it look like a standolne app?
  "orientation": "portrait-primary", // set (and force) default orientation
  "background_color": "#fff", // background whilst loading and on splashscreen
  "theme_color": "#3f51b5", theme color (e.g. on top bar in task switcher)
  "description": "A simple Instagram Clone, implementing a lot of PWA love.", (e.g. as favorite)
  "dir": "ltr", // read direction of your app
  "lang": "en-US" // main language of app
}
```

launch pwa in browser


we need a dev server to simulate that our app runs on a server

```js
  "devDependencies": {
    "http-server": "^0.10.0"
  }
```

first of all, we need to

```js
npm start
```





























