A caching strategy defines how an application reads data from a cache, loads missing data, and handles updates.
<img width="1307" height="727" alt="image" src="https://github.com/user-attachments/assets/ccd13b79-5c52-4b2f-8869-4333d0cae311" />

# Redis Caching Strategies

## 1. Cache-Aside (Lazy Loading)

**How it works:** The application checks the cache first. If the data is not found, it fetches the data from the database, stores it in the cache, and returns it.
<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/29f4f8bb-64bd-4681-a3cc-adb68e0a9ee6" />

### Advantages
- Simple and easy to implement.
- Works well for read-heavy workloads.
- Application can continue working if the cache goes down, provided database fallback is implemented.

### Disadvantages
- Application must manage cache misses and invalidation.
- Cached data can become stale until it is updated or expires.
- Multiple simultaneous cache misses can overload the database (Cache Stampede).

### When to Use
- General-purpose caching.
- Product catalogues, product details, and user profiles.
- Applications with more read operations than write operations.

---

## 2. Read-Through

**How it works:** The application always requests data through the cache layer. If the data is not found, the cache layer fetches it from the database, stores it in the cache, and returns it.

### Advantages
- Cache-miss handling is centralized.
- Simplifies application code.
- Provides consistent data-loading behaviour.

### Disadvantages
- Requires a custom cache layer or provider that supports database fetching.
- Adds complexity to the caching layer.
- Cache failure can cause read failures if no fallback is implemented.

### When to Use
- When you want the cache layer to handle database fetching.
- When multiple application components share the same caching logic.

**Note:** Redis and .NET's `IDistributedCache` do not automatically fetch data from SQL Server. This behaviour must be implemented separately.

---

## 3. Write-Through

**How it works:** When data is updated, both the database and cache are updated as part of the same write operation. The operation completes after both updates succeed.

### Advantages
- Cache is updated during the write operation.
- Reduces the chances of serving stale data after an update.
- Keeps cache updates consistent with the write flow.

### Disadvantages
- Write operations can be slower because both stores are updated.
- Failure handling becomes more complicated.
- Database and cache updates are not automatically atomic.

### When to Use
- When data freshness is important.
- Product prices, inventory, and user information.
- Applications that can accept additional write latency.

---

## 4. Write-Back (Write-Behind)

**How it works:** The application updates the cache first and returns a response. The database is updated later by a background process.

### Advantages
- Very fast write operations.
- Reduces immediate database write load.
- Background processing can batch multiple updates.

### Disadvantages
- Database may temporarily contain outdated data.
- Pending updates can be lost if the system crashes before they are persisted.
- Requires reliable background processing, retries, and recovery mechanisms.

### When to Use
- Write-heavy applications.
- Analytics and activity tracking.
- Applications where eventual consistency is acceptable.

**Note:** Avoid using this approach for critical financial transactions without proper durability and recovery mechanisms.

---

## 5. Refresh-Ahead

**How it works:** Frequently accessed cache entries are refreshed in the background before they expire. This reduces the chances of users encountering a cache miss.

### Advantages
- Reduces cache misses for frequently accessed data.
- Improves response times.
- Helps avoid sudden database load when popular cache entries expire.

### Disadvantages
- Requires background processing.
- Data may be refreshed even when nobody needs it.
- Creates additional database load.

### When to Use
- Frequently accessed data that changes regularly.
- Popular dashboards and news feeds.
- Trending products and frequently accessed catalogues.

---

## 6. Write-Around

**How it works:** Write operations go directly to the database and bypass the cache. Data is added to the cache only when it is requested later.

### Advantages
- Avoids caching data that may never be accessed.
- Reduces unnecessary cache writes.
- Useful for write-heavy workloads.

### Disadvantages
- First read after a write results in a cache miss.
- Existing cached entries must be invalidated to avoid stale data.

### When to Use
- Data that is written frequently but read rarely.
- Applications where caching every write would waste cache space.

---

## Quick Comparison

| Strategy | Read Behaviour | Write Behaviour | Best Suited For |
|---|---|---|---|
| Cache-Aside | Application checks cache and fetches from DB on a miss | Application updates DB and manages cache | General-purpose, read-heavy workloads |
| Read-Through | Cache layer fetches from DB on a miss | Separate write strategy | Centralized cache loading |
| Write-Through | Reads from cache | Updates cache and DB together | Data freshness after updates |
| Write-Back | Reads from cache | Updates cache first, DB later | High-write workloads |
| Refresh-Ahead | Popular entries are refreshed before expiry | Background refresh | Frequently accessed data |
| Write-Around | Cache is checked during reads | Writes bypass cache | Write-heavy, rarely-read data |

---

## Which Strategy Should You Choose?

| Requirement | Recommended Strategy |
|---|---|
| Need a simple caching solution | Cache-Aside |
| Want the cache layer to handle database fetching | Read-Through |
| Need cache updates during write operations | Write-Through |
| Need faster writes and can accept eventual consistency | Write-Back |
| Want popular data refreshed before expiry | Refresh-Ahead |
| Data is written frequently but rarely read | Write-Around |

---

## Important Notes

- These caching strategies are not mutually exclusive. Different strategies can be combined based on application requirements.
- Cache-Aside is a good starting point for most applications.
- TTL (Time To Live) defines how long a cache entry remains valid.
- Cache invalidation removes or updates cached data when the original data changes.
- TTL and cache invalidation are different mechanisms.
- Redis and the database do not automatically stay synchronized. The application must handle consistency.
