




browser that does not accept pwa, just ignore the link manifest.json in the index.html header

since safaria, or other browsers, ignore the manifest, we need to pass the properties manually

```html
<link rel="manifest" href="/manifest.json">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="PWAGram">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-57x57.png" sizes="57x57">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-60x60.png" sizes="60x60">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-72x72.png" sizes="72x72">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-76x76.png" sizes="76x76">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-114x114.png" sizes="114x114">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-120x120.png" sizes="120x120">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-144x144.png" sizes="144x144">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-152x152.png" sizes="152x152">
<link rel="apple-touch-icon" href="/src/images/icons/apple-icon-180x180.png" sizes="180x180">
```

adding properties for internet explorer

```html
<meta name="msapplication-TileImage" content="/src/images/icons/app-icon-144x144.png">
<meta name="msapplication-TileColor" content="#fff">
<meta name="theme-color" content="#3f51b5">
```

we have to do this in all the pages we are loading

Useful resources & useful links

Web App Manifest - Browser Support: http://caniuse.com/#feat=web-app-manifest
MDN Article on the Web App Manifest (includes List of all Properties): https://developer.mozilla.org/en-US/docs/Web/Manifest
A detailed Web App Manifest Explanation by Google: https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/
More about the "Web App Install Banner" (including Requirements): https://developers.google.com/web/fundamentals/engage-and-retain/app-install-banners/









