# Circuit Breaker Pattern in .NET Microservices (Polly v8)

## 1. What is Circuit Breaker?

The Circuit Breaker pattern prevents a microservice from repeatedly calling another microservice when that service is failing or unavailable.

Imagine this scenario in our e-commerce application:

1. Order Service calls User Service to validate a user.
2. User Service is down or continuously returning server errors.
3. Order Service keeps sending requests to User Service.
4. These repeated requests waste resources and can make recovery harder.

The Circuit Breaker pattern temporarily stops outgoing requests after detecting repeated failures. It gives the failing service time to recover.

**Think of it like an electrical circuit breaker:** when it detects a problem, it cuts the connection to prevent further damage.

### Why do we need Circuit Breaker?

- Prevents repeated calls to an unhealthy service.
- Avoids wasting resources on requests that are likely to fail.
- Helps prevent cascading failures between microservices.
- Gives the failing service time to recover.
- Allows the application to fail fast instead of waiting for repeated timeouts.

## 2. Three states of Circuit Breaker

Circuit Breaker has three main states:

### 1. Closed State

- This is the normal operating state.
- Requests are allowed to pass through to the target service.
- The Circuit Breaker monitors failures.
- If the configured failure threshold is reached, it moves to the Open state.

### 2. Open State

- Requests are blocked immediately.
- Requests are not forwarded to the target service.
- Polly throws a `BrokenCircuitException`.
- The circuit remains open for the configured `BreakDuration`.

### 3. Half-Open State

- After the break duration expires, the circuit moves to Half-Open.
- Polly allows a probe request to check whether the target service has recovered.
- If the probe succeeds, the circuit moves to Closed.
- If the probe fails, the circuit moves back to Open.

### State transitions

| Current State | Condition | Next State |
|---|---|---|
| Closed | Failure threshold reached | Open |
| Open | Break duration expires | Half-Open |
| Half-Open | Probe succeeds | Closed |
| Half-Open | Probe fails | Open |

## 3. Understanding Circuit Breaker configuration

Polly v8 uses a sampling-based Circuit Breaker.

Unlike a simple consecutive-failure counter, it evaluates the failure ratio over a configured time window.

The following are the important properties:

| Property | Description |
|---|---|
| `FailureRatio` | Failure percentage required to open the circuit. |
| `SamplingDuration` | Time window used to calculate the failure ratio. |
| `MinimumThroughput` | Minimum number of executions required within the sampling window. |
| `BreakDuration` | How long the circuit remains open before allowing a recovery probe. |

### Example configuration

```csharp
FailureRatio = 0.5,
SamplingDuration = TimeSpan.FromSeconds(10),
MinimumThroughput = 4,
BreakDuration = TimeSpan.FromSeconds(30)
```

Let's understand this configuration:

- `FailureRatio = 0.5`: At least 50% of sampled executions must fail.
- `SamplingDuration = 10 seconds`: Polly examines executions within a 10-second window.
- `MinimumThroughput = 4`: At least four executions must occur in that window.
- `BreakDuration = 30 seconds`: The circuit stays open for 30 seconds before allowing a recovery probe.

### Practical example

Suppose Order Service sends 4 requests to User Service within 10 seconds.

| Request | Result |
|---|---|
| Request 1 | Failed |
| Request 2 | Failed |
| Request 3 | Successful |
| Request 4 | Failed |

Here:

- Total requests = 4
- Failed requests = 3
- Failure ratio = 3 / 4 = 75%
- Configured failure ratio = 50%

Since the minimum throughput is reached and the failure ratio meets the configured threshold, the circuit opens.

**Important:** If only 3 requests are made within the sampling window, the circuit will not open, even if all 3 fail, because the minimum throughput of 4 has not been reached.

## 4. Implement Circuit Breaker in our project

We already implemented Retry for communication between Order Service and User Service.

Now we will add Circuit Breaker using the same architecture.

Our implementation contains:

1. `IUserMicroservicePolicies` – Interface.
2. `UserMicroservicePolicies` – Implementation.
3. `Program.cs` – Register and configure the resilience pipeline.
4. `UserServiceClient` – Handle Circuit Breaker exceptions.

We will continue using Polly v8 and `AddResilienceHandler()`.

### Step 1: Update IUserMicroservicePolicies

Add a method for configuring Circuit Breaker.

**IUserMicroservicePolicies.cs**

