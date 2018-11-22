

we have files with no sw. we need to register them on our own

close the tabs, otherwise we see the old caching of our old project, not the new assignment

application, clear sotrage, clear site data

remove sw and clear the cache

1) register a service worker

2) identify the appshell, the core assets your app requires

3) precache the appsheel

4) add code to fetch the precached assets
intercept fetch events

5) precache more files, icons?????????

6) chaing styling, you see that its not loaded.

7) make sure to clean up unused caches

8) add dynamic caching




max answer -----------------------------


in the public folder,

sw.js

we register it in main.js

first, that the browser supports the feature
if serviceworker in navigator

if thats the case, we register the service worker, pointing to the sw file

we can use a promise, to output that swe succesfully resgistered the service worker

---------------

identify the appshell

we need to import the font roboto

the icons,well hit some problems fixed with dynamic cache

app.css,

etc.

the index.html, or the /

lets cache this, in the sw.js

install event, intall precache assets


we access caches, the cache storage api

event.waitUntil, that we wait this to finish

cache.addAll([

]);

we put all, /, /index.html

etc.

---------------------------------

fetch this precached assets


if i dont' find the object in my cache, i am done.
so, we need to check if response exist
if we have the response, we return the response, i.e., the asset in cache
if not, we feetch it from the internet

event.request

------------------------------------------

add the other asset



































































