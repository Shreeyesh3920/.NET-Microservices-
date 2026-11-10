<img width="880" height="150" alt="image" src="https://github.com/user-attachments/assets/8295949d-a12b-410b-b404-d794ae94a305" />

TTL (Time To Live) means how long cached data should remain valid before it expires.

For example, in your ProductServiceAPI:

- You fetch all products from SQL Server.
- Store the result in Redis.
- Set TTL to 30 minutes.
- After 30 minutes, Redis expires that cache entry.
- The next request gets a cache miss, fetches fresh data from SQL Server and stores it again.

## 1. Understand with an example

Product cache — TTL: 30 minutes

1. 10:00 AM — Cache created

   Cache HIT

   Redis stores product data with a 30-minute TTL.
2. 10:15 AM — User requests products

   Cache HIT

   Redis returns cached data. Remaining TTL: 15 minutes.
3. 10:30 AM — TTL expires

   Cache MISS

   Redis expires the key.
4. 10:31 AM — Next request

   Application fetches products from SQL Server and caches them again.

Important: TTL doesn't automatically fetch fresh data from the database. It only controls cache expiration.

## 2. How to implement TTL in .NET using Redis?

Since you're using `IDistributedCache`, .NET provides `DistributedCacheEntryOptions` to configure expiration.

Step 1: Inject IDistributedCache

```
private readonly IDistributedCache _cache;

public ProductService(IDistributedCache cache)
{
    _cache = cache;
}
```

Step 2: Set TTL while storing data

```
public async Task<string> GetProductsAsync()
{
    string cacheKey = "all_products";

    // Check Redis first
    var data = await _cache.GetStringAsync(cacheKey);

    if (data != null)
        return data;

    // Cache miss: fetch from database
    var products = "Products from SQL Server";

    // Store in Redis with 30-minute TTL
    await _cache.SetStringAsync(
        cacheKey,
        products,
        new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
        });

    return products;
}
```

The important part is:

```
AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
```

This sets the TTL to 30 minutes from when the cache entry is created.

## 3. Two types of expiration

| Type                | Behaviour                                               |
| ------------------- | ------------------------------------------------------- |
| Absolute Expiration | Expires at a fixed time, regardless of access.          |
| Sliding Expiration  | Expiration timer resets whenever the entry is accessed. |

Absolute Expiration example:

```
AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
```

Cache created at 10:00 → expires at 10:30, even if accessed at 10:25.

Sliding Expiration example:

```
SlidingExpiration = TimeSpan.FromMinutes(30)
```

Cache created at 10:00 → accessed at 10:20 → expiration moves to 10:50.

If nobody accesses it for 30 minutes, it expires.

## 4. Absolute vs Sliding visually

Absolute expiration

10:00 Created

10:30 Expires

Accessing the cache does not extend its lifetime.

Sliding expiration

10:00 Created

10:50 Expires

Access at 10:20 resets the 30-minute inactivity timer.

## 5. Which one should you use?

For your ProductServiceAPI:

- Absolute expiration: Good for product lists that you want to refresh periodically.
- Sliding expiration: Useful for data that should stay cached while actively accessed.

My recommendation: Start with Absolute Expiration of 30 minutes, and invalidate `all_products` after successful Add, Update, or Delete operations.

Remember: TTL controls how long data stays in cache. Cache invalidation controls when your application deliberately removes cached data.


## Why do we invalidate (Delete) the cache instead of updating it directly?

- Ek simple example: Do users simultaneously ek hi student's marks update kar rahe hain.

Initial Redis cache:

```
Rahul = 70
Priya = 80
```

- Request A: Rahul ke marks 70 → 90.
- Request B: Priya ke marks 80 → 95.

Dono Redis se same old data read karte hain.

Request A ki copy:

```
Rahul = 90
Priya = 80
```

Request B ki copy:

```
Rahul = 70
Priya = 95
```

Ab A apni copy Redis mein save karta hai. Uske baad B apni copy save karta hai.

Final Redis:

```
Rahul = 70  ❌
Priya = 95 ✅
```

Rahul ka update lost ho gaya!

Isliye hum kya karenge?

Database mein successful update ke baad Redis ki puri cached entry delete kar denge.

Next request database se latest data lekar Redis mein fresh cache create karegi.

Bas bhai, isi wajah se humare current Cache-Aside implementation mein update karne ke bajaye cache invalidate karna prefer karte hain.
