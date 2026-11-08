# Circuit Breaker Pattern in .NET Microservices (Polly v8)

We already implemented the Retry pattern for communication between the Order Service and User Service. Now let's understand the Circuit Breaker pattern and integrate it into the same architecture.

## 1. What is a Circuit Breaker?

The Circuit Breaker pattern prevents a microservice from repeatedly calling another microservice when that service is failing or unavailable.

Imagine this scenario:

- Order Service calls User Service to validate a user.
- User Service is down or continuously returning errors.
- Without a Circuit Breaker, Order Service keeps sending requests.
- These repeated requests waste resources and can make recovery harder.

The Circuit Breaker temporarily stops outgoing requests after detecting repeated failures. It gives the failing service time to recover.

Think of it like an electrical circuit breaker: when it detects a problem, it cuts the connection to prevent further damage.

## 2. Three states of Circuit Breaker

A Circuit Breaker has three states:

CLOSED

Normal operation

Requests are allowed to pass through to User Service. The breaker monitors failures.

OPEN

Requests are blocked

Once the failure threshold is reached, the circuit opens. Requests fail immediately without calling User Service.

HALF-OPEN

Testing recovery

After the break duration expires, a limited number of requests are allowed through to check whether User Service has recovered.

The transitions are:

- Closed → Open: The configured failure threshold is reached.
- Open → Half-Open: The configured break duration expires.
- Half-Open → Closed: The test request succeeds.
- Half-Open → Open: The test request fails.

## 3. Understanding the configuration

Polly v8 uses a sampling-based Circuit Breaker. It doesn't simply open after a fixed number of consecutive failures.

These are the important properties:

| Property            | Meaning                                                 |
| ------------------- | ------------------------------------------------------- |
| `FailureRatio`      | Failure percentage required to open the circuit         |
| `SamplingDuration`  | Time window used to calculate failures                  |
| `MinimumThroughput` | Minimum number of requests required in that time window |
| `BreakDuration`     | How long the circuit stays open                         |

For example, suppose we configure:

```
FailureRatio = 0.5,
SamplingDuration = TimeSpan.FromSeconds(10),
MinimumThroughput = 4,
BreakDuration = TimeSpan.FromSeconds(30)
```

This means:

- Polly examines requests within a 10-second window.
- At least 4 requests must pass through the circuit.
- If 50% or more of those requests fail, the circuit opens.
- It remains open for 30 seconds.
- After 30 seconds, it allows a probe request to check whether User Service has recovered.

Important: If only 3 requests fail within those 10 seconds, the circuit will not open because `MinimumThroughput` is 4.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1



## 4. Implement Circuit Breaker in our project

We'll implement it in the same place where we configured Retry:

- `IUserMicroservicePolicies`
- `UserMicroservicePolicies`
- `Program.cs`

We will continue using `AddResilienceHandler()` and the typed `UserServiceClient`. No manual `ExecuteAsync()` is required.

### Step 1: Update `IUserMicroservicePolicies`

Add a method for configuring the Circuit Breaker.

```
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

### Step 2: Update `UserMicroservicePolicies.cs`

Add the Circuit Breaker implementation.

```
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

Understanding our configuration:

- `FailureRatio = 0.5`: Circuit opens when at least 50% of sampled requests fail.
- `SamplingDuration = 10 seconds`: Polly monitors failures over a 10-second window.
- `MinimumThroughput = 4`: At least four executions must occur in that window.
- `BreakDuration = 30 seconds`: Circuit stays open for 30 seconds before allowing a recovery probe.
- `ShouldHandle`: Only the specified exceptions and HTTP responses count as failures.

These values are for learning and testing. Production thresholds should be chosen based on actual traffic and service behavior.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1



### Step 3: Update `Program.cs`

We already have this registration:

```
builder.Services.AddSingleton<
    IUserMicroservicePolicies,
    UserMicroservicePolicies>();
```

Now update the existing `AddResilienceHandler()` configuration:

