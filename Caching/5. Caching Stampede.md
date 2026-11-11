# Caching Stampede Or Thundering Herd
When multiple concurrent requests access the same uncached or expired key, all of them experience a cache miss and independently query the database. This creates unnecessary database load. We can prevent it using mechanisms like SemaphoreSlim or distributed locking.

Ek application mein Cache-Aside implement hai. 100 concurrent requests ek hi uncached product ko access karti hain. Sabhi requests Redis mein cache miss dekhti hain aur SQL Server ko query bhej deti hain. Is phenomenon ko kya kehte hain? Cache stampede


# How do you handle Cache Stampede?
"Cache Stampede occurs when multiple concurrent requests try to fetch the same expired or missing cache entry, resulting in multiple database queries.

To prevent this, we can use SemaphoreSlim with double-checked caching. It ensures that only one request fetches the missing data from the database while other requests wait. Once the first request populates the cache, the waiting requests retrieve the data from Redis instead of querying the database again."

let's implement Cache Stampede prevention in your existing `GetAllProductsAsync()` using `SemaphoreSlim`.

We'll keep your existing Cache-Aside logic and add only the required locking mechanism.

### Updated GetAllProductsAsync()

```
private static readonly SemaphoreSlim _lock = new(1, 1);

public async Task<IEnumerable<ProductResponse>> GetAllProductsAsync()
{
    const string cacheKey = "all_products";

    // 1. Check Redis
    var cacheData = await _cache.GetStringAsync(cacheKey);

    if (cacheData != null)
        return JsonSerializer.Deserialize<List<ProductResponse>>(cacheData)!;

    // 2. Acquire lock
    await _lock.WaitAsync();

    try
    {
        // 3. Check Redis again
        cacheData = await _cache.GetStringAsync(cacheKey);

        if (cacheData != null)
            return JsonSerializer.Deserialize<List<ProductResponse>>(cacheData)!;

        // 4. Fetch from SQL Server
        var products = await _productRepository.GetAllProductsAsync();

        var response = _mapper.Map<List<ProductResponse>>(products);

        // 5. Store in Redis
        await _cache.SetStringAsync(
            cacheKey,
            JsonSerializer.Serialize(response),
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
            });

        return response;
    }
    finally
    {
        // 6. Release lock
        _lock.Release();
    }
}
```

### How does it work?

Suppose 100 requests arrive simultaneously:

Initially: Redis has no `all_products` key.

100 requests → Cache MISS

Request 1 acquires lock

Request 1 fetches products from SQL Server and saves them in Redis

Other requests acquire lock one by one and check Redis again

Only one SQL query for this cache miss.

### Why do we check Redis twice?

- First check: Avoid acquiring the lock when data is already cached.
- Second check: Another request might have populated Redis while this request was waiting for the lock.

## 1. TTL Jitter – Spread expiration times

Problem: Multiple cache entries expire at the same time, causing many requests to hit the database together.

Solution: Add a random time to the cache TTL.

Pseudocode:

```
Generate random TTL (25–35 minutes)

Store data in Redis with that TTL
```

Example:

- Product A → 25 minutes
- Product B → 32 minutes
- Product C → 28 minutes

Result: Cache entries expire at different times, reducing simultaneous cache misses.

## 2. Locking – Control concurrent requests

Problem: 100 requests access the same cache key when it expires. All 100 try to fetch data from the database.

Solution: Allow only one request to fetch data while others wait.

Pseudocode:

```
Check Redis cache

IF data exists:
    Return cached data

ELSE:
    Acquire lock

    Check Redis cache again

    IF data exists:
        Return cached data

    ELSE:
        Fetch data from database
        Store data in Redis

    Release lock
    Return data
```

Result: Only one request fetches data from the database; waiting requests use the newly populated cache.

Remember for interviews:

- TTL Jitter: Prevents many cache entries from expiring simultaneously.
- Locking: Prevents multiple requests from fetching the same missing data simultaneously.


<img width="1752" height="897" alt="image" src="https://github.com/user-attachments/assets/4f98ad07-03f1-4941-abf9-c3d55fa0cdf8" />

