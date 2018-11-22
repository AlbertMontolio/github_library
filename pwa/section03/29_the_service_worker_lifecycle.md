

index.html loads app.js


app.js registers sw.js

it is registered as a background process

is installed at the browser -> install event

after installed, it can be activated

once activated, service worker now controls all pages of Scope

service workers are installed if thy changed or page for first time, not in every refresh

once sw is activated, it enters in idle mode, it listens

waiting for events, like a fetch event

is serviceworker ready? webpage






















































