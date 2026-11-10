A caching strategy defines how an application reads data from a cache, loads missing data, and handles updates.
<img width="1307" height="727" alt="image" src="https://github.com/user-attachments/assets/ccd13b79-5c52-4b2f-8869-4333d0cae311" />

Redis Caching Strategies
1. Cache-Aside (Lazy Loading)
How it works: The application checks the cache first. If the data is
missing, it reads from the database, stores the result in the cache, and
returns it.

Advantages - Simple and easy to implement. - Good for read-heavy
workloads. - The application can still read from the database if the
cache is unavailable, provided fallback handling is implemented.

Disadvantages - The application must manage cache misses and
invalidation. - Cached data can become stale until it is updated or
expires. - Many simultaneous misses can overload the database (cache
stampede).

When to use - General-purpose caching. - Product catalogues, product
details, and user profiles. - Read-heavy applications where occasional
cache misses are acceptable.
2. Read-Through
How it works: The application always requests data through the cache
layer. If the data is missing, the cache layer fetches it from the
database, stores it, and returns it.

Advantages - Cache-miss loading logic is centralized. - Application
code can be simpler. - Useful when many parts of an application need the
same loading behaviour.

Disadvantages - Requires a cache provider or custom layer that can
fetch data from the database. - Adds complexity to the caching layer. -
If the cache layer is unavailable and there is no fallback, reads can
fail.

When to use - When you want the cache layer to manage database
fetching. - When multiple services or application areas share the same
caching behaviour.

Note: Redis and .NET's IDistributedCache do not automatically
fetch data from SQL Server. Read-Through behaviour must be implemented
in your application or cache provider.
3. Write-Through
How it works: On a write, the cache layer coordinates updating the
database and the cache before the operation is considered complete.

Advantages - Cache is updated as part of the write flow. - Reduces
the chance of serving old cached data after a successful write. - Keeps
write behaviour centralized.

Disadvantages - Writes can be slower because both stores are
updated. - Failure handling is more complicated if one update succeeds
and the other fails. - A normal Redis operation and SQL transaction are
not automatically atomic.

When to use - When data freshness after updates is important. - For
data such as product prices or inventory, when the application can
handle the extra write complexity.
4. Write-Back (Write-Behind)
How it works: The application updates the cache first and returns
quickly. The database is updated later by a background process.

Advantages - Fast write response. - Can reduce database write
load. - Background processing can batch updates.

Disadvantages - The database may temporarily contain old data. -
Pending updates can be lost if they are not stored durably and the
system fails. - Requires reliable background processing, retries, and
recovery.

When to use - High-write workloads where eventual consistency is
acceptable. - Analytics, activity tracking, or workloads where updates
can safely be processed later.

Avoid using it casually for financial transactions, account
balances, or other critical data without durable storage and recovery
guarantees.
5. Refresh-Ahead
How it works: Frequently accessed cache entries are refreshed in the
background before they expire, so users are less likely to encounter a
cache miss.

Advantages - Reduces cache misses for popular data. - Helps maintain
fast response times around expiry. - Can reduce sudden database traffic
when popular entries expire.

Disadvantages - Background refresh adds complexity. - Data may be
refreshed even when nobody needs it. - Refresh jobs create additional
database load.

When to use - Frequently accessed data that changes regularly. -
Popular dashboards, news feeds, and trending product lists.
6. Write-Around (Related Write Pattern)
How it works: Writes go directly to the database and bypass the
cache. The cache is populated later when the data is read.

Advantages - Avoids filling the cache with data that may never be
read. - Keeps one-time or write-heavy data out of the cache.

Disadvantages - The first read after a write may be a cache miss. -
If an old cache entry exists, it must be invalidated or it may serve
stale data.

When to use - Data that is written often but read infrequently. -
Workloads where caching every write would waste cache space.
Quick Comparison
  Pattern           Read behaviour    Write behaviour   Best suited for
  Cache-Aside       Application       Application       General, read-heavy
checks cache;     updates DB and    workloads
loads from DB on  manages cache
miss                                

  Read-Through      Cache layer loads Separate write    Centralized cache
from DB on miss   strategy          loading

  Write-Through     Usually reads     Cache and DB      Freshness after writes
from cache        updated in the
write flow        

  Write-Back        Reads from cache  Cache first; DB   High-write,
later             eventual-consistency
workloads

  Refresh-Ahead     Popular entries   Background        Frequently accessed
refreshed before  refresh           data
expiry                              
Write-Around      Cache is checked  Writes bypass     Write-heavy,
on reads          cache             rarely-read data
Simple Rule for Choosing
- Need a simple, general solution? Use Cache-Aside.
- Want the cache layer to load missing data? Use Read-Through.
- Need cache updated during writes? Consider Write-Through.
- Need very fast writes and can accept delayed database updates?
  Consider Write-Back.
- Need popular data refreshed before expiry? Consider
  Refresh-Ahead.
- Most written data is rarely read? Consider Write-Around.
Important
These patterns are not all mutually exclusive. For example, an
application can use Cache-Aside for reads and invalidate the relevant
cache entry after a successful database update.

TTL controls how long a cache entry lives. Cache invalidation removes or
updates an entry when its data changes. Neither one automatically
guarantees consistency between Redis and the database.
