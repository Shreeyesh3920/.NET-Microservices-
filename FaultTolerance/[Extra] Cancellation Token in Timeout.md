CancellationToken is a mechanism in .NET that allows one part of an application to notify another part that an ongoing operation should stop.

CancellationTokenSource – Cancellation signal generate karta hai.

CancellationToken – Signal receive karta hai.

Cancel() – Cancellation signal bhejta hai.

## 1. Without Polly: Simple CancellationToken example

Imagine ek function 10 numbers print kar raha hai. Har number ke baad 5 seconds wait karta hai.

Lekin 12 seconds baad hum usko stop karna chahte hain.

```
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var cts = new CancellationTokenSource();

        // Calling another function and passing the token
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

### Execution samajh:

```
Main()
  |
  |-- Creates CancellationTokenSource
  |
  |-- Calls PrintNumbersAsync(token)
  |       |
  |       |-- Prints 1
  |       |-- Waits 5 seconds
  |       |-- Prints 2
  |       |-- Waits 5 seconds
  |
  |-- 12 seconds complete
  |
  |-- cts.Cancel()
  |
  v
PrintNumbersAsync receives cancellation
  |
  |-- OperationCanceledException
  |
  v
Processing stopped!
```

Yahan `Main()` cancellation signal bhej raha hai aur `PrintNumbersAsync()` us signal ko receive karke processing stop kar raha hai.

## 2. Ab tera Polly wala doubt ki hum bass Cancellation Token bhej rahe hai— ye important hai!

Tu bilkul sahi keh raha hai.

Polly Timeout mein hum normally sirf `CancellationToken` pass karte hain:

```
var response = await httpClient.GetAsync(
    "/api/products",
    cancellationToken
);
```

Toh phir `CancellationTokenSource` aur `Cancel()` kaun handle karta hai?

Answer: Polly internally handle karta hai!

Jab hum ye configure karte hain:

```
pipeline.AddTimeout(TimeSpan.FromSeconds(10));
```

Polly internally cancellation mechanism manage karta hai.

- Polly internally `CancellationTokenSource` create/manage karta hai.
- Polly 10 seconds ka timeout monitor karta hai.
- 10 seconds complete hone par internally cancellation request bhejta hai.
- HTTP operation ko cancellation signal milta hai.
- Polly timeout ko `TimeoutRejectedException` ke through report karta hai.

Polly internally

CancellationTokenSource

Managed by Polly

10-second timeout expires

Cancel() internally

CancellationToken signals HTTP operation

### Toh difference kya hai?

| Without Polly                       | With Polly Timeout                                         |
| ----------------------------------- | ---------------------------------------------------------- |
| We create `CancellationTokenSource` | Polly manages its own timeout cancellation                 |
| We call `Cancel()`                  | Polly calls cancellation internally                        |
| We decide when to cancel            | Polly decides based on configured timeout                  |
| We pass `CancellationToken`         | HTTP resilience pipeline propagates its cancellation token |

Ek important distinction: Agar user/client ka apna cancellation token bhi hai, toh woh alag cancellation source hai. Polly us caller cancellation ko bhi respect karta hai.

Bas bhidu! Ab yaad rakh: Normal cancellation mein hum `Cancel()` call karte hain. Polly Timeout mein Polly khud internally cancellation trigger karta hai. 😎
