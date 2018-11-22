177_workbox_3_vs_2.md

Workbox is now available in two different versions: 2.x  and 3.x

There have been some major changes from 2.x  => 3.x , you find a detailed migration guide here: https://developers.google.com/web/tools/workbox/guides/migrations/migrate-from-v2

For this module, the following breaking change is important:

Use precacheAndRoute()  instead of precache()  in the sw-base.js  file

You don't have to update to 3.x if you don't want to. In order to get the same version as used in this module, you can simply install version 2:

```js
npm install --save-dev workbox-cli@^2
```













