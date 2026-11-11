# Polly Timeout Strategy in .NET 8
[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)
## 1. What is Timeout Policy?

![Timeout Strategy](https://github.com/user-attachments/assets/37ac1768-3d5b-4db6-8d81-20e3402d94ef)

**Timeout is a resilience strategy that defines the maximum amount of time an operation is allowed to execute. If the operation exceeds this configured time limit, Polly cancels the operation and throws a `TimeoutRejectedException`.**

Simple example:

Maan le Order Service, Product Service ko product details fetch karne ke liye call karta hai.

We configure a timeout of 3 seconds.

- Response received within 3 seconds → Request succeeds.
- Response takes more than 3 seconds → Timeout occurs.
- Polly sends a cancellation signal to stop the ongoing operation.

**Purpose:** Prevent an application from waiting indefinitely for a slow or unresponsive dependency.

## 2. When should we use Timeout?

Timeout should be used when an operation must complete within a reasonable time limit.

Common scenarios:

| Scenario | Why use Timeout? |
|---|---|
| Calling Product Service | Avoid waiting too long for product details |
| Calling Payment Service | Prevent payment requests from hanging |
| Calling external APIs | Limit waiting time for third-party responses |
| Database operations | Restrict how long a query can execute, provided the database operation supports cancellation |
| Microservice communication | Prevent slow dependencies from consuming resources indefinitely |

### Real-world example

Suppose:

- Order Service calls Product Service.
- Product Service takes 30 seconds to respond.
- Order Service has a timeout configured for 3 seconds.

Instead of waiting for 30 seconds, Order Service requests cancellation after 3 seconds.

This helps maintain application responsiveness and avoids unnecessary waiting.

## 3. What is TimeoutRejectedException?

`TimeoutRejectedException` is an exception provided by Polly.

**It is thrown when an operation exceeds the timeout configured in Polly's Timeout Strategy.**

Example:

```csharp
try
{
    var response = await httpClient.GetAsync(
        "/api/products/101"
    );
}
catch (TimeoutRejectedException)
{
    Console.WriteLine("Product Service request timed out!");
}
```

Import:

```csharp
using Polly.Timeout;
```

### When do we use TimeoutRejectedException?

We use it when we want to handle a timeout separately from other failures.

For example:

- Log timeout failures.
- Return an appropriate error response.
- Trigger a fallback response.
- Provide a meaningful message to the client.
- Handle timeout failures differently from other HTTP errors.

**Remember:** `TimeoutRejectedException` is not thrown for every HTTP error. It specifically indicates that Polly's timeout was exceeded.

## 4. How does CancellationToken work?

This is an important concept to understand before implementing Timeout Strategy.

### What is CancellationToken?

**CancellationToken is a mechanism in .NET that allows one part of an application to notify another part that an ongoing operation should stop.**

Simple example:

Imagine Order Service requests product details from Product Service.

Product Service starts processing the request, but Order Service no longer needs the response.

Order Service can send a cancellation signal so that Product Service can stop unnecessary processing.

### Important classes and methods

| Component | Purpose |
|---|---|
| `CancellationTokenSource` | Creates and manages a cancellation signal |
| `CancellationToken` | Carries the cancellation signal to another operation |
| `Cancel()` | Sends the cancellation request |
| `ThrowIfCancellationRequested()` | Checks whether cancellation was requested |

### Basic CancellationToken example (without Polly)

Suppose we have a function that prints 10 numbers, waiting 5 seconds between each number.

We want to stop the operation after 12 seconds.

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var cts = new CancellationTokenSource();

        // Start the operation and pass the token
        Task task = PrintNumbersAsync(cts.Token);

        // Wait for 12 seconds
        await Task.Delay(12000);

        // Send cancellation signal
        cts.Cancel();

        await task;
    }

    static async Task PrintNumbersAsync(CancellationToken token)
    {
        try
        {
            for (int i = 1; i <= 10; i++)
            {
                token.ThrowIfCancellationRequested();

                Console.WriteLine(i);

                await Task.Delay(5000, token);
            }
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Processing stopped!");
        }
    }
}
```

### How does it work?

1. `CancellationTokenSource` creates the cancellation signal source.
2. We pass `cts.Token` to `PrintNumbersAsync()`.
3. The function starts printing numbers.
4. After 12 seconds, `cts.Cancel()` sends the cancellation signal.
5. The running operation detects cancellation and stops.

**Important:** CancellationToken does not forcefully kill an operation. The operation must check or respect the cancellation token.

## 5. How does CancellationToken work with Polly Timeout?

Now let's connect CancellationToken with our Timeout Strategy.

Suppose we configure a timeout of 10 seconds.

```csharp
pipeline.AddTimeout(TimeSpan.FromSeconds(10));
```

What happens internally?

1. Polly starts monitoring the operation.
2. The HTTP request starts executing.
3. If the request completes within 10 seconds, everything succeeds.
4. If 10 seconds expire, Polly internally triggers cancellation.
5. The cancellation signal is propagated to the HTTP operation.
6. Polly reports its timeout by throwing `TimeoutRejectedException`.

### Execution flow

```text
             Order Service
                   |
                   |
                   v
          Polly Timeout Strategy
                   |
              10 Seconds
                   |
                   v
           Timeout Expires
                   |
                   v
          Polly internally calls
               Cancel()
                   |
                   v
          CancellationToken
                   |
                   v
              HttpClient
                   |
                   v
        HTTP Request Aborted
