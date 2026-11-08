Bhai, ye notes directly GitHub README mein add kar sakta hai. Pehle concept, phir use cases, aur finally Product Service ka practical implementation. 😎

# Polly Fallback Strategy in .NET 8

## 1. What is Fallback?

Fallback is a resilience strategy that provides an alternative response or action when the original operation fails.

Simple words mein, agar koi service available nahi hai ya request fail ho jaati hai, toh application completely fail hone ke bajaye ek alternative response provide kar sakti hai.

Example:

Maan le Order Service, Product Service ko product details fetch karne ke liye call karta hai.

- Product Service working hai → Actual product details return hongi.
- Product Service unavailable hai → Fallback execute hoga.
- Fallback → Alternative response return karega, jaise HTTP 503 or cached product details.

## 2. When should we use Fallback?

Fallback tab use karte hain jab application ko failure ke situation mein bhi gracefully respond karna ho.

Common use cases:

| Scenario                           | Fallback action                     |
| ---------------------------------- | ----------------------------------- |
| Product Service unavailable        | Return cached product details       |
| Recommendation Service unavailable | Return default recommendations      |
| Weather API unavailable            | Return last known weather data      |
| Payment Service unavailable        | Return a temporary failure response |
| External API timeout               | Return an alternative response      |

Important: Fallback response business logic ke according decide karna chahiye.

For example, Order Service mein product ki price verify nahi ho rahi hai toh default price use karke order place nahi karna chahiye.

## 3. How to implement Fallback using Polly v8?

Polly v8 provides `AddFallback()` to configure a fallback strategy.

Let's implement it in our Order Microservice for Product Service calls.

Step 1: Configure the Fallback Strategy

```
using Polly;
using Polly.Fallback;
using Polly.CircuitBreaker;
using System.Net;

pipeline.AddFallback(
    new FallbackStrategyOptions<HttpResponseMessage>
    {
        ShouldHandle = new PredicateBuilder<HttpResponseMessage>()
            .Handle<HttpRequestException>()
            .Handle<BrokenCircuitException>(),

        FallbackAction = args =>
        {
            Console.WriteLine("Product Service is unavailable!");

            var fallbackResponse = new HttpResponseMessage(
                HttpStatusCode.ServiceUnavailable
            );

            return Outcome.FromResultAsOutcome(fallbackResponse);
        }
    });
```

Step 2: Understand the properties

| Property                        | Purpose                                                        |
| ------------------------------- | -------------------------------------------------------------- |
| `ShouldHandle`                  | Defines which failures should trigger fallback                 |
| `FallbackAction`                | Defines what action should execute when a failure occurs       |
| `Outcome.FromResultAsOutcome()` | Wraps the fallback response in Polly's expected outcome format |

In this example:

- `HttpRequestException` → Handles network-related failures.
- `BrokenCircuitException` → Handles requests blocked by an open circuit breaker.
- `FallbackAction` → Returns HTTP 503 Service Unavailable.

Note: HTTP error responses like 500 or 503 are returned by HttpClient as responses, not automatically thrown as exceptions. If you also want fallback for these responses, add a `HandleResult()` condition.

For example:

```
.HandleResult(response =>
    (int)response.StatusCode >= 500)
```

## 4. How does Fallback work?

Order Service

Requests product details

Product Service

Request fails

Fallback Strategy

Executes the configured alternative action

Fallback Response

HTTP 503 Service Unavailable

## 5. Difference between Retry, Circuit Breaker and Fallback

| Strategy        | Responsibility                                         |
| --------------- | ------------------------------------------------------ |
| Retry           | Tries the failed operation again                       |
| Circuit Breaker | Temporarily blocks calls to an unhealthy service       |
| Fallback        | Provides an alternative response when a failure occurs |

Remember:

- Retry → Try again.
- Circuit Breaker → Stop calling temporarily.
- Fallback → Provide an alternative.

One important concept: Fallback doesn't automatically retry a request or recover the service. It only handles configured failures by executing the alternative action.
