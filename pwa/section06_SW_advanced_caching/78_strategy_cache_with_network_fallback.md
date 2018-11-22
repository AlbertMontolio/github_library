78_strategy_cache_with_network_fallback.md

what we've done so far is, we try to access the cache, and if we dont find the file, we access the network

so, our page sends a request to the sw, in other words, the sw intercepts any request that our page makes

the sw, has a look at the cache. if a resource is found, it is returned

if not, we reach out the network

the network then returns the response

with dynamic caching, we store also this response


good thing is, we load assets from the cache, even if we have internet connection, this is quicker

the bad thing, we parse everything like that, for some resources, this is bad, cuz we dont reach the network by default, first always the cache. so if we find it, we dont reach the networ, therefore, we can show outdated content


lets use another strategy



















