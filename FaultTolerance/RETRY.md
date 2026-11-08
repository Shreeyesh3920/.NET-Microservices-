- Order Service calls User Service to validate a user.
- If User Service temporarily fails, Order Service retries the request.
- We'll create a `Policies` folder containing `IUserServicePolicies` and `UserServicePolicies`.
- We'll build the Polly pipeline once and reuse it through dependency injection.

# Fault Tolerance – Retry Pattern using Polly v8

## 1. What is Fault Tolerance?

Fault Tolerance is the ability of a system to continue operating even when one or more of its components fail.

In a microservices architecture, services communicate with each other over a network. These calls can fail for several reasons:

- Temporary network interruption.
- Target service is temporarily unavailable.
- Request timeout.
- Server temporarily returns an error.
- Database or another dependency is temporarily unavailable.

A failure in one service can affect other services that depend on it.

Example:

Suppose our Order Service depends on User Service to validate a user before creating an order.

```
Client
  |
  v
Order Service
  |
  |----> User Service
  |
  v
Create Order
```

If User Service temporarily fails, Order Service may also fail to process the order.

Fault Tolerance techniques help us handle these temporary failures more gracefully.

One such technique is the Retry Pattern.

## 2. What is the Retry Pattern?

The Retry Pattern is a fault tolerance technique in which an application automatically retries an operation when it encounters a temporary failure.

Instead of immediately returning an error, the application attempts the same operation again, usually after waiting for a configured period.

Example:

Suppose Order Service calls User Service to retrieve user details.

1. Order Service sends a request to User Service.
2. User Service temporarily fails and returns HTTP 503.
3. Order Service waits for a configured delay.
4. Order Service sends the same request again.
5. If User Service responds successfully, Order Service continues processing the order.

```
          Order Service
                |
                | GET User Details
                v
           User Service
                |
                v
          Temporary Failure
                |
                v
          Wait for 2 seconds
                |
                v
          Retry Request
                |
                v
           User Service
                |
                v
          Successful Response
                |
                v
        Continue Order Creation
```

The main purpose of Retry is to recover from temporary failures without requiring manual intervention.

### When should we use Retry?

Retry is useful when a failure is likely to be temporary.

Examples:

