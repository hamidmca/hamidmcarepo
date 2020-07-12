Redis Cache in .net Core


// Set cache 
 [HttpGet]
        [Route("m1")]
        public async Task GetMessages1()
        {
            var currentTimeUTC = "Test Cache A1";
            byte[] encodedCurrentTimeUTC = Encoding.UTF8.GetBytes(currentTimeUTC);
            var options = new DistributedCacheEntryOptions()
                .SetSlidingExpiration(TimeSpan.FromDays(20));
            await _cache.SetAsync("cachedTimeUTC", encodedCurrentTimeUTC, options);

        }

// get cache
        [HttpGet]
        [Route("m2")]
        public async Task GetMessages2()
        {
            var encodedCachedTimeUTC = await _cache.GetAsync("cachedTimeUTC");

            if (encodedCachedTimeUTC != null)
            {
                var CachedTimeUTC = Encoding.UTF8.GetString(encodedCachedTimeUTC);
            }

            var s1 = await _cache.GetAsync("s1");

            if (s1 != null)
            {
                var s11 = Encoding.UTF8.GetString(s1);
            }
        }
    }


A couple more notes. 

IDistributedCache has async techniques. You should utilize these in all situations that are conceivable. 

IDistributedCache takes into consideration putting away either string qualities or byte esteems.
 In the event that you need to serialize an article and store the whole thing, you can either 
serialize it to bytes and spare it as bytes, or serialize it to JSON and spare it as a string in the event that you like. 

As talked about before, while this is a Redis conveyed store,
 there are different executions accessible that pursue the careful example for InMemory, SQL Server and so on.
