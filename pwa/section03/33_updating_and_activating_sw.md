33_updating_and_activating_sw.md

the activate block is missing

and, service worker registered before sw. since we have two threads, app.js and sw.js, we can't guarantee the order


if we see the browser, sw is waiting to be activated

the page may still be communicating with the old one, so if the new one is activated, it may break the page

therefore, in order to activate a sw, is, close the tab, and enter again

!!!
if you change code in sw, close tabs of your application
!!!

there are short cuts

you can click update on reload

















