Bro, here's how I'd answer this in an interview. Keep it simple, but demonstrate that you understand the architecture and implementation.

# 1. First explain your approach

"If I have to protect a Forgot Password API from spam requests, I would implement IP-based and Email-based Fixed Window Rate Limiting."

My requirement would be:

- Maximum 5 requests per IP in 10 minutes.
- Maximum 5 requests per email in 10 minutes.
- If either limit is reached, reject the request with HTTP 429.
- Both counters should be checked together.

### Why IP + Email?

Consider this example:

- An attacker sends requests using the same IP but different email addresses.
- Another attacker uses different IP addresses to target the same email.

IP checking alone cannot prevent the second scenario. Email checking alone cannot prevent the first.

That's why we check both.

# 2. Explain the architecture

Client

ForgotPassword Action Filter

Reads client IP and email

Rate Limiter Service

Checks both counters

Storage

In-memory for single instance, Redis for multiple instances

Forgot Password Business Logic

Send reset email

# 3. How would I implement this in .NET?


# 4. What if the application has multiple instances?

This is an important interview follow-up.

Suppose your monolithic application runs on three servers.

Each server has its own memory. So the counters won't be shared.

My solution:

- Replace the in-memory dictionary with Redis.
- Store IP and Email counters in Redis.
- Use atomic Redis operations (Lua script) to check and increment both counters together.
- All application instances use the same Redis storage.

The Action Filter and interface can remain almost the same; only the rate-limiter implementation changes to asynchronous Redis operations.

# 5. What should you say in the interview?

Here's a concise answer worth practising:

> "I would implement a custom Action Filter for the Forgot Password endpoint. The filter extracts the client's IP address and email address from the request and calls a rate-limiter service.
>
> The rate-limiter service maintains two independent Fixed Window counters: one for IP and another for email. For example, I would allow five requests per IP and five requests per email within ten minutes.
>
> If either counter reaches its limit, I return HTTP 429. Otherwise, I allow the controller action to execute.
>
> For a single application instance, I can use thread-safe in-memory storage. If the application is deployed across multiple instances, I would use Redis with atomic operations so all instances share the same counters."

## 6. Four things to remember

| Interview question | Answer                                    |
| ------------------ | ----------------------------------------- |
| Which algorithm?   | Fixed Window                              |
| Why IP + Email?    | Prevent different types of spam attacks   |
| How in .NET?       | Custom Action Filter                      |
| Why Redis?         | Shared counters across multiple instances |

One production caveat: The in-memory code above is an interview-friendly starting implementation. Add cleanup for expired dictionary entries, hash identifiers rather than storing raw email/IP in keys, configure trusted forwarded headers behind Ocelot, and use Redis atomic operations for multi-instance deployment. Also consider a cooldown on actual email sending so attackers cannot repeatedly trigger emails within the allowed request limit.

My advice: understand the Action Filter → Rate Limiter Service → Storage flow. If you can explain those three responsibilities clearly, you can handle most LLD follow-up questions.



We'll implement this with just one custom Action Filter and one Dictionary.

Our requirement:

- Maximum 5 requests per IP in 10 minutes.
- Maximum 5 requests per Email in 10 minutes.
- If either limit is reached, return HTTP 429.

## 1. Custom Action Filter

Create `ForgotPasswordRateLimitFilter.cs`.

```

using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Filters;

public class ForgotPasswordRateLimitFilter : IActionFilter
{
    private static Dictionary<string, int> counters = new();

    private static Dictionary<string, DateTime> startTimes = new();

    public void OnActionExecuting(ActionExecutingContext context)
    {
        var ip = context.HttpContext.Connection.RemoteIpAddress?.ToString() ?? "unknown";

        var request = context.ActionArguments.Values
            .OfType<ForgotPasswordRequest>().First();

        var email = request.Email.ToLower();

        string ipKey = "IP_" + ip;
        string emailKey = "EMAIL_" + email;

        // Check both limits
        if (IsLimitReached(ipKey) || IsLimitReached(emailKey))
        {
            context.Result = new StatusCodeResult(429);
            return;
        }

        // Increase both counters
        Increment(ipKey);
        Increment(emailKey);
    }

    private bool IsLimitReached(string key)
    {
        if (!startTimes.ContainsKey(key))
            return false;

        if (DateTime.UtcNow - startTimes[key] >= TimeSpan.FromMinutes(10))
            return false;

        return counters[key] >= 5;
    }

    private void Increment(string key)
    {
        if (!startTimes.ContainsKey(key) ||
            DateTime.UtcNow - startTimes[key] >= TimeSpan.FromMinutes(10))
        {
            counters[key] = 0;
            startTimes[key] = DateTime.UtcNow;
        }

        counters[key]++;
    }

    public void OnActionExecuted(ActionExecutedContext context) { }
}

```

## 2. Register in Program.cs

```
builder.Services.AddScoped<ForgotPasswordRateLimitFilter>();
```

## 3. Apply to your API

```
[HttpPost("forgot-password")]
[ServiceFilter(typeof(ForgotPasswordRateLimitFilter))]
public IActionResult ForgotPassword(ForgotPasswordRequest request)
{
    // Send password reset email

    return Ok("If account exists, reset link will be sent.");
}
```

## 4. Understand the logic

Request 1

IP counter: 1\
&#x20;   Email counter: 1

Allowed

Request 2–5

Both counters increase up to 5.

Allowed

Request 6

IP counter or Email counter has reached 5.

Rejected — HTTP 429

After 10 minutes

The respective counter resets when the next request arrives.

Important: This is a simplified learning example for a single application instance, not production-ready code. `Dictionary` isn't thread-safe, and this version needs synchronization and expired-entry cleanup before real use. For multiple instances, use Redis.

That's the basic implementation. No interfaces, no separate services, no Lua scripts. Just understand the filter and its three operations: check → increment → allow/reject.
