124_how_does_background_sync_work.md



your app wants to send data to server

but if we have no internet...

store the request, and send it later when we have internet

we can register sync task, to service worker

usually we have a json, so we store the data which should be sent in IndexedDB
(we can also store images in indexedDB)

if we have internet, sw executes the task immediately

connectiviety resestablished, event "sync" is triggered

sw fetches the data from indexeddb and sends it to the server

we can even close the browser, and it will work










