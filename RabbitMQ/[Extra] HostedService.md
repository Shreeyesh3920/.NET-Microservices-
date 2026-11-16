## **What is a Hosted Service?**

A **Hosted Service is a background task that starts automatically when our .NET application starts.**

It runs in the background without waiting for an API request.

## **How did we use it in our RabbitMQ project?**

Remember our **Order Service**?

We wanted it to continuously listen for messages from RabbitMQ, such as product updates and product deletions.

So, instead of starting consumers manually through an API, we used a Hosted Service.

**Our code:**

```
public class RabbitMQProductUpdateHostedService : IHostedService
{
    private readonly IRabbitMQConsumer _rabbitMQConsumer;

    public RabbitMQProductUpdateHostedService(
        IRabbitMQConsumer rabbitMQConsumer)
    {
        _rabbitMQConsumer = rabbitMQConsumer;
    }

    public async Task StartAsync(CancellationToken cancellationToken)
    {
        await _rabbitMQConsumer.ConnectAsync();

        await _rabbitMQConsumer.Consume();

        await _rabbitMQConsumer.ConsumeProductDeleteWithHeaders();
    }

    public async Task StopAsync(CancellationToken cancellationToken)
    {
        await _rabbitMQConsumer.DisposeAsync();
    }
}
```

**What happens here?**

1. We start our Order Service.
2. .NET automatically calls `StartAsync()`.
3. We connect to RabbitMQ using `ConnectAsync()`.
4. We register our consumers using `Consume()` and `ConsumeProductDeleteWithHeaders()`.
5. Now our consumers keep listening for RabbitMQ messages in the background.
6. When Order Service shuts down, .NET calls `StopAsync()`.

We registered it in `Program.cs`:

```
builder.Services.AddHostedService<RabbitMQProductUpdateHostedService>();
```

**That's it!**

Think of it this way:

- **Order Service:** Our application.
- **Hosted Service:** Automatically starts our background task.
- **RabbitMQ Consumer:** Listens for incoming messages.

**Interview answer:** We used `IHostedService` in our Order Service to automatically connect to RabbitMQ and register message consumers whenever the application starts. This allows our service to continuously listen for and process messages in the background without requiring an API request.