```csharp
using Polly;

namespace BusinessLogicLayer.Policies
{
    public interface IUserMicroservicePolicies
    {
        void ConfigureRetry(
            ResiliencePipelineBuilder<HttpResponseMessage> pipeline);

        void ConfigureCircuitBreaker(
            ResiliencePipelineBuilder<HttpResponseMessage> pipeline);
    }
}
```

### Step 2: Update UserMicroservicePolicies.cs

Add the Circuit Breaker implementation.

**UserMicroservicePolicies.cs**

```csharp
using Polly;
using Polly.CircuitBreaker;
using Polly.Retry;
using System.Net;

namespace BusinessLogicLayer.Policies
{
    public class UserMicroservicePolicies : IUserMicroservicePolicies
    {
        public void ConfigureRetry(
            ResiliencePipelineBuilder<HttpResponseMessage> pipeline)
        {
            pipeline.AddRetry(new RetryStrategyOptions<HttpResponseMessage>
            {
                MaxRetryAttempts = 3,
                Delay = TimeSpan.FromSeconds(2),
                UseJitter = true,
                BackoffType = DelayBackoffType.Exponential,

                ShouldHandle = new PredicateBuilder<HttpResponseMessage>()
                    .Handle<HttpRequestException>()
                    .HandleResult(response =>
                        response.StatusCode == HttpStatusCode.RequestTimeout ||
                        response.StatusCode == HttpStatusCode.TooManyRequests ||
                        (int)response.StatusCode >= 500)
            });
        }

        public void ConfigureCircuitBreaker(
            ResiliencePipelineBuilder<HttpResponseMessage> pipeline)
        {
            pipeline.AddCircuitBreaker(
                new CircuitBreakerStrategyOptions<HttpResponseMessage>
                {
                    FailureRatio = 0.5,
                    SamplingDuration = TimeSpan.FromSeconds(10),
                    MinimumThroughput = 4,
                    BreakDuration = TimeSpan.FromSeconds(30),

                    ShouldHandle = new PredicateBuilder<HttpResponseMessage>()
                        .Handle<HttpRequestException>()
                        .HandleResult(response =>
                            response.StatusCode == HttpStatusCode.RequestTimeout ||
                            response.StatusCode == HttpStatusCode.TooManyRequests ||
                            (int)response.StatusCode >= 500)
                });
        }
    }
}
```

**Understanding the implementation:**

- `AddCircuitBreaker()` adds the Circuit Breaker strategy to the resilience pipeline.
- `FailureRatio` defines the failure percentage threshold.
- `SamplingDuration` defines the time window for monitoring failures.
- `MinimumThroughput` defines the minimum number of executions needed to evaluate the failure ratio.
- `BreakDuration` defines how long the circuit stays open.
- `ShouldHandle` defines which exceptions and HTTP responses count as failures.

We have configured the same failure conditions for Retry and Circuit Breaker:

- `HttpRequestException`
- HTTP 408 – Request Timeout
- HTTP 429 – Too Many Requests
- HTTP 5xx – Server Errors

This means ordinary HTTP 4xx responses, such as 400 or 404, are not treated as retryable or circuit-breaking failures by these predicates.

### Step 3: Update Program.cs

We already registered the policy implementation through Dependency Injection:

```csharp
builder.Services.AddSingleton<
    IUserMicroservicePolicies,
    UserMicroservicePolicies>();
```

Now update the existing `AddResilienceHandler()` configuration.

```csharp
builder.Services.AddHttpClient<UserServiceClient>(client =>
{
    client.BaseAddress = new Uri(
        builder.Configuration["UserService:BaseUrl"]
        ?? "https://localhost:7061");
})
.AddResilienceHandler("UserServiceResilience", (pipeline, context) =>
{
    var policies = context.ServiceProvider
        .GetRequiredService<IUserMicroservicePolicies>();

    // Retry Strategy
    policies.ConfigureRetry(pipeline);

    // Circuit Breaker Strategy
    policies.ConfigureCircuitBreaker(pipeline);
});
```

That's it. We have integrated both Retry and Circuit Breaker into the same HTTP resilience pipeline.

**Why do we configure Retry first and Circuit Breaker second?**

The first strategy is the outer strategy, and the second is the inner strategy.

Our execution order is:

1. Retry
2. Circuit Breaker
3. HTTP request to User Service