```
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

Why are we adding Retry first and Circuit Breaker second?

The first strategy is the outer strategy, and the second is the inner strategy.

Our execution order is:

1. Retry
2. Circuit Breaker
3. HTTP request to User Service

This means the Circuit Breaker observes individual HTTP attempts, including retry attempts. This is also the ordering used in Microsoft's standard HTTP resilience pipeline.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://devblogs.microsoft.com\&sz=32)

.NET Blog+1



### Step 4: Handle `BrokenCircuitException`

There is one important change needed in our existing `UserServiceClient`.

When the circuit is open, Polly throws a `BrokenCircuitException` without sending an HTTP request to User Service.

Our existing client only catches `HttpRequestException`, so add another catch:

```
catch (BrokenCircuitException)
{
    return null;
}
catch (HttpRequestException)
{
    return null;
}
```

Add this using statement:

```
using Polly.CircuitBreaker;
```

This allows our application to handle an open circuit gracefully instead of allowing the exception to propagate unexpectedly.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.pollydocs.org\&sz=32)

Polly+1



## 5. Complete Circuit Breaker execution diagram

\#chatgpt-mermaid-\_r_4u1\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_4u1\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_4u1\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_4u1\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_4u1\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_4u1\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_4u1\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_4u1\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_4u1\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_4u1\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_4u1\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_4u1\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_4u1\_ p{margin:0;}#chatgpt-mermaid-\_r_4u1\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_4u1\_ .label text,#chatgpt-mermaid-\_r_4u1\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .node rect,#chatgpt-mermaid-\_r_4u1\_ .node circle,#chatgpt-mermaid-\_r_4u1\_ .node ellipse,#chatgpt-mermaid-\_r_4u1\_ .node polygon,#chatgpt-mermaid-\_r_4u1\_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .rough-node .label text,#chatgpt-mermaid-\_r_4u1\_ .node .label text,#chatgpt-mermaid-\_r_4u1\_ .image-shape .label,#chatgpt-mermaid-\_r_4u1\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_4u1\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .rough-node .label,#chatgpt-mermaid-\_r_4u1\_ .node .label,#chatgpt-mermaid-\_r_4u1\_ .image-shape .label,#chatgpt-mermaid-\_r_4u1\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_4u1\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_4u1\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_4u1\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_4u1\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_4u1\_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_4u1\_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_4u1\_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_4u1\_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-\_r_4u1\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_4u1\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_4u1\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_4u1\_ .icon-shape,#chatgpt-mermaid-\_r_4u1\_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_4u1\_ .icon-shape p,#chatgpt-mermaid-\_r_4u1\_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-\_r_4u1\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_4u1\_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_4u1\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_4u1\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_4u1\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_4u1\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_4u1\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_4u1\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_4u1\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_4u1\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_4u1\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_4u1\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_4u1\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_4u1\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_4u1\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_4u1\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .node rect,#chatgpt-mermaid-\_r_4u1\_ .node circle,#chatgpt-mermaid-\_r_4u1\_ .node ellipse,#chatgpt-mermaid-\_r_4u1\_ .node polygon,#chatgpt-mermaid-\_r_4u1\_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_4u1\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_4u1\_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_4u1\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_4u1\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_4u1\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Order Service sends requestRetry StrategyCircuit BreakerCircuit State?Call User ServiceThrow BrokenCircuitExceptionRequest successful?Return successful responseRecord failureFailure threshold reached?Retry if eligibleOpen CircuitBlock subsequent requestsWait for Break DurationHalf-OpenAllow probe requestProbe successful?Close CircuitHandle failure in Order ServiceClosedOpenYesNoNoYesYesNo

## 6. Retry vs Circuit Breaker

| Retry                          | Circuit Breaker                                   |
| ------------------------------ | ------------------------------------------------- |
| Tries a failed request again.  | Temporarily blocks requests to a failing service. |
| Useful for temporary failures. | Useful when a service is consistently failing.    |
| Waits before retrying.         | Fails fast while open.                            |
| Doesn't stop future requests.  | Stops requests until recovery is tested.          |

One important thing to remember: Retry and Circuit Breaker solve different problems. Retry attempts recovery from temporary failures, while Circuit Breaker prevents repeated calls to an unhealthy dependency.

## 7. How can we test it?

For our project:

1. Keep User Service running.
2. Send requests from Order Service and verify normal operation.
3. Stop User Service.
4. Send enough requests to meet the configured failure threshold.
5. Observe that the circuit opens.
6. Send another request while the circuit is open. It should fail immediately without reaching User Service.
7. Start User Service again and wait 30 seconds.
8. Send another request. Polly allows a probe request.
9. If successful, the circuit returns to Closed.

Interview question: What is the difference between Retry and Circuit Breaker?

A good answer:

"Retry attempts to execute a failed operation again, assuming the failure might be temporary. Circuit Breaker monitors failures and temporarily blocks requests when a dependency becomes unhealthy. Retry helps recover from transient failures, whereas Circuit Breaker prevents cascading failures and gives the dependent service time to recover."

Official reference: [Polly v8 – Circuit Breaker documentation](https://www.pollydocs.org/strategies/circuit-breaker?utm_source=chatgpt.com).
