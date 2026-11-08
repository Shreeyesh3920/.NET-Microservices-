# Polly Timeout Strategy in .NET 8

## 1. What is Timeout Policy?

Timeout Policy is a resilience strategy that defines the maximum amount of time an operation is allowed to execute. If the operation exceeds this configured time limit, Polly cancels the operation and throws a `TimeoutRejectedException`.

Simple example:

Maan le Order Service, Product Service ko product details fetch karne ke liye call karta hai.

We configure a timeout of 3 seconds.

- Response received within 3 seconds → Request succeeds.
- Response takes more than 3 seconds → Timeout occurs.
- Polly cancels the operation and reports a timeout.

Purpose: Prevent an application from waiting indefinitely for a slow or unresponsive dependency.

## 2. When should we use Timeout?

Timeout should be used when an operation must complete within a reasonable time limit.

Common scenarios:

| Scenario                   | Why use Timeout?                                                                             |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| Calling Product Service    | Avoid waiting too long for product details                                                   |
| Calling Payment Service    | Prevent payment requests from hanging                                                        |
| Calling external APIs      | Limit waiting time for third-party responses                                                 |
| Database operations        | Restrict how long a query can execute, provided the database operation supports cancellation |
| Microservice communication | Prevent slow dependencies from consuming resources indefinitely                              |

### Real-world example

Suppose:

- Order Service calls Product Service.
- Product Service takes 30 seconds to respond.
- Order Service has a timeout configured for 3 seconds.

Instead of waiting for 30 seconds, Order Service cancels its request after the timeout period.

This helps maintain application responsiveness.

## 3. What is TimeoutRejectedException?

`TimeoutRejectedException` is an exception provided by Polly.

It is thrown when an operation exceeds the timeout configured in Polly's Timeout Strategy.

Example:

```
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

```
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

Remember: `TimeoutRejectedException` is not thrown for every HTTP error. It specifically indicates that Polly's timeout was exceeded.

## 4. How to implement Timeout using Polly v8?

Polly v8 provides `AddTimeout()` to add a Timeout Strategy to a resilience pipeline.

### Step 1: Add Timeout to the pipeline

```
using Polly;
using Polly.Timeout;

pipeline.AddTimeout(TimeSpan.FromSeconds(3));
```

This configures a maximum execution time of 3 seconds.

You can also use `TimeoutStrategyOptions`:

```
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

Here, `OnTimeout` is an optional callback that executes when a timeout occurs. It is useful for logging and monitoring.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1



### Step 2: Integrate Timeout into our Product Service HttpClient

In our existing Order Service `Program.cs`, we already have Retry and Circuit Breaker configured inside a single `AddResilienceHandler()`.

We can add Timeout to that same pipeline:

```
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

Why did we add Timeout first?

Polly executes the first-added strategy as the outer strategy.

So the execution order is:

`Timeout → Retry → Circuit Breaker → HTTP Request`

This means the 10-second timeout applies to the complete operation, including retries, rather than giving every retry a fresh 10 seconds.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1



### Step 3: Handle TimeoutRejectedException in ProductServiceClient

When Polly's timeout expires, we can catch the exception and handle it separately.

```
using Polly.Timeout;

public async Task<Product?> GetProductAsync(int productId)
{
    try
    {
        var response = await _httpClient.GetAsync(
            $"/api/products/{productId}"
        );

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<Product>();
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
- The calling service can then handle the unavailable product response.

Important: Returning `null` is just an example. In a real application, you might return a Result object or throw a custom exception so the API layer can return an appropriate HTTP response.

## 5. Important points to remember

1. Polly v8 uses `AddTimeout()` to configure a timeout strategy.
2. `TimeoutRejectedException` is thrown when Polly's configured timeout expires.
3. Polly Timeout uses cancellation tokens. The underlying operation must respect the cancellation token for cancellation to work properly.
4. If the original caller cancels the request, Polly honours that cancellation; it does not treat it as its own timeout.
5. `OnTimeout` is useful for logging timeout events.
6. Timeout can be combined with Retry, Circuit Breaker and Fallback.

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1

## 6. Quick revision

| Concept                    | Meaning                                       |
| -------------------------- | --------------------------------------------- |
| Timeout Strategy           | Limits the maximum execution time             |
| `AddTimeout()`             | Adds Timeout to the pipeline                  |
| `Timeout`                  | Defines the maximum allowed duration          |
| `TimeoutRejectedException` | Exception thrown when Polly's timeout expires |
| `OnTimeout`                | Callback executed when a timeout occurs       |

Remember, bhidu:

- Retry → Try again.
- Circuit Breaker → Stop calling temporarily.
- Fallback → Provide an alternative.
- Timeout → Don't wait too long.

For more details: [Official Polly Timeout Documentation](https://www.pollydocs.org/strategies/timeout.html?utm_source=chatgpt.com).