```

### Who creates CancellationTokenSource and calls Cancel() in Polly?

In a normal application, we create `CancellationTokenSource` and call `Cancel()` ourselves.

But with Polly Timeout:

- Polly internally manages its timeout cancellation mechanism.
- Polly creates and manages the cancellation source for its timeout.
- When the configured timeout expires, Polly triggers cancellation internally.
- The cancellation signal is propagated to the operation.

**We do not need to manually create a CancellationTokenSource or call `Cancel()` for Polly's own timeout.**

### Important: Does HTTP cancellation stop the Product Service database query?

Not necessarily.

Cancelling an HTTP request does not guarantee that server-side processing or an already-running database query will stop.

If we also want to cancel the database operation, the cancellation token should be propagated through the Product Service layers.

```text
Order Service
     |
     | Polly Timeout
     |
     v
HttpClient
     |
     | Cancellation signal
     |
     v
Product Service Controller
     |
     v
Product Service Layer
     |
     v
Repository Layer
     |
     v
Database Query
```

Each layer must pass the token to the next layer, and the underlying operation must support cancellation.

### Example: Product Service cancellation propagation

**Controller:**

```csharp
[HttpGet("{id}")]
public async Task<IActionResult> GetProduct(
    int id,
    CancellationToken cancellationToken)
{
    var product = await _service.GetProductAsync(
        id,
        cancellationToken
    );

    return Ok(product);
}
```

**Service Layer:**

```csharp
public async Task<Product?> GetProductAsync(
    int id,
    CancellationToken cancellationToken)
{
    return await _repository.GetProductAsync(
        id,
        cancellationToken
    );
}
```

**Repository Layer:**

```csharp
public async Task<Product?> GetProductAsync(
    int id,
    CancellationToken cancellationToken)
{
    return await _dbContext.Products
        .FirstOrDefaultAsync(
            p => p.Id == id,
            cancellationToken
        );
}
```

Here, EF Core receives the cancellation token and can cancel the database query if the database provider supports cancellation.

**Remember:** Cancellation is cooperative, not forceful. There is no guarantee that every operation will stop immediately.

## 6. How to implement Timeout using Polly v8?

Polly v8 provides `AddTimeout()` to add a Timeout Strategy to a resilience pipeline.

### Step 1: Add Timeout to the pipeline

```csharp
using Polly;
using Polly.Timeout;

pipeline.AddTimeout(TimeSpan.FromSeconds(3));
```

This configures a maximum execution time of 3 seconds.

You can also use `TimeoutStrategyOptions`:

```csharp
pipeline.AddTimeout(new TimeoutStrategyOptions
{
    Timeout = TimeSpan.FromSeconds(3),

    OnTimeout = args =>
    {
        Console.WriteLine(
            $"Operation timed out after {args.Timeout.TotalSeconds} seconds."
        );

        return ValueTask.CompletedTask;
    }
});
```

Here:

- `Timeout` defines the maximum allowed execution time.
- `OnTimeout` is an optional callback executed when a timeout occurs.
- `OnTimeout` is useful for logging and monitoring.

### Step 2: Integrate Timeout into our Product Service HttpClient

In our existing Order Service `Program.cs`, we already have Retry and Circuit Breaker configured inside a single `AddResilienceHandler()`.

We can add Timeout to that same pipeline:

```csharp
builder.Services.AddHttpClient<ProductServiceClient>(client =>
{
    client.BaseAddress = new Uri(
        builder.Configuration["ProductService:BaseUrl"]
        ?? "https://localhost:7062");
})
.AddResilienceHandler("ProductServicePolicy", (pipeline, context) =>
{
    // Overall timeout for the complete operation
    pipeline.AddTimeout(TimeSpan.FromSeconds(10));

    var policies = context.ServiceProvider
        .GetRequiredService<IProductServicePolicies>();

    policies.ConfigureRetry(pipeline);
    policies.ConfigureCircuitBreaker(pipeline);
});
```

**Why did we add Timeout first?**

Polly executes the first-added strategy as the outer strategy.

So the execution order is:

```text
Timeout
   |
   v
 Retry
   |
   v