The Retry strategy can retry eligible failures. The Circuit Breaker monitors the individual attempts and can stop further attempts when the circuit opens.

If the Circuit Breaker is already open, it throws `BrokenCircuitException`. Our Retry predicate does not handle that exception, so it does not keep retrying an already-open circuit.

### Step 4: Handle BrokenCircuitException in UserServiceClient

When the circuit is open, Polly throws a `BrokenCircuitException` without sending an HTTP request to User Service.

Our existing `UserServiceClient` catches `HttpRequestException`, but it does not currently catch `BrokenCircuitException`.

Update the exception handling:

```csharp
catch (BrokenCircuitException)
{
    return null;
}
catch (HttpRequestException)
{
    return null;
}
```

Add the following using statement:

```csharp
using Polly.CircuitBreaker;
```

Now our client handles both situations:

- `HttpRequestException`: An HTTP communication failure.
- `BrokenCircuitException`: Polly blocked the request because the circuit is open.

Returning `null` allows the calling business logic to handle the unavailable User Service according to the application's requirements.

## 5. Simple Circuit Breaker flow diagram

```text
        Order Service
              |
              v
       Circuit Breaker
              |
       Is circuit closed?
          /       \
        YES        NO
         |          |
         v          v
   Call User      Block Request
    Service       (Fail Fast)
         |
         v
   Is request successful?
       /       \
     YES        NO
      |          |
      v          v
  Stay CLOSED  Count Failure
                  |
                  v
          Failure threshold reached?
                /       \
              NO         YES
              |           |
              v           v
          Stay CLOSED    OPEN
                           |
                           v
                    Wait 30 seconds
                           |
                           v
                       HALF-OPEN
                           |
                           v
                     Test a request
                       /       \
                     YES        NO
                      |          |
                      v          v
                   CLOSED      OPEN
```

### Quick revision

- **Closed:** Requests pass normally.
- **Open:** Requests are blocked immediately.
- **Half-Open:** A probe request checks whether the service has recovered.

## 6. Retry vs Circuit Breaker

| Retry | Circuit Breaker |
|---|---|
| Tries a failed request again. | Temporarily blocks requests to a failing service. |
| Useful for temporary failures. | Useful when a service is consistently failing. |
| Waits before retrying. | Fails fast while open. |
| Does not stop future requests. | Stops requests until recovery is tested. |
| Focuses on recovering from transient failures. | Focuses on preventing repeated calls to an unhealthy dependency. |

**Can we use Retry and Circuit Breaker together?**

Yes. They solve different problems.

Retry attempts to recover from temporary failures, while Circuit Breaker prevents repeated calls to an unhealthy service.

## 7. How to test Circuit Breaker in our project

Follow these steps:

1. Start User Service.
2. Start Order Service.
3. Send requests from Order Service and verify that they work normally.
4. Stop User Service.
5. Send enough requests to meet the configured failure threshold.
6. Observe that the circuit opens.
7. Send another request while the circuit is open. It should fail immediately without reaching User Service.
8. Start User Service again.
9. Wait for the configured 30-second break duration to expire.
10. Send another request. Polly allows a probe request to check whether User Service has recovered.
11. If the probe succeeds, the circuit returns to Closed.

**Note:** Because Retry is also enabled, one incoming request from Order Service can result in multiple HTTP attempts. The Circuit Breaker monitors the attempts that reach it. Therefore, test the behavior while considering the configured retry count.

---

# 8. Scenario-Based Interview Questions

### Q1. Your Order Service is calling User Service, but User Service is down. What happens?

**Answer:**

Without Circuit Breaker, Order Service may continue sending requests and waste resources.

With Circuit Breaker, Polly monitors failures. Once the configured failure threshold is reached, it opens the circuit and blocks subsequent requests for the configured break duration.

### Q2. What happens when the Circuit Breaker is in the Open state?

**Answer:**

When the circuit is Open, requests are not forwarded to the target service.

Polly immediately throws a `BrokenCircuitException`, allowing the application to fail fast instead of waiting for another HTTP request to fail.

### Q3. User Service was down, but now it has recovered. How does Order Service know?

**Answer:**

After the configured `BreakDuration` expires, the Circuit Breaker transitions to Half-Open.

Polly allows a probe request to reach User Service.

If the request succeeds, the circuit moves to Closed. If it fails, the circuit moves back to Open.

### Q4. What is the difference between Closed, Open, and Half-Open?

