# Bulkhead Isolation in .NET 8 using Polly

## 1. What is Bulkhead Isolation?

**Bulkhead Isolation is a resilience technique that limits the number of concurrent operations to prevent one dependency from consuming excessive resources and affecting other operations.**

In simple words, it controls how many requests can execute simultaneously.

It helps isolate workloads so that a slow or overloaded service does not affect the entire application.

### Real-World Example

Imagine a ship with multiple separate compartments.

If one compartment gets flooded, the other compartments remain protected because of the bulkheads separating them.

The same concept applies to microservices.

For example, our Order Service communicates with:

- Product Service
- User Service
- Payment Service

If Product Service becomes slow, we don't want its requests to consume all the available resources and affect other operations.

Bulkhead Isolation helps control this by setting separate concurrency limits.

## 2. How does Bulkhead Isolation work?

### Without Bulkhead Isolation

```text
                 Order Service
                       |
                Incoming Requests
                       |
                Product Service
                       |
              Requests keep waiting
                       |
             Resources get exhausted
                       |
          Other operations may be affected
```

### With Bulkhead Isolation

We configure separate concurrency limits for each dependency.

```text
                  Order Service
                       |
          +------------+------------+
          |            |            |
          v            v            v
     Product        User         Payment
      Service      Service       Service
          |            |            |
      Limit: 3      Limit: 5      Limit: 2
```

Each dependency has its own concurrency limit.

If Product Service reaches its limit, additional Product Service requests can be rejected or queued without consuming permits reserved for the other dependencies.

## 3. When should we use Bulkhead Isolation?

Use Bulkhead Isolation when:

1. Multiple microservices are called by a single application.
2. One dependency may become slow or overloaded.
3. We want to control simultaneous requests.
4. We want to isolate workloads and prevent resource exhaustion.
5. We want to protect critical operations from excessive concurrent workloads.

### Common Use Cases

| Scenario | Purpose |
|---|---|
| Product Service | Limit concurrent product requests |
| Payment Service | Control simultaneous payment operations |
| External APIs | Prevent excessive concurrent API calls |
| Background Jobs | Limit simultaneously executing jobs |
| Microservices | Isolate workload capacity between dependencies |

## 4. How to implement Bulkhead Isolation using Polly v8?

Polly v8 provides `AddConcurrencyLimiter()` to limit concurrent executions.

### Step 1: Configure Concurrency Limiter

```csharp
using Polly;
using Polly.RateLimiting;

pipeline.AddConcurrencyLimiter(
    permitLimit: 3,
    queueLimit: 0
);
```

### Step 2: Understand the Parameters

| Parameter | Description |
|---|---|
| `permitLimit` | Maximum number of operations allowed to execute simultaneously |
| `queueLimit` | Maximum number of additional operations allowed to wait for a permit |

### Example

```csharp
pipeline.AddConcurrencyLimiter(
    permitLimit: 3,
    queueLimit: 0
);
```

Here:

- `permitLimit: 3` means a maximum of 3 requests can execute simultaneously.
- `queueLimit: 0` means additional requests cannot wait in a queue.

## 5. Understanding permitLimit and queueLimit

### Case 1: queueLimit = 0

Configuration:

```csharp
pipeline.AddConcurrencyLimiter(
    permitLimit: 3,
    queueLimit: 0
);
```

Suppose 4 requests arrive simultaneously.

```text
       Concurrency Limit = 3

       +--------+--------+--------+
       |   R1   |   R2   |   R3   |
       +--------+--------+--------+
           Running (3 requests)

       +--------+
       |   R4   |
       +--------+
          Rejected
```

- R1, R2 and R3 execute.
- R4 is rejected immediately because no permits are available and the queue is full.

### Case 2: queueLimit = 1

Configuration:

```csharp
pipeline.AddConcurrencyLimiter(
    permitLimit: 3,
    queueLimit: 1
);
```

Suppose 4 requests arrive simultaneously.

```text
       Concurrency Limit = 3

       +--------+--------+--------+
       |   R1   |   R2   |   R3   |
       +--------+--------+--------+
           Running (3 requests)

       +--------+
       |   R4   |
       +--------+
          Waiting
```

- R1, R2 and R3 execute.
- R4 waits in the queue.
- If R5 arrives while R4 is waiting, R5 gets rejected because the queue is full.

When one of the executing requests completes, its permit is released and R4 can start executing.

### Quick Comparison

| Configuration | Behaviour |
|---|---|
| `queueLimit: 0` | No waiting; additional requests are rejected |
| `queueLimit: 1` | One additional request can wait |
| `queueLimit: 5` | Five additional requests can wait |
| `queueLimit: 10` | Ten additional requests can wait |

**Remember:**

- `permitLimit` = Maximum concurrent executions.
- `queueLimit` = Maximum additional waiting executions.

## 6. Handling RateLimiterRejectedException

When a request cannot acquire a permit and cannot enter the queue, Polly throws `RateLimiterRejectedException`.

We can handle it using a try-catch block.

```csharp
using Polly.RateLimiting;

try
{
    var response = await _httpClient.GetAsync(
        "/api/products"
    );
}
catch (RateLimiterRejectedException)
{
    Console.WriteLine(
        "Too many concurrent requests. Try again later!"
    );
}
```

This allows the application to handle rejected requests separately.

## 7. Bulkhead Isolation vs Rate Limiting

Both strategies control requests, but they solve different problems.

| Bulkhead / Concurrency Limiter | Rate Limiter |
|---|---|
| Controls simultaneous executions | Controls request frequency |
| Limits concurrent operations | Limits requests within a time period |
| Example: 3 requests executing at once | Example: 3 requests per second |

**Example:**

- Concurrency Limiter: Maximum 3 requests executing simultaneously.
- Rate Limiter: Maximum 3 requests allowed per second.

These are different limits.

## 8. Important Points to Remember

1. Polly v8 provides `AddConcurrencyLimiter()` for concurrency limiting.
2. `permitLimit` defines the maximum concurrent executions.
3. `queueLimit` defines the maximum waiting requests.
4. `RateLimiterRejectedException` is thrown when a request cannot acquire a permit or enter the queue.
5. Once an operation completes, its permit is released.
6. Queued requests can execute when permits become available.
7. Bulkhead Isolation does not directly limit requests per second.

## 9. Quick Revision

| Strategy | Responsibility |
|---|---|
| Retry | Try the failed operation again |
| Circuit Breaker | Temporarily stop calling an unhealthy service |
| Fallback | Provide an alternative response |
| Timeout | Limit how long an operation can execute |
| Bulkhead Isolation | Limit concurrent operations and isolate workload capacity |

### One-Line Summary

**Bulkhead Isolation prevents one dependency from consuming excessive concurrent capacity by setting limits on simultaneous operations.**
