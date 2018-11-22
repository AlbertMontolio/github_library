100_dynamic_caching_vs_caching_dynamic_content.md

what is so special about dynamic content

why cache api is not the best choice

difference

dynamic caching we did that fast

the page makes a request, it is intercept by the sw. if it its not find, it goes to the internet and fetches it. it stores it in the cache. and it gives back the response to the page

caching dynamic content

dynamic caching whenever you dont have static files, but you add a response while you fetch sth

store received responses in Cache, so that they can be used in the future

Caching dynamic content ---------------

it works similarly, but we dont use the cache api, we use a new tool, the indexedDb

a key-value database

it runs in the browser and it helps you store, data

we store json data

we dont store files, assets, images

why we use indexedDb then

data changes frequently (=Dynamic) AND typically is in JSON Format

we can store a part of the response, we can transform the data

the approach is comparable (fetch data and store for retrieveal in the future)

the data nature and format is different




























