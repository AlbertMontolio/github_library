questions.md

lecture 76 ----------------------------

albert: the sw is getting big. is it a way with plain js to split the code in multiple js files? or we should use webpack to achieve this?


max: You can of course split it. Either via a build tool/ bundler like Webpack (which will therefore yield one file again at the end) or via importScripts() (as we'll do it throughout the course).

Max

---------------------------

lecture 83:

albert: you said we need to return res, so that it reaches the feed.js. I don't get it. i think it's enough if we cache.put, if we store it in the cache. in feed.js we are also reaching the cache or the network by need.



could you please clarify this issue, i am a little bit confused. other than that, amazing course, as always

max:

Hi Albert,

To which minute mark/ snippet are you referring?

In general, you need to return the response when "capturing" it in the SW. Otherwise you store it in the cache but the page itself (which requested it originally) never receives it.

Max

------------------------------------


lecture 84:

albert:


so, we use this strategy, whenever we have dynamic data? whenever we are reaching to an api endpoint, that usually gives a list right?

we check if the request that we are doing, is the url api, if it is, we store it in the dynamic cache, if not, we do the normal one dynamic cache.

I am talking about the sw.



in the js file, we fetch url and caches, store the last that is finished (clearing up cards), and with the particularity that if networkDataReceived is true, we of course do not overwrite the info with the old cache info.



any missunderstandings in my thoughts? mostly in the first part?



thanks a lot


max:

Hi Albert,

This is correct - this strategy is great for assets which you want to serve up quickly whilst ensuring that you (at least at some time) show the latest version.

Max

----------------------------------------

lecture: 86

albert:

The match() method of the Cache interface returns a Promise that resolves to the Response associated with the first matching request in the Cache object. If no match is found, the Promise resolves to undefined.



just to understand, the event.request, gives us the url of the request, for example an image, a css file whatever.

the match method, fetches this asset from the cache, meaning, it goes to the cache, looks for the file, and gives us back a response. since this could take a while, a promise is used for this method. promis is done, then, we analyse if we have a response or not (undefined).  in case undefined, we fetch the request in the internet, and we store it with cache.put



we use this for dynamic cache right? for the assets that we didn't precached. assets that will be cached dynamically, while the user visits new pages.



All good? or any wrong explanation?



max:

You're right regarding how match() works, yes.

But we don't just use it for the dynamic cache, we also use it for the statically cached items we added to the cache.

Max



























