Circuit Breaker
   |
   v
HTTP Request
```

This means the 10-second timeout applies to the complete pipeline execution, including retries, rather than giving every retry a fresh 10 seconds.

### Step 3: Pass CancellationToken to HttpClient

Our ProductServiceClient can accept a CancellationToken from its caller.

```csharp
public async Task<Product?> GetProductAsync(
    int productId,
    CancellationToken cancellationToken)
{
    try
    {
        var response = await _httpClient.GetAsync(
            $"/api/products/{productId}",
            cancellationToken
        );

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<Product>(
            cancellationToken
        );
    }
    catch (TimeoutRejectedException)
    {
        Console.WriteLine("Product Service request timed out!");

        return null;
    }
}
```

**Important distinction:**

- The caller's CancellationToken allows cancellation to be propagated from the calling operation.
- Polly internally manages its own timeout cancellation.
- The HttpClient resilience pipeline receives the cancellation signal and propagates it to the HTTP operation.

We do not need to manually create a CancellationTokenSource or call `Cancel()` for Polly's timeout.

### Step 4: Handle TimeoutRejectedException in ProductServiceClient

When Polly's timeout expires, we can catch the exception and handle it separately.

```csharp
using Polly.Timeout;

public async Task<Product?> GetProductAsync(
    int productId,
    CancellationToken cancellationToken)
{
    try
    {
        var response = await _httpClient.GetAsync(
            $"/api/products/{productId}",
            cancellationToken
        );

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<Product>(
            cancellationToken
        );
    }
    catch (TimeoutRejectedException)
    {
        Console.WriteLine("Product Service request timed out!");

        return null;
    }
}
```

Here:

- If Product Service responds in time → Return product details.
- If Polly's timeout expires → Catch `TimeoutRejectedException` and return `null`.
- If the original caller cancels the operation → Caller cancellation is propagated separately; do not mistake it for Polly's own timeout.

**Important:** Returning `null` is just an example. In a real application, you might return a Result object or throw a custom exception so the API layer can return an appropriate HTTP response.

## 7. Important points to remember

1. Polly v8 uses `AddTimeout()` to configure a Timeout Strategy.
2. `TimeoutRejectedException` is thrown when Polly's configured timeout expires.
3. Polly Timeout internally uses cancellation tokens to request cancellation.
4. We do not need to manually call `Cancel()` for Polly's timeout.
5. The underlying operation must respect the cancellation token for cancellation to work.
6. HTTP request cancellation does not guarantee that server-side database processing will stop.
7. To propagate cancellation through Product Service, pass the token from Controller → Service → Repository.
8. If the original caller cancels the request, Polly honours that cancellation; it is different from Polly's own timeout.
9. `OnTimeout` is useful for logging timeout events.
10. Timeout can be combined with Retry, Circuit Breaker and Fallback.

## 8. Quick Revision

| Concept | Meaning |
|---|---|
| Timeout Strategy | Limits the maximum execution time |
| `AddTimeout()` | Adds Timeout to the pipeline |
| `Timeout` | Defines the maximum allowed duration |
| `TimeoutRejectedException` | Exception thrown when Polly's timeout expires |
| `OnTimeout` | Callback executed when a timeout occurs |
| `CancellationTokenSource` | Creates and manages a cancellation signal |
| `CancellationToken` | Carries the cancellation signal |
| `Cancel()` | Requests cancellation |
| `ThrowIfCancellationRequested()` | Checks whether cancellation was requested |

### Remember, bhidu:

- Retry → Try again.
- Circuit Breaker → Stop calling temporarily.
- Fallback → Provide an alternative.
- Timeout → Don't wait too long.
- CancellationToken → Send a signal to stop an ongoing operation.

For more details: [Official Polly Timeout Documentation](https://www.pollydocs.org/strategies/timeout.html).
