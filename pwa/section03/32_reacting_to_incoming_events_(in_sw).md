32_reacting_to_incoming_events_(in_sw).md

registration, the register method, we can pass a second argument, we can restrict

with

```js
  .register('/sw.js', {scope: '/help/'})
```

sw and manifest.json are not rlated, they are independent

sw only work with https (localhost is an exception)

sw.js

self access to sw
we add event listeners. we dont have dom access in the sw
we have the install event

event object

```js
self.addEventListener('install', function(event) {
  console.log('[Service Worker] Installing Service Worker ...', event);
})

self.addEventListener('activate', function(event) {
  console.log('[Service Worker] Activating Service Worker ...', event);
  return self.clients.claim();
})
```

self.clients is not necessary sometimes

in the browser

```js
Service worker registered!

[Service Worker] Installing Service Worker ... InstallEvent {isTrusted: true, type: "install", target: ServiceWorkerGlobalScope, currentTarget: ServiceWorkerGlobalScope, eventPhase: 2, …}
```

we don't see activating... this is crucial



