**Answer:**

- Closed: Requests are allowed, and failures are monitored.
- Open: Requests are blocked immediately.
- Half-Open: A probe request is allowed to check whether the service has recovered.

### Q5. You configured FailureRatio as 50%, SamplingDuration as 10 seconds, and MinimumThroughput as 4. Only three requests failed. Will the circuit open?

**Answer:**

No.

The Circuit Breaker requires at least four executions within the 10-second sampling window before evaluating the failure ratio.

Even if all three requests fail, the minimum throughput has not been reached.

### Q6. What happens if the Circuit Breaker is Open and another request arrives?

**Answer:**

Polly immediately throws `BrokenCircuitException`.

The request is not sent to the target microservice. This is called fail-fast behavior.

### Q7. Can we use Retry and Circuit Breaker together?

**Answer:**

Yes.

Retry handles temporary failures by attempting the request again.

Circuit Breaker monitors failures and prevents repeated calls when the target service becomes unhealthy.

Using both helps improve the resilience of microservice communication.

### Q8. What is a cascading failure?

**Answer:**

A cascading failure occurs when one service becomes unhealthy and causes other dependent services to become unhealthy as well.

For example:

1. User Service becomes slow or unavailable.
2. Order Service keeps waiting for User Service responses.
3. Order Service starts consuming more resources.
4. Order Service may also become unavailable.

Circuit Breaker helps prevent this by stopping repeated calls to the unhealthy dependency.

### Q9. What is fail-fast behavior?

**Answer:**

Fail-fast means immediately returning a failure instead of waiting for an operation that is likely to fail.

When the Circuit Breaker is Open, it blocks requests immediately and throws `BrokenCircuitException`.

### Q10. Does Circuit Breaker retry failed requests?

**Answer:**

No.

Circuit Breaker does not retry requests. Its responsibility is to monitor failures and temporarily block requests when the failure threshold is reached.

Retry is a separate resilience strategy.

### Q11. Does Polly v8 use consecutive failure counting?

**Answer:**

No. Polly v8's standard Circuit Breaker uses a sampling-based approach.

It evaluates the failure ratio over a configured sampling duration and requires the minimum throughput to be reached.

### Q12. What happens if the probe request fails in Half-Open state?

**Answer:**

The Circuit Breaker moves back to Open and blocks requests for another configured break duration.

### Q13. What is BrokenCircuitException?

**Answer:**

`BrokenCircuitException` is an exception thrown by Polly when an operation is attempted while the Circuit Breaker is Open.

It indicates that the request was blocked and was not executed against the target service.

### Q14. Does Circuit Breaker guarantee that the target service is healthy after the break duration?

**Answer:**

No.

The break duration only determines when Polly can test the service again.

The probe request checks whether the service has recovered. A successful probe closes the circuit; a failed probe opens it again.

### Q15. Should we configure Circuit Breaker for every HTTP request?

**Answer:**

Not necessarily.

Circuit Breaker is useful when communicating with external dependencies or downstream services that may become unavailable.

Its configuration should depend on the service's traffic, failure characteristics, and business requirements.

---

## 9. Quick Interview Revision

| Question | Short Answer |
|---|---|
| What is Circuit Breaker? | Prevents repeated calls to an unhealthy service. |
| How many states? | Three: Closed, Open, Half-Open. |
| What is FailureRatio? | Failure percentage threshold. |
| What is SamplingDuration? | Time window for monitoring failures. |
| What is MinimumThroughput? | Minimum executions required for evaluation. |
| What is BreakDuration? | Time the circuit stays open. |
| What is BrokenCircuitException? | Exception thrown when an open circuit blocks a request. |
| Does Circuit Breaker retry? | No. Retry is a separate strategy. |
| Can Retry and Circuit Breaker work together? | Yes. |
| What is fail-fast? | Blocking a request immediately instead of executing it. |

## 10. Official Documentation

- [Polly v8 – Circuit Breaker Strategy](https://www.pollydocs.org/strategies/circuit-breaker.html)
- [Polly v8 – Migration Guide](https://www.pollydocs.org/migration-v8.html)
- [Microsoft – HTTP Resilience](https://learn.microsoft.com/en-us/dotnet/core/resilience/http-resilience)

**Final takeaway:** Retry tries again when a request fails. Circuit Breaker stops calling a service when it is repeatedly failing. Together, they help make communication between microservices more resilient.
