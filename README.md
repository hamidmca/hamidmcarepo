Redis Cache in .net Core

Redis is a tip top flowed store. It's phenomenal for taking care of data that you will 
require again and again in a brief time allotment when you would favor not to use dealing with ability
 to "make" that data yet again. Think figuring or generous SQL request for data that doesn't change normally.

The primary thing you have to do is include the Redis reserving bundle given by Microsoft. 
You can do this in your bundle director reassure by running :
In your startup.cs, you now need to add the following to your ConfigureServices method.
 It should look something like :

 Microsoft.Extensions.Caching.Redis

public void ConfigureServices(IServiceCollection services)
{
 services.AddMvc();
 
 services.AddDistributedRedisCache(option =>
 {
 option.Configuration = "127.0.0.1";
 option.InstanceName = "master";
 });
}

For your Configuration, while I've hardcoded this to 127.0.0.1, you can clearly change this to pull from your arrangement as required.
 Either from AppSettings/ConnectionStrings and so on. 

What's more, as you can presumably figure with the technique mark of "AddDistributedRedisCache", 
you can likewise utilize things like SQL or InMemory reserves utilizing a comparable kind of strategy. We will go over this in future posts! 

AddDistributedRedisCache really adds an interface amazingly to your administration accumulation called
 "IDistributedCache" that you would then be able to use to set and recover values. 
You would then be able to utilize controller reliance infusion to go anyplace in your application.
 So state we have a controller called HomeController and it needs to utilize the RedisCache. It would go :

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