- Temporary network interruption.
- HTTP 503 – Service Unavailable.
- HTTP 502 – Bad Gateway.
- HTTP 504 – Gateway Timeout.
- HTTP 408 – Request Timeout.
- HTTP 429 – Too Many Requests (respecting the server's retry instructions where possible).

However, Retry is not useful for every failure.

For example:

- HTTP 400 – Bad Request.
- HTTP 401 – Unauthorized.
- HTTP 403 – Forbidden.
- HTTP 404 – Not Found.

These usually require correcting the request, permissions, or resource rather than repeating the same request.

## 3. What is Polly?

Polly is a resilience and transient-fault-handling library for .NET applications.

It provides implementations of resilience strategies that help applications handle failures when communicating with external services or dependencies.

For our implementation, we will use Polly v8.

Polly v8 introduces the concept of a Resilience Pipeline.

A resilience pipeline is an object that executes our operation through one or more configured resilience strategies.

For now, we will add only the Retry strategy to our pipeline.

### Install Polly v8

Install the Polly NuGet package in the Order Microservice project:

```
dotnet add package Polly
```

The `Polly` package provides the core resilience pipeline and Retry implementation.

## 4. Understanding Retry in Polly v8

Polly v8 provides the following classes and methods for implementing Retry:

| Class / Method              | Purpose                                                  |
| --------------------------- | -------------------------------------------------------- |
| `ResiliencePipelineBuilder` | Creates and configures a resilience pipeline.            |
| `RetryStrategyOptions`      | Configures Retry behavior.                               |
| `AddRetry()`                | Adds the Retry strategy to the pipeline.                 |
| `Build()`                   | Builds the configured pipeline.                          |
| `ExecuteAsync()`            | Executes an asynchronous operation through the pipeline. |

### Important Retry properties

The `RetryStrategyOptions` class provides several properties to configure how retries should work.

| Property           | Description                                                        |
| ------------------ | ------------------------------------------------------------------ |
| `MaxRetryAttempts` | Maximum number of retries after the original request.              |
| `Delay`            | Time to wait between retry attempts.                               |
| `BackoffType`      | Determines how the delay changes between retries.                  |
| `UseJitter`        | Adds randomness to retry delays.                                   |
| `ShouldHandle`     | Defines which exceptions or failed results should trigger a retry. |
| `OnRetry`          | Executes a callback when a retry occurs.                           |
| `MaxDelay`         | Sets an upper limit on the calculated retry delay.                 |

### 4.1 MaxRetryAttempts

Defines how many times Polly should retry an operation after its initial execution.

For example:

```
MaxRetryAttempts = 3
```

This means:

- First execution: Original request.
- Second execution: Retry 1.
- Third execution: Retry 2.
- Fourth execution: Retry 3.



Important

`MaxRetryAttempts = 3` means a maximum of 4 total executions, including the original request.



### 4.2 Delay

Defines how long Polly should wait before executing the next retry.

```
Delay = TimeSpan.FromSeconds(2)
```

This configures a two-second delay between retry attempts when using constant backoff.

### 4.3 BackoffType

Defines how the delay between retries should be calculated.

Polly v8 provides three backoff types:

Constant Backoff

The delay remains the same between every retry.

```
BackoffType = DelayBackoffType.Constant,
Delay = TimeSpan.FromSeconds(2)
```

Example:

| Execution        | Delay before execution |
| ---------------- | ---------------------- |
| Original request | 0 seconds              |
| Retry 1          | 2 seconds              |
| Retry 2          | 2 seconds              |
| Retry 3          | 2 seconds              |

Linear Backoff

The delay increases linearly with each retry.

For example, with a base delay of 2 seconds:

```
Retry 1 → 2 seconds
Retry 2 → 4 seconds
Retry 3 → 6 seconds
```

Exponential Backoff

The delay increases exponentially with each retry.

For example, with a base delay of 2 seconds:

```
Retry 1 → 2 seconds
Retry 2 → 4 seconds
Retry 3 → 8 seconds
```

Exponential backoff is useful when we want to give an unavailable service progressively more time to recover.

### 4.4 UseJitter

Jitter adds a random variation to the retry delay.

Consider a situation where 1,000 Order Service instances try calling User Service, and User Service becomes temporarily unavailable.

If every instance retries after exactly two seconds, all 1,000 requests may reach User Service at approximately the same time.

This can create additional load on the recovering service.

This situation is commonly associated with the Thundering Herd Problem.

Jitter helps distribute retry requests over time.

```
UseJitter = true
```

For example, instead of every request retrying after exactly two seconds, the actual delays can vary.

## 5. Implementing Retry in Our Order Microservice

Now let's implement Retry using the architecture you suggested.

Our requirement is:

Whenever Order Service calls User Service to retrieve user details, Polly should automatically retry the request if a transient failure occurs.

We will create the following folder structure:

```
OrderMicroserviceAPI
│
├── Policies
│   ├── IUserServicePolicies.cs
│   └── UserServicePolicies.cs
│
├── Clients
│   └── UserServiceClient.cs
│
├── Program.cs
│
└── ...
```

### Step 1: Create IUserServicePolicies.cs

Inside the `Policies` folder, create an interface named `IUserServicePolicies`.

```
using Polly;

namespace OrderMicroserviceAPI.Policies;

public interface IUserServicePolicies
{
    ResiliencePipeline GetUserServicePipeline();
}
```

Explanation:

- `IUserServicePolicies` defines the contract for our User Service resilience policies.
- `GetUserServicePipeline()` exposes the configured Polly pipeline.
- `ResiliencePipeline` is the Polly v8 object that executes operations using the configured resilience strategies.

Why are we creating an interface?

We want to separate the definition of our policy from its actual implementation.

Our `UserServiceClient` will depend on the interface rather than directly depending on the concrete `UserServicePolicies` class.

This follows the Dependency Inversion Principle.

### Step 2: Create UserServicePolicies.cs

Now create a concrete implementation named `UserServicePolicies.cs`.

```
using System.Net;
using System.Net.Http;
using Polly;
using Polly.Retry;

namespace OrderMicroserviceAPI.Policies;

public class UserServicePolicies : IUserServicePolicies
{
    private readonly ResiliencePipeline _userServicePipeline;

    public UserServicePolicies()
    {
        _userServicePipeline =
            new ResiliencePipelineBuilder()
                .AddRetry(new RetryStrategyOptions
                {
                    MaxRetryAttempts = 3,

                    Delay = TimeSpan.FromSeconds(2),

                    BackoffType = DelayBackoffType.Constant,

                    UseJitter = false,

                    ShouldHandle = new PredicateBuilder()
                        .Handle<HttpRequestException>(exception =>
                            exception.StatusCode == null ||
                            exception.StatusCode == HttpStatusCode.RequestTimeout ||
                            exception.StatusCode == HttpStatusCode.TooManyRequests ||
                            (int)exception.StatusCode >= 500)
                })
                .Build();
    }

    public ResiliencePipeline GetUserServicePipeline()
    {
        return _userServicePipeline;
    }
}
```

Understanding the implementation:

1\. Create a pipeline field

```
private readonly ResiliencePipeline _userServicePipeline;
```

This field stores the configured Polly resilience pipeline.

We use `readonly` because we want to initialize the pipeline once in the constructor and reuse that same instance.

2\. Create the pipeline builder

```
new ResiliencePipelineBuilder()
```

This creates a builder that allows us to configure the resilience pipeline.

3\. Add Retry

```
.AddRetry(new RetryStrategyOptions
{
    MaxRetryAttempts = 3,
    Delay = TimeSpan.FromSeconds(2),
    BackoffType = DelayBackoffType.Constant,
    UseJitter = false
})
```

Here we configure Retry:

- Maximum retries: 3.
- Delay: 2 seconds.
- Backoff: Constant.
- Jitter: Disabled.

4\. Configure ShouldHandle

```
ShouldHandle = new PredicateBuilder()
    .Handle<HttpRequestException>(exception =>
        exception.StatusCode == null ||
        exception.StatusCode == HttpStatusCode.RequestTimeout ||
        exception.StatusCode == HttpStatusCode.TooManyRequests ||
        (int)exception.StatusCode >= 500)
```

This defines which exceptions should trigger a retry.

In our example:

- Network-related `HttpRequestException` with no HTTP status code is retried.
- HTTP 408 is retried.
- HTTP 429 is retried.
- HTTP 5xx errors are retried.
- HTTP 400, 401, 403 and 404 are not retried by this predicate.

This is important because we should not waste time retrying failures that are unlikely to resolve by repeating the same request.

5\. Build the pipeline

```
.Build();
```

`Build()` creates the actual `ResiliencePipeline` using the configured strategies.

6\. Return the pipeline

```
public ResiliencePipeline GetUserServicePipeline()
{
    return _userServicePipeline;
}
```

This method returns the already-created pipeline.

We are not building a new pipeline every time this method is called.

### Step 3: Register the policy in Program.cs

Now we need to register our policy implementation with the Dependency Injection container.

Open `Program.cs` in the Order Microservice project.

Add:

```
builder.Services.AddSingleton<
    IUserServicePolicies,
    UserServicePolicies>();
```

Our existing HTTP client registrations can remain:

```
builder.Services.AddHttpClient<UserServiceClient>(client =>
{
    client.BaseAddress = new Uri(
        builder.Configuration["UserService:BaseUrl"]
        ?? "https://localhost:7061");
});
```

Why AddSingleton?

We register `UserServicePolicies` as a Singleton because:

1. Polly's resilience pipeline is designed to be reused and is thread-safe.
2. We build the pipeline only once.
3. All User Service client instances can use the same configured pipeline.
4. We avoid unnecessarily rebuilding the pipeline for every request.

How does Dependency Injection know which class to instantiate?

Look at this registration:

```
builder.Services.AddSingleton<
    IUserServicePolicies,
    UserServicePolicies>();
```

We explicitly tell the DI container:

- Service type: `IUserServicePolicies`.
- Implementation type: `UserServicePolicies`.

Therefore, whenever a class requests `IUserServicePolicies` through its constructor, the DI container provides an instance of `UserServicePolicies`.

### Step 4: Inject the policy into UserServiceClient

Now we need to use our pipeline when making HTTP requests to User Service.

Our existing `UserServiceClient` calls:

```
GET /api/auth/getUser/{userId}
```

Update the client as follows:

```
using System.Net.Http;
using System.Net.Http.Json;
using OrderMicroserviceAPI.Policies;

namespace OrderMicroserviceAPI.Clients;

public class UserServiceClient
{
    private readonly HttpClient _httpClient;
    private readonly IUserServicePolicies _userServicePolicies;

    public UserServiceClient(
        HttpClient httpClient,
        IUserServicePolicies userServicePolicies)
    {
        _httpClient = httpClient;
        _userServicePolicies = userServicePolicies;
    }

    public async Task<UserDTO?> GetUserByIdAsync(
        int userId,
        CancellationToken cancellationToken = default)
    {
        return await _userServicePolicies
            .GetUserServicePipeline()
            .ExecuteAsync(async token =>
            {
                using var response = await _httpClient.GetAsync(
                    $"api/auth/getUser/{userId}",
                    token);

                response.EnsureSuccessStatusCode();

                return await response.Content
                    .ReadFromJsonAsync<UserDTO>(
                        cancellationToken: token);

            }, cancellationToken);
    }
}
```

Let's understand the important changes.

1\. Inject IUserServicePolicies

```
private readonly IUserServicePolicies _userServicePolicies;
```

We declare a field to store the injected policy service.

In the constructor:

```
public UserServiceClient(
    HttpClient httpClient,
    IUserServicePolicies userServicePolicies)
{
    _httpClient = httpClient;
    _userServicePolicies = userServicePolicies;
}
```

The DI container provides:

- `HttpClient` through `AddHttpClient<UserServiceClient>()`.
- `IUserServicePolicies` through our Singleton registration.

2\. Get the configured pipeline

```
_userServicePolicies.GetUserServicePipeline()
```

This retrieves the pipeline that we created inside `UserServicePolicies`.

3\. Execute the HTTP request through Polly

```
.ExecuteAsync(async token =>
{
    // HTTP request
}, cancellationToken);
```

This is the most important part.

Instead of directly executing the HTTP request, we execute it through Polly's `ExecuteAsync()` method.

If the operation encounters an exception that matches our `ShouldHandle` predicate, Polly waits for the configured delay and executes the callback again.

4\. Send the HTTP request

```
using var response = await _httpClient.GetAsync(
    $"api/auth/getUser/{userId}",
    token);
```

This sends the GET request to User Service.

We pass the cancellation token so that cancellation can propagate to the HTTP request.

5\. Ensure the HTTP request succeeded

```
response.EnsureSuccessStatusCode();
```

This is particularly important.

`HttpClient.GetAsync()` does not automatically throw an exception for HTTP 500 or 503 responses. It normally returns an `HttpResponseMessage` containing the unsuccessful status code.

`EnsureSuccessStatusCode()` throws an `HttpRequestException` when the response is unsuccessful.

That exception is then evaluated by our Retry strategy.

For example:

```
HTTP 503
   |
   v
EnsureSuccessStatusCode()
   |
   v
HttpRequestException
   |
   v
Polly ShouldHandle
   |
   v
Retry
```

## 6. Complete execution flow

Let's understand how everything works together.

Client sends Create Order request

Order Service

Calls UserServiceClient

IUserServicePolicies

Returns the configured pipeline

Polly Resilience Pipeline

Executes the HTTP request with Retry

User Service

Success

Return user details and continue order creation.

Transient failure

Wait 2 seconds and retry, up to 3 times.

### Example execution

Suppose User Service is temporarily unavailable.

| Attempt          | Result   | Polly action        |
| ---------------- | -------- | ------------------- |
| Original request | HTTP 503 | Wait 2 seconds      |
| Retry 1          | HTTP 503 | Wait 2 seconds      |
| Retry 2          | HTTP 503 | Wait 2 seconds      |
| Retry 3          | HTTP 200 | Return user details |

The operation succeeds on the fourth execution.

If all four executions fail with handled errors, Polly stops retrying and propagates the final exception to the calling code.

## 7. Important points to remember

1. Retry is useful for temporary failures, not permanent failures.
2. `MaxRetryAttempts = 3` means three retries plus the original execution.
3. `Delay` defines the waiting period between retries.
4. `BackoffType` determines how the waiting period changes.
5. `ShouldHandle` determines which failures should trigger retries.
6. `ExecuteAsync()` executes the operation through the configured pipeline.
7. `Build()` creates the pipeline.
8. Reuse the pipeline instead of rebuilding it for every request.
9. Pass cancellation tokens through the execution and HTTP request.
10. Do not blindly retry operations that can create duplicate side effects.

Important note about POST requests: Our example uses GET, which is generally safe to retry. If we retry an order-creation POST, the first request might have successfully created an order even though its response was lost. Retrying could create a duplicate order. Such operations need an idempotency mechanism before automatic retries are applied.

Also, avoid combining this manually executed Retry pipeline with another HTTP handler that independently retries the same request. Otherwise, you may unintentionally multiply the number of attempts.

## 8. Quick revision

| Question                         | Answer                                                                                                 |
| -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| What is Fault Tolerance?         | Ability of a system to handle failures and continue operating.                                         |
| What is Retry?                   | Re-executing an operation after a handled failure.                                                     |
| What is Polly?                   | A .NET resilience library.                                                                             |
| What is a Resilience Pipeline?   | An execution pipeline containing configured resilience strategies.                                     |
| What does `AddRetry()` do?       | Adds Retry to the pipeline.                                                                            |
| What does `Build()` do?          | Creates the configured pipeline.                                                                       |
| What does `ExecuteAsync()` do?   | Executes an asynchronous operation through the pipeline.                                               |
| Why Singleton?                   | Reuses the thread-safe pipeline instance.                                                              |
| Why `EnsureSuccessStatusCode()`? | Converts unsuccessful HTTP responses into exceptions that our configured Retry predicate can evaluate. |
| Why `ShouldHandle`?              | Controls which failures trigger retries.                                                               |

### Official references

- [Polly v8 – Retry Strategy](https://www.pollydocs.org/strategies/retry?utm_source=chatgpt.com)&#x20;

  [image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

  Polly+1

- [Polly v7 to v8 Migration Guide](https://www.pollydocs.org/migration-v8.html?utm_source=chatgpt.com)&#x20;

  [image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

  Polly



This completes the Retry Pattern using Polly v8, including the concept, configuration, project structure, dependency injection, implementation, and execution flow.
