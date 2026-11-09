# Redis Notes (Easy Revision)

## 1. What is Redis?

**Redis (Remote Dictionary Server)** is an open-source, in-memory data
store.

-   It stores data mainly in **RAM**, so read/write operations are very
    fast.
-   It stores data using **keys and values**.
-   It supports data structures such as Strings, Hashes, Lists, Sets and
    Sorted Sets.
-   Redis can be used for caching, session storage, messaging and more.

Example:

``` text
Key              Value
product:101      iPhone
user:1           Shree
```

> **Remember:** Redis is an in-memory data store. Caching is one of its
> use cases; Redis is not only a cache.

## 2. What is caching?

Caching means keeping a temporary copy of frequently used data in faster
storage so that the application does not need to fetch the same data
from the main database repeatedly.

### Without caching

``` text
Request 1 -> .NET Application -> SQL Server
Request 2 -> .NET Application -> SQL Server
Request 3 -> .NET Application -> SQL Server
```

Every request queries the database.

### With caching (Cache-Aside pattern)

``` text
                 .NET Application
                        |
                  Check Redis
                   /       \
               Cache Hit   Cache Miss
                  |            |
             Return Data   Fetch from SQL
                               |
                         Store in Redis
                               |
                           Return Data
```

Steps:

1.  The application checks Redis for the requested data.
2.  If the data exists (**cache hit**), return it from Redis.
3.  If the data does not exist (**cache miss**), fetch it from the
    database.
4.  Store the fetched data in Redis.
5.  Return the data to the user.

Benefits: - Faster responses for repeated requests. - Fewer repeated
database queries. - Reduced load on the main database.

## 3. In-memory cache vs Redis vs distributed caching

These terms describe different things.

### In-memory cache in ASP.NET Core

`IMemoryCache` stores cached data in the memory of the application
process.

-   Each application instance has its own local cache.
-   Another application instance cannot directly access that local
    cache.
-   If the application process restarts, its in-process cache is lost.

### Redis

Redis runs as a separate server/process and stores data mainly in that
Redis server's RAM.

-   It is separate from the .NET application's memory.
-   Applications communicate with it over a network.
-   Redis can be used as a shared cache.

### Distributed caching

Distributed caching means multiple application instances can access a
shared cache.

Example: three .NET application servers use the same Redis server.

``` text
.NET App 1 -----\
.NET App 2 -------> Redis Server (RAM)
.NET App 3 -----/
```

All three applications can read and write shared cached data.

**Important terminology:** - **Centralized** describes the topology in
this example: one shared Redis server. - **Distributed caching**
describes the cache being shared by multiple application instances. - A
single Redis server can provide distributed caching. Redis Cluster is
not required. - **Redis Cluster** is a separate Redis deployment mode
that distributes/shards data across multiple Redis nodes.

## 4. Redis use cases

  -----------------------------------------------------------------------
  Use case                            Example
  ----------------------------------- -----------------------------------
  Caching                             Keep frequently requested product
                                      details in Redis

  Session storage                     Share session-related data across
                                      application servers

  Pub/Sub messaging                   An Order Service publishes an
                                      event; a Notification Service
                                      subscribes

  Distributed locking                 Help prevent multiple instances
                                      from processing the same protected
                                      operation at once

  Leaderboards                        Use Sorted Sets to rank players by
                                      score

  Rate limiting                       Count API requests per user/time
                                      window

  Queues and event processing         Use Lists or Streams for
                                      message/job workflows
  -----------------------------------------------------------------------

Notes: - Redis Pub/Sub does not retain messages for disconnected
subscribers. - Redis Streams support persisted messages and consumer
groups. - Distributed locks need careful expiry, ownership and failure
handling.

## 5. Redis data structures

  Data structure   Simple meaning                    Example use
  ---------------- --------------------------------- ------------------------
  String           A value associated with a key     Cache a JSON string
  Hash             Fields and values under one key   User profile
  List             Ordered collection of values      Queue-like workflows
  Set              Unique values                     Unique tags or members
  Sorted Set       Unique values with scores         Leaderboard

## 6. Using Redis in ASP.NET Core

A common approach is to use the `IDistributedCache` abstraction with the
Redis provider.

### Install package

``` bash
dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
```

If using `IConnectionMultiplexer` directly, also install:

``` bash
dotnet add package StackExchange.Redis
```

### appsettings.json

``` json
{
  "RedisCacheOptions": {
    "Configuration": "localhost:6379",
    "InstanceName": "MyApp:"
  }
}
```

`localhost:6379` means the Redis server is running on the same machine
as the application. In a deployed environment, use the actual Redis
host/connection configuration.

### Program.cs --- register distributed cache

``` csharp
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration =
        builder.Configuration["RedisCacheOptions:Configuration"];

    options.InstanceName =
        builder.Configuration["RedisCacheOptions:InstanceName"];
});
```

This registers an `IDistributedCache` implementation backed by Redis.

### Inject and use IDistributedCache

``` csharp
using Microsoft.Extensions.Caching.Distributed;
using System.Text.Json;

public class ProductService
{
    private readonly IDistributedCache _cache;
    private readonly IProductRepository _repository;

    public ProductService(
        IDistributedCache cache,
        IProductRepository repository)
    {
        _cache = cache;
        _repository = repository;
    }

    public async Task<List<ProductDTO>> GetAllProducts()
    {
        const string cacheKey = "products";

        // 1. Check Redis
        var cachedData = await _cache.GetStringAsync(cacheKey);

        if (cachedData is not null)
        {
            // Cache hit
            return JsonSerializer.Deserialize<List<ProductDTO>>(cachedData)!;
        }

        // 2. Cache miss: fetch from the database
        var products = await _repository.GetAllAsync();

        // Map to DTOs if required
        var productDtos = products
            .Select(p => new ProductDTO
            {
                Id = p.Id,
                Name = p.Name
            })
            .ToList();

        // 3. Store the result in Redis
        await _cache.SetStringAsync(
            cacheKey,
            JsonSerializer.Serialize(productDtos),
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
            });

        return productDtos;
    }
}
```

`IDistributedCache` provides common cache operations such as: -
`GetAsync()` / `GetStringAsync()` - `SetAsync()` / `SetStringAsync()` -
`RefreshAsync()` - `RemoveAsync()`

The cache API stores byte arrays; the string helper methods make string
values easier to work with. Objects are commonly serialized to JSON
before being stored.

## 7. Cache expiration

Expiration controls how long cached data remains available.

### Absolute expiration

The entry expires at a fixed time after it is created, even if it is
accessed frequently.

``` csharp
new DistributedCacheEntryOptions
{
    AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30)
}
```

Use it when data should be refreshed after a maximum period, such as
product prices or news.

### Sliding expiration

The inactivity timer resets when the entry is accessed. If it is not
accessed for the configured period, it expires.

``` csharp
new DistributedCacheEntryOptions
{
    SlidingExpiration = TimeSpan.FromMinutes(5)
}
```

Use it for data that should remain cached while it is actively used.

### Using both

``` csharp
new DistributedCacheEntryOptions
{
    AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30),
    SlidingExpiration = TimeSpan.FromMinutes(5)
}
```

The entry expires when either: - it reaches the 30-minute absolute
limit, or - it has not been accessed for 5 minutes.

Absolute expiration acts as the maximum lifetime.

`RefreshAsync()` refreshes the sliding expiration timeout (when
supported by the cache provider). It does **not** change the cached data
itself.

## 8. IDistributedCache vs IConnectionMultiplexer

### IDistributedCache

Use this when you need common cache operations through a .NET
abstraction.

``` csharp
private readonly IDistributedCache _cache;
```

Typical operations: - Get cached data. - Set cached data. - Remove
cached data. - Refresh expiration.

### IConnectionMultiplexer

`IConnectionMultiplexer` is provided by StackExchange.Redis. It manages
connections to Redis and gives access to Redis-specific functionality.

Registration:

``` csharp
builder.Services.AddSingleton<IConnectionMultiplexer>(sp =>
{
    var configuration =
        sp.GetRequiredService<IConfiguration>();

    var connectionString =
        configuration["RedisCacheOptions:Configuration"];

    return ConnectionMultiplexer.Connect(connectionString!);
});
```

Inject it:

``` csharp
private readonly IConnectionMultiplexer _redis;

public CacheAdminService(IConnectionMultiplexer redis)
{
    _redis = redis;
}
```

Example: delete a specific Redis key.

``` csharp
var db = _redis.GetDatabase();

await db.KeyDeleteAsync("MyApp:products");
```

Use `IConnectionMultiplexer` when you need Redis-specific operations
such as: - Working with Redis data structures. - Deleting keys or
scanning keys. - Pub/Sub. - Server information and diagnostics. -
Distributed locks.

**Remember:** `IDistributedCache` is a .NET abstraction.
`IConnectionMultiplexer` is a Redis-specific client API. They serve
different needs.

## 9. Quick revision

  -----------------------------------------------------------------------
  Question                            Short answer
  ----------------------------------- -----------------------------------
  What is Redis?                      An in-memory data store

  Where does Redis store data?        Mainly in the Redis server's RAM

  Is Redis only used for caching?     No, caching is one use case

  What is a cache hit?                Requested data is found in the
                                      cache

  What is a cache miss?               Requested data is not found in the
                                      cache

  What is distributed caching?        Multiple application instances
                                      share a cache

  Does distributed caching require    No
  Redis Cluster?                      

  What is `IMemoryCache`?             In-process application memory cache

  What is `IDistributedCache`?        .NET abstraction for distributed
                                      caching

  What is `IConnectionMultiplexer`?   StackExchange.Redis connection
                                      manager/client API

  What is TTL/expiration?             How long a cache entry remains
                                      available
  -----------------------------------------------------------------------

## 10. Important points to remember

1.  Redis is an in-memory data store; caching is one of its use cases.
2.  Redis runs separately from the .NET application and uses its own
    server/process memory.
3.  Multiple .NET application instances can share one Redis server.
4.  A single Redis server can be used for distributed caching; Redis
    Cluster is not mandatory.
5.  `IMemoryCache` is local to an application process.
6.  `IDistributedCache` gives .NET applications a common API for
    distributed caching.
7.  `IConnectionMultiplexer` provides access to Redis-specific features.
8.  Cache data should not automatically be treated as the permanent
    source of truth; the primary database usually remains the source of
    truth.
