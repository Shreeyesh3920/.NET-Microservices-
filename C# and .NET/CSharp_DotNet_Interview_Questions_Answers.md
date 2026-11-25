# C# and .NET Interview Questions & Answers

> A practical interview-preparation handbook covering fundamentals
> through advanced .NET engineering.
>
> **How to use this guide:** Try answering each question aloud before
> reading the answer. For scenario questions, explain your assumptions,
> approach, trade-offs and how you would verify the result. Examples use
> modern C# / ASP.NET Core unless stated otherwise.

## Contents

-   [1. C# fundamentals and OOP](#1-c#-fundamentals-and-oop)
-   [2. Advanced C# language concepts](#2-advanced-c#-language-concepts)
-   [3. Memory management and Garbage
    Collection](#3-memory-management-and-garbage-collection)
-   [4. Asynchronous programming, Tasks and
    Multithreading](#4-asynchronous-programming,-tasks-and-multithreading)

------------------------------------------------------------------------

## 1. C# fundamentals and OOP

### 1. What is the difference between C# and .NET?

**Answer:** C# is a programming language; .NET is the platform/runtime
and libraries used to build and run applications. Example: you write C#
code and run it on .NET.

### 2. What is CLR, and what responsibilities does it handle?

**Answer:** The Common Language Runtime executes .NET code and provides
services such as JIT compilation, garbage collection, exception
handling, and type safety. Example: CLR manages memory for your objects.

### 3. What are CTS and CLS?

**Answer:** CTS defines how types are represented across .NET languages.
CLS is a set of rules that language-facing APIs should follow to work
across .NET languages. Example: a CLS-compliant library can be consumed
by C# and VB.NET.

### 4. What is IL (Intermediate Language)?

**Answer:** IL is the CPU-independent instruction set produced by the C#
compiler. The runtime later compiles it to native machine code. Example:
a method in a DLL contains IL rather than x64 instructions.

### 5. What happens internally when you compile and execute a C# application?

**Answer:** The compiler converts C# into IL plus metadata in an
assembly. The CLR loads it, verifies/types it as needed, and
JIT-compiles methods to native code when required. Example: the first
call to a method may trigger JIT compilation.

### 6. What is the difference between value types and reference types?

**Answer:** A value variable contains its value; a reference variable
contains a reference to an object. Copying a value copies the value,
while copying a reference points both variables at the same object.
Example: changing a copied int does not change the original.

### 7. What is the difference between class, struct and record?

**Answer:** A class is a reference type; a struct is a value type; a
record provides value-oriented equality and concise data modelling.
Example: use a class for an entity with identity, a record for an
immutable DTO.

### 8. What is boxing and unboxing? What are its performance implications?

**Answer:** Boxing wraps a value type in an object; unboxing extracts
it. Boxing can allocate and add GC pressure. Example: assigning an int
to object boxes it; casting it back to int unboxes it.

### 9. What is the difference between const, readonly and static readonly?

**Answer:** const is a compile-time constant; readonly can be assigned
at declaration or in a constructor; static readonly is one value per
type. Example: use static readonly for a value calculated at startup.

### 10. What is the difference between ref, out and in?

**Answer:** ref passes a variable by reference and requires
initialization; out must be assigned by the method; in passes by
readonly reference. Example: TryParse uses out for its parsed result.

### 11. What is the difference between is and as?

**Answer:** is tests whether a value matches a type or pattern; as
attempts a reference/nullable conversion and returns null on failure.
Example: if (obj is Customer c) uses a safe type test.

### 12. What is the difference between method overloading and method overriding?

**Answer:** Overloading uses the same method name with different
parameter lists; overriding replaces a virtual base implementation in a
derived class. Example: Print(int) and Print(string) overload; Dog
overrides Animal.Speak().

### 13. What is method hiding using new?

**Answer:** The new keyword hides a base member rather than overriding
it. The selected member depends on the compile-time reference type.
Example: Base x = new Derived(); calls the base hidden method.

### 14. What is the difference between an abstract class and an interface?

**Answer:** An abstract class can hold state, constructors and shared
implementation; an interface defines a contract and can also have
default members. A class can implement many interfaces but inherit only
one class.

### 15. Can an interface contain implementations?

**Answer:** Yes. Modern C# supports default interface implementations
and static abstract members. They help evolve contracts, but should not
replace clear implementation ownership.

### 16. What are default interface methods?

**Answer:** They are interface methods with a body, used to add
functionality without forcing every existing implementer to add the
method immediately. Example: an interface can provide a default logging
helper.

### 17. What is explicit interface implementation?

**Answer:** A class implements an interface member so it is callable
through the interface reference, not directly through the class
instance. Useful when two interfaces have same-named members.

### 18. What is polymorphism? Explain compile-time and runtime polymorphism.

**Answer:** Polymorphism lets one interface represent different
behaviours. Overloading is compile-time polymorphism; virtual dispatch
is runtime polymorphism. Example: Shape.Draw() behaves differently for
Circle and Rectangle.

### 19. What are encapsulation, inheritance and abstraction?

**Answer:** Encapsulation protects internal state; inheritance
reuses/extends a base type; abstraction exposes essential behaviour
while hiding details. Example: BankAccount.Deposit() validates changes
without exposing its balance setter.

### 20. What is the difference between shallow copy and deep copy?

**Answer:** A shallow copy duplicates the outer object but shares
referenced children; a deep copy duplicates the object graph. Example:
two copied orders may still share the same mutable Address in a shallow
copy.

## 2. Advanced C# language concepts

### 21. What are delegates, and how do they work internally?

**Answer:** A delegate is a type-safe reference to one or more methods,
including their target object when applicable. Example:
Action`<string>`{=html} can refer to Console.WriteLine.

### 22. What is the difference between Func, Action and Predicate?

**Answer:** Func returns a value, Action returns void, and
Predicate`<T>`{=html} returns bool for a T. Example: Func\<int,bool\>
checks a number; Action`<int>`{=html} prints it.

### 23. What are multicast delegates?

**Answer:** A multicast delegate holds an invocation list and calls each
registered method in order. If one handler throws, later handlers
normally are not called unless exceptions are handled.

### 24. What is an event in C#? How is it different from a delegate?

**Answer:** An event wraps a delegate and restricts external code to
subscribing or unsubscribing; only the declaring type can raise it.
Example: OrderPlaced event notifies subscribers.

### 25. What is the difference between IEnumerable and IEnumerator?

**Answer:** IEnumerable represents something that can provide an
enumerator; IEnumerator performs the iteration using MoveNext and
Current. Example: foreach obtains an enumerator from a list.

### 26. What is deferred execution in LINQ?

**Answer:** A query is defined now but runs when enumerated. Example:
var q = items.Where(...); filtering happens when you iterate q, not when
q is created.

### 27. What is the difference between deferred execution and immediate execution?

**Answer:** Deferred operators build a query; immediate operators
execute it and return a result. Where is deferred; ToList, Count and
First execute.

### 28. What is the difference between IEnumerable`<T>`{=html} and IQueryable`<T>`{=html}?

**Answer:** IEnumerable runs delegates over in-process objects;
IQueryable builds expression trees that a provider can translate, often
into SQL. Example: EF Core filters can execute in the database.

### 29. What is the difference between IQueryable`<T>`{=html} and IAsyncEnumerable`<T>`{=html}?

**Answer:** IQueryable describes a provider-translatable query;
IAsyncEnumerable streams results asynchronously. EF Core can expose
async enumeration with AsAsyncEnumerable().

### 30. What is an expression tree?

**Answer:** An expression tree represents code as data, allowing
inspection or translation. Example: EF Core reads x =\> x.Price \> 100
and translates it into SQL.

### 31. How does LINQ translate expressions into SQL?

**Answer:** EF Core's provider inspects the expression tree and
translates supported operations into SQL. Unsupported expressions may
fail translation or, in limited cases, run after materialization.

### 32. What happens when you call ToList() in the middle of a LINQ query?

**Answer:** The query executes immediately and results are materialized
in memory. Later filters run in the application rather than being added
to the SQL query.

### 33. What is the difference between Select and SelectMany?

**Answer:** Select maps each item to one result; SelectMany maps each
item to a sequence and flattens the sequences. Example: flatten all
order line items.

### 34. What is the difference between First, FirstOrDefault, Single and SingleOrDefault?

**Answer:** First takes the first match; Single requires exactly one
match. The OrDefault variants return default when none exists; Single
still throws if multiple matches exist.

### 35. What are extension methods, and how does the compiler resolve them?

**Answer:** An extension method is a static method called with instance
syntax. The compiler resolves it from imported namespaces and applicable
static classes. Example: Enumerable.Where(list, predicate).

### 36. What are generics and generic constraints?

**Answer:** Generics let types and methods work with multiple types
while retaining compile-time safety. Constraints restrict allowed type
arguments, such as where T : class or new().

### 37. What is covariance and contravariance in C#?

**Answer:** Covariance allows a more derived output type; contravariance
allows a less derived input type. Example: IEnumerable`<string>`{=html}
can be used as IEnumerable`<object>`{=html}.

### 38. What is the difference between object, dynamic and var?

**Answer:** object is statically typed as object; dynamic defers member
binding to runtime; var is compile-time type inference. Example: var n =
5 is still an int.

### 39. What are nullable reference types?

**Answer:** They are compiler annotations and analysis that help detect
possible null dereferences. Example: string? means null is allowed;
string means the code expects non-null.

### 40. What are pattern matching and switch expressions?

**Answer:** Pattern matching tests shape, type, values or properties
concisely. Example: return value switch { \> 0 =\> "positive", 0 =\>
"zero", \_ =\> "negative" };

### 41. What are records and how does value-based equality work?

**Answer:** Records generate equality based on their data members rather
than object identity. Example: two Person records with the same property
values compare equal.

### 42. What are init-only setters?

**Answer:** An init accessor allows a property to be set during object
initialization or a permitted construction phase, then prevents ordinary
reassignment. Example: new User { Name = "A" }.

### 43. What is the difference between yield return and returning a collection?

**Answer:** yield return produces items lazily as the caller iterates,
avoiding building the whole collection first. Example: stream a large
file line by line.

### 44. What are iterators and how are they implemented by the compiler?

**Answer:** Iterator methods use yield return; the compiler generates a
state machine that remembers the current position between MoveNext
calls.

### 45. What is the difference between lock, Monitor and SemaphoreSlim?

**Answer:** lock is convenient syntax over Monitor for mutual exclusion;
SemaphoreSlim limits concurrent entrants and supports async WaitAsync.
Do not await inside lock.

### 46. What are Span`<T>`{=html} and Memory`<T>`{=html}?

**Answer:** They represent contiguous memory without requiring a new
array allocation. Span is stack-only and synchronous; Memory can be
stored and used across async boundaries.

### 47. What is stackalloc, and when should you avoid it?

**Answer:** stackalloc allocates a small memory region on the stack.
Avoid large or unbounded allocations because they can cause stack
overflow.

### 48. What are ref structs and why do they have restrictions?

**Answer:** Ref structs such as Span`<T>`{=html} may reference stack
memory, so the compiler prevents them escaping to unsafe lifetimes such
as heap fields or ordinary async state machines.

### 49. What are source generators in .NET?

**Answer:** Source generators inspect compilation information and emit
C# source during compilation. They can remove runtime reflection or
generate repetitive code, such as serializers.

### 50. What is the difference between a normal method and a local function?

**Answer:** A local function is declared inside another method and can
access its variables. It is useful for private helper logic scoped to
one operation.

## 3. Memory management and Garbage Collection

### 51. How does Garbage Collection work in .NET?

**Answer:** The GC finds objects reachable from roots, reclaims
unreachable objects, and may compact managed heaps. Developers normally
do not free managed objects manually.

### 52. Explain Generation 0, Generation 1 and Generation 2.

**Answer:** New short-lived objects start in Gen 0. Survivors may be
promoted to Gen 1 and Gen 2; Gen 2 collections are generally more
expensive.

### 53. What is the Large Object Heap (LOH)?

**Answer:** The LOH stores large objects, commonly arrays around 85 KB
or larger. Large allocations can increase memory pressure and
fragmentation.

### 54. What is the difference between Stack and Heap?

**Answer:** The stack stores call frames and local execution state; the
managed heap stores objects whose lifetimes are managed by GC. A
reference local may be on the stack while its object is on the heap.

### 55. How does the GC identify objects that can be collected?

**Answer:** It traces references starting from GC roots. Objects not
reachable from roots are eligible for collection.

### 56. What are GC roots?

**Answer:** Roots are references the GC treats as starting points, such
as stack locals, static fields, CPU registers and GC handles.

### 57. What is object promotion between generations?

**Answer:** An object that survives a collection can move to an older
generation. This reflects the observation that long-lived objects often
remain alive longer.

### 58. What is the difference between workstation GC and server GC?

**Answer:** Workstation GC is optimized for client responsiveness and
resource use; server GC uses multiple heaps/threads to improve
throughput on server workloads.

### 59. What is a memory leak in a garbage-collected language?

**Answer:** A leak occurs when objects remain reachable even though the
application no longer needs them. Example: an event subscription keeps
an abandoned view model alive.

### 60. How can static events cause memory leaks?

**Answer:** A long-lived publisher's event delegate references the
subscriber, keeping it alive. Unsubscribe when the subscriber's lifetime
ends or use a suitable weak-event design.

### 61. What is the purpose of IDisposable?

**Answer:** IDisposable provides deterministic release of resources such
as file handles, streams and database connections. Use using to ensure
Dispose is called.

### 62. What is the difference between Dispose() and a finalizer?

**Answer:** Dispose is explicit and deterministic; a finalizer is
nondeterministic cleanup performed by the GC for unmanaged-resource
safety. Prefer SafeHandle and Dispose patterns.

### 63. What is the using statement and how does it work internally?

**Answer:** using ensures Dispose is called even when an exception
occurs, effectively compiling to try/finally. Example: using var stream
= File.OpenRead(path).

### 64. What is IAsyncDisposable?

**Answer:** It provides asynchronous cleanup through DisposeAsync,
useful when releasing a resource requires asynchronous work. Use await
using.

### 65. What is the difference between GC.Collect() and normal automatic GC?

**Answer:** Normal GC decides when collection is beneficial. GC.Collect
forces a collection request and usually harms performance if used
routinely.

### 66. What is a pinned object?

**Answer:** A pinned object cannot be moved by the GC, often because
native code holds its address. Excessive pinning can fragment the heap.

### 67. What is LOH fragmentation?

**Answer:** It is unused gaps between large-object allocations that may
prevent efficient reuse. Reduce churn, reuse buffers and measure before
changing GC settings.

### 68. What is ArrayPool`<T>`{=html} and when would you use it?

**Answer:** ArrayPool rents and returns reusable arrays to reduce
repeated allocations. Example: rent a byte buffer for high-throughput
network processing and return it in finally.

### 69. What is ObjectPool`<T>`{=html}?

**Answer:** An object pool reuses expensive objects instead of
repeatedly creating them. Pool only when profiling shows
construction/allocation is costly and objects can be safely reset.

### 70. How would you investigate increasing memory consumption in production?

**Answer:** Capture memory dumps, compare heap snapshots, inspect GC
counters and allocation stacks, then find retained object paths. Tools
include dotnet-counters, dotnet-dump and profilers.

### 71. Can an object be alive even when there are no obvious references to it?

**Answer:** Yes. It may be retained by a static field, event, closure,
timer, cache, finalization queue or native handle. Inspect GC roots in a
dump.

### 72. What is the difference between weak references and strong references?

**Answer:** A strong reference keeps an object alive; a weak reference
does not prevent collection. Weak references suit optional caches, not
required business state.

### 73. How do finalization queues work?

**Answer:** Objects with finalizers are queued for finalization after
becoming unreachable; the finalizer thread runs them, and reclamation
may require another collection.

### 74. What is the difference between GC.SuppressFinalize() and GC.KeepAlive()?

**Answer:** SuppressFinalize prevents a finalizer from running after
deterministic cleanup. KeepAlive ensures an object remains alive until a
specified point in execution.

### 75. How would excessive object allocation affect application throughput?

**Answer:** It increases GC frequency and CPU spent collecting,
potentially increasing latency. Reduce temporary allocations, reuse
buffers and stream data where appropriate.

### 76. How can boxing increase GC pressure?

**Answer:** Boxing value types creates object allocations. Prefer
generic collections and typed APIs instead of object-based APIs where
possible.

### 77. How would you identify LOH allocation issues?

**Answer:** Use allocation profiling and GC counters/dumps to find large
arrays or strings. Reuse buffers or stream content instead of
constructing huge temporary objects.

### 78. What is the difference between latency mode and throughput-oriented GC?

**Answer:** GC latency modes trade collection timing and pause behaviour
against throughput. Choose only after measuring the workload and
understanding memory limits.

### 79. How does Span`<T>`{=html} help reduce allocations?

**Answer:** Span slices existing memory without copying it. Example:
parse part of a string or byte buffer without allocating a
substring/array.

### 80. How would you optimize an allocation-heavy API without sacrificing maintainability?

**Answer:** Profile first; then remove avoidable copies, stream large
results, use pooling selectively and keep APIs clear. Verify
improvements with benchmarks and production metrics.

## 4. Asynchronous programming, Tasks and Multithreading

### 81. What is the difference between synchronous and asynchronous programming?

**Answer:** Synchronous work blocks the caller until completion;
asynchronous work allows the caller to await completion without holding
a thread idle during I/O. Example: await an HTTP request.

### 82. What is the difference between a Task and a Thread?

**Answer:** A Thread is an OS execution thread; Task represents an
operation or promise of completion and may run on a ThreadPool thread or
complete through I/O callbacks.

### 83. Does async/await create a new thread?

**Answer:** No. async/await is a compiler-supported way to compose
asynchronous operations. I/O awaits usually do not need a dedicated
thread while waiting.

### 84. How does the compiler transform an async method?

**Answer:** The compiler generates a state machine that stores execution
state across await points and resumes when awaited operations complete.

### 85. What is a Task state machine?

**Answer:** It is generated code that tracks the async method's current
state, locals and continuation so execution can resume after an await.

### 86. What is the difference between Task, Task`<T>`{=html} and ValueTask`<T>`{=html}?

**Answer:** Task represents completion without a result;
Task`<T>`{=html} returns a result; ValueTask`<T>`{=html} can avoid
allocation when results often complete synchronously, but is more
complex.

### 87. When should you use ValueTask instead of Task?

**Answer:** Use it only when profiling shows Task allocation is
significant and the operation frequently completes synchronously.
Otherwise Task is simpler and safer.

### 88. What happens when you call .Result or .Wait() on an asynchronous operation?

**Answer:** The calling thread blocks. This can cause thread-pool
starvation and, in some contexts, deadlocks. Prefer await.

### 89. What is sync-over-async?

**Answer:** It means blocking on asynchronous work using Result, Wait or
GetAwaiter().GetResult(). It can waste threads and cause starvation or
deadlocks.

### 90. What is a deadlock in asynchronous programming?

**Answer:** A deadlock occurs when continuations cannot run because
required resources or contexts are blocked. Blocking on async work in a
captured synchronization context is a classic case.

### 91. What is ConfigureAwait(false)?

**Answer:** It tells an await not to require resuming on the captured
synchronization context. It is commonly useful in reusable library code;
ASP.NET Core normally has no classic request SynchronizationContext.

### 92. What is SynchronizationContext?

**Answer:** It represents a context for scheduling continuations, such
as a UI thread. Await may capture it and resume there unless configured
otherwise.

### 93. What is TaskScheduler?

**Answer:** It determines where Tasks are scheduled. The default
scheduler generally uses the ThreadPool.

### 94. What is the difference between Task.Run() and directly awaiting an asynchronous method?

**Answer:** Task.Run queues CPU-bound work to the ThreadPool. Do not
wrap naturally asynchronous I/O in Task.Run in server code; just await
it.

### 95. What is the difference between CPU-bound and I/O-bound operations?

**Answer:** CPU-bound work spends time computing; I/O-bound work waits
for disk, network or database operations. Parallel CPU work may use
threads; async I/O avoids blocking while waiting.

### 96. What is the ThreadPool?

**Answer:** It is a pool of reusable worker threads used by Tasks,
timers and many runtime operations. It reduces the cost of creating
threads repeatedly.

### 97. What is ThreadPool starvation?

**Answer:** It occurs when available worker threads are blocked or busy,
delaying queued work. Common causes include sync-over-async and long
blocking operations.

### 98. What is the difference between Task.WhenAll() and Task.WhenAny()?

**Answer:** WhenAll completes when every supplied task completes;
WhenAny completes when the first task completes. Example: run
independent calls concurrently with WhenAll.

### 99. How does Task.WhenAll() handle multiple exceptions?

**Answer:** The returned task faults if any task faults; its Exception
contains aggregated exceptions. Await typically throws one exception,
while inspecting task.Exception reveals the aggregate.

### 100. What is CancellationToken?

**Answer:** It is a cooperative signal that a caller requests
cancellation. Pass it through APIs and check/observe it; it does not
forcibly terminate code.

### 101. How does cooperative cancellation work?

**Answer:** The caller cancels a CancellationTokenSource; operations
receive the token and stop at safe points or pass it to cancellable
APIs.

### 102. What happens if a method ignores a cancellation token?

**Answer:** That method continues running until it finishes or
independently stops. Cancellation only works when the operation observes
the token.

### 103. What is a race condition?

**Answer:** A race occurs when outcome depends on timing of concurrent
operations. Example: two requests read stock=1 and both decrement it
without atomic protection.

### 104. What is a thread-safe application?

**Answer:** Its shared operations remain correct when accessed
concurrently. Use immutable data, synchronization, concurrent
collections or atomic database operations.

### 105. What is the difference between lock and SemaphoreSlim?

**Answer:** lock provides synchronous exclusive access; SemaphoreSlim
can limit concurrency and supports asynchronous waiting. Use WaitAsync
in async methods.

### 106. What is a deadlock in multithreaded programming?

**Answer:** Two or more threads wait forever for resources held by each
other. Avoid inconsistent lock ordering and keep critical sections
short.

### 107. What is the difference between parallelism and concurrency?

**Answer:** Concurrency manages overlapping work; parallelism executes
work at the same time on multiple cores. Async I/O can be concurrent
without parallel CPU execution.

### 108. What is Parallel.ForEachAsync()?

**Answer:** It processes an enumerable with bounded parallel
asynchronous delegates. Set MaxDegreeOfParallelism to protect downstream
resources.

### 109. What is a producer-consumer pattern?

**Answer:** Producers create work items and consumers process them
through a queue/channel. It decouples production speed from processing
capacity.

### 110. How would you limit the number of concurrent API calls?

**Answer:** Use SemaphoreSlim or a bounded Channel, acquire before
calling and release in finally. Also configure downstream timeouts and
rate limits.

### 111. Your API suddenly experiences high CPU usage and increasing request latency. How would you investigate ThreadPool starvation?

**Answer:** Check ThreadPool counters, queue length, thread count and
blocked stacks. Search for sync-over-async, locks and blocking I/O;
capture traces before changing thread settings.

### 112. You have 10 independent API calls. How would you execute them concurrently without creating unnecessary threads?

**Answer:** Start all asynchronous calls and await Task.WhenAll. Use
HttpClient and cancellation tokens; limit concurrency if the downstream
service has capacity constraints.

### 113. One task fails inside Task.WhenAll(). What happens to the remaining tasks?

**Answer:** WhenAll does not automatically cancel siblings. Other tasks
continue unless you use a linked CancellationTokenSource and cancel them
explicitly.

### 114. Your application processes 10,000 records. How would you process them concurrently while limiting database connections?

**Answer:** Use bounded batches or a bounded Channel with a small worker
count. Reuse scoped DbContexts per worker and tune concurrency to
database capacity.

### 115. Two threads simultaneously update the same account balance. How would you prevent inconsistent data?

**Answer:** Use a database transaction with a conditional update or
optimistic concurrency token. An in-process lock is insufficient across
multiple app instances.

### 116. Your API receives a cancellation request, but a downstream HTTP request continues running. How would you fix it?

**Answer:** Pass HttpContext.RequestAborted or the operation token to
HttpClient.SendAsync and downstream methods; ensure no layer drops the
token.

### 117. How would you implement a background worker that processes messages without blocking incoming HTTP requests?

**Answer:** Use a hosted BackgroundService with a bounded Channel or
broker consumer. Keep work off the request path and create a DI scope
per unit of work.

### 118. How would you troubleshoot a deadlock that occurs only occasionally in production?

**Answer:** Capture dumps/traces during the incident, inspect waiting
threads and lock ownership, then map stacks to code. Add correlation and
lock-duration telemetry.

### 119. When would you use Channel`<T>`{=html} instead of ConcurrentQueue`<T>`{=html}?

**Answer:** Channel provides async producer/consumer coordination,
completion and bounded capacity/backpressure. ConcurrentQueue is a
thread-safe queue but does not provide async waiting.

### 120. How would you implement a thread-safe singleton without introducing unnecessary locking?

**Answer:** Use a static readonly instance or a nested static holder;
CLR type initialization is thread-safe. Prefer DI singleton registration
in application code.

## 5. ASP.NET Core and Web API internals

### 121. Explain the complete ASP.NET Core request lifecycle.

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 122. What is middleware?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 123. How does middleware execute internally?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 124. What is the difference between Use, Run and Map?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 125. What is middleware short-circuiting?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 126. Why does middleware ordering matter?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 127. What happens if exception-handling middleware is registered after authentication middleware?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 128. What is dependency injection?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

### 129. Explain Transient, Scoped and Singleton lifetimes.

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

### 130. What happens when a Singleton depends on a Scoped service?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 131. What is a captive dependency?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

### 132. How does the built-in .NET DI container resolve dependencies?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 133. What happens if multiple implementations of the same interface are registered?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 134. What is IServiceScope?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 135. How do you resolve scoped services inside a BackgroundService?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 136. What is IHttpClientFactory?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

### 137. Why is creating a new HttpClient for every request problematic?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 138. What is socket exhaustion?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

### 139. How does IHttpClientFactory manage HttpMessageHandler lifetimes?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

### 140. What is the Options pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 141. Difference between IOptions, IOptionsSnapshot and IOptionsMonitor.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 142. How does configuration loading work in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 143. What is the difference between Minimal APIs and Controllers?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 144. What is model binding?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 145. What is model validation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 146. What is the difference between filters and middleware?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 147. Explain authorization filters, action filters and exception filters.

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 148. What is content negotiation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 149. What is the difference between IActionResult, ActionResult`<T>`{=html} and IResult?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 150. What is API versioning?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 151. How do you implement global exception handling?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 152. What is Problem Details in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 153. What is endpoint routing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 154. How does attribute routing differ from conventional routing?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 155. What is Kestrel?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 156. What is a reverse proxy?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 157. How does HTTPS termination work?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 158. What is CORS, and why is it enforced by browsers?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 159. What is rate limiting in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 160. How would you implement IP-based and user-based rate limiting?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 161. An API works locally but returns HTTP 500 after deployment. How would you investigate it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 162. Your API receives 5,000 simultaneous requests. How would you protect the database from being overwhelmed?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 163. Your middleware is not executing. How would you debug the request pipeline?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 164. Your Singleton service accidentally depends on a Scoped DbContext. What can go wrong?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 165. How would you implement global exception handling without exposing internal stack traces to clients?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 166. Your API is responding slowly even though the database query is fast. What would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 167. How would you implement API versioning without breaking existing clients?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 168. How would you secure an API that is consumed by multiple independent applications?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 169. How would you implement request correlation across multiple microservices?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 170. How would you prevent a slow downstream service from exhausting your API's available resources?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 6. Entity Framework Core and database interaction

### 171. What is Entity Framework Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 172. What is the difference between EF Core and Dapper?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 173. What is DbContext?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 174. Why is DbContext not thread-safe?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 175. What is Change Tracking?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 176. Explain the entity states: Added, Modified, Deleted, Unchanged and Detached.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 177. What is AsNoTracking()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 178. What is AsNoTrackingWithIdentityResolution()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 179. What is the difference between eager loading, lazy loading and explicit loading?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 180. What is the N+1 query problem?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 181. What is the difference between Include() and ThenInclude()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 182. What is the difference between Find() and FirstOrDefault()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 183. What is the difference between SaveChanges() and SaveChangesAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 184. How does EF Core manage database transactions?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 185. What is optimistic concurrency?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 186. What is a concurrency token?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 187. What is SQL Server rowversion?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 188. What is the difference between IQueryable and IEnumerable in EF Core?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 189. What is client-side evaluation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 190. What is the difference between single-query and split-query execution?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 191. What is Cartesian explosion in EF Core?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 192. What are compiled queries?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 193. What is DbContext pooling?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 194. What is connection pooling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 195. What are global query filters?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 196. How would you implement soft deletion using global query filters?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 197. What is the difference between ExecuteUpdateAsync() and loading an entity followed by SaveChangesAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 198. What happens to the ChangeTracker when you use ExecuteDeleteAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 199. What are EF Core migrations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 200. How would you apply database migrations safely in production?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 201. A query is taking 8 seconds to execute. How would you identify whether the problem is EF Core, SQL Server or network latency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 202. Your API loads 10,000 records but only displays 20. How would you optimize this?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 203. You have multiple Include() operations and the query generates huge amounts of duplicated data. How would you solve it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 204. Two users update the same record simultaneously. How would you handle concurrency conflicts?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 205. How would you execute multiple database operations atomically?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 206. You have a very complex LINQ query. How would you inspect the SQL generated by EF Core?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 207. Your application executes the same query thousands of times. How would you optimize query compilation and execution?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 208. When would you choose raw SQL over LINQ?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 209. When would you choose Dapper instead of EF Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 210. How would you implement bulk updates for millions of records?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 211. How would you prevent accidental loading of large object graphs?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 212. Why should you generally avoid sharing one DbContext between multiple concurrent operations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 213. How would you implement multi-tenancy using EF Core global query filters?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 214. How would you safely introduce a new non-nullable column into a production database with millions of existing records?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 215. How would you avoid database connection pool exhaustion?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 7. SQL Server and query optimization

### 216. What is the difference between clustered and non-clustered indexes?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 217. What is a composite index?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 218. What is index fragmentation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 219. What is an execution plan?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 220. What is a table scan versus an index seek?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 221. What is a covering index?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 222. What is the difference between a primary key and a unique key?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 223. What is a foreign key?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 224. What is normalization and denormalization?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 225. Explain database normalization forms.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 226. What are ACID properties?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 227. What is transaction isolation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 228. Explain Read Uncommitted, Read Committed, Repeatable Read and Serializable.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 229. What is Snapshot Isolation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 230. What is a dirty read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 231. What is a non-repeatable read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 232. What is a phantom read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 233. What is a database deadlock?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 234. What is the difference between blocking and deadlocking?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 235. What is a CTE?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 236. What is the difference between CTE, temporary table and table variable?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 237. What are window functions?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 238. Explain ROW_NUMBER(), RANK() and DENSE_RANK().

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 239. What is the difference between UNION and UNION ALL?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 240. What is parameter sniffing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 241. What is the difference between stored procedures and parameterized queries?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 242. What is a SARGable query?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 243. Why can applying functions to indexed columns affect query performance?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 244. How would you optimize a query involving millions of rows?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 245. How would you troubleshoot a SQL Server deadlock?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 8. Microservices and distributed systems

### 246. What is a microservice architecture?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 247. What is the difference between monolithic and microservice architecture?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 248. What are the advantages and disadvantages of microservices?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 249. What is an API Gateway?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 250. What is a reverse proxy?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 251. What is the difference between an API Gateway and a Load Balancer?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 252. What is service discovery?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 253. What is inter-service communication?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 254. What is synchronous versus asynchronous communication?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 255. What is the difference between REST and gRPC?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 256. What is eventual consistency?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 257. What is distributed caching?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 258. What is cache invalidation?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 259. What is the Cache-Aside pattern?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 260. What are Write-Through and Write-Behind caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 261. What is the Circuit Breaker pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 262. What is the Retry pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 263. What is exponential backoff?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 264. Why is jitter important in retry policies?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 265. What is the Bulkhead pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 266. What is the Timeout pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 267. What is the difference between Retry and Circuit Breaker?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 268. What is the Saga pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 269. What is the difference between orchestration and choreography?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 270. What is the Outbox pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 271. What is the Inbox pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 272. What is idempotency?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 273. How do you prevent duplicate message processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 274. What is distributed tracing?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 275. What is a correlation ID?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 276. What is a distributed transaction?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 277. Why is two-phase commit problematic in distributed systems?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 278. What is CQRS?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 279. What is Event Sourcing?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 280. What is the Strangler Fig pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 281. Your Order service successfully creates an order, but the Payment service goes down before payment processing. How would you handle this?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 282. A message is delivered twice by RabbitMQ. How would you prevent duplicate order processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 283. Your Product service is unavailable. Should the Order service retry indefinitely?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 284. Redis becomes unavailable. Should your API fail or fall back to the database?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 285. Two microservices update different databases as part of one business operation. How would you maintain consistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 286. Your service processes a message successfully but crashes before acknowledging it. What happens next?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 287. How would you implement an idempotent payment API?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 288. How would you handle a poison message that continuously fails processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 289. Your API Gateway is working, but one downstream service becomes extremely slow. How would you prevent cascading failures?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 290. How would you implement distributed rate limiting across five instances of the same API?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 291. How would you trace one request across an API Gateway, Order service, Product service and database?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 292. When would you use RabbitMQ instead of Kafka?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 293. What is the difference between RabbitMQ Direct, Topic, Fanout and Headers exchanges?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 294. What happens if a consumer dies while processing a message?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 295. How would you ensure reliable event publishing when a database transaction commits?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 296. How would you decide whether a business operation should use synchronous HTTP communication or asynchronous messaging?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 297. What happens when you introduce a breaking change in an event contract?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 298. How would you scale a microservice horizontally without causing duplicate processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 299. What is the difference between at-most-once, at-least-once and effectively-once processing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 300. How would you design a distributed order-processing system that remains operational during partial failures?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 9. Redis and distributed caching

### 301. What is Redis, and how does it store data?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 302. Why is Redis called an in-memory data store?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 303. Is Redis a distributed cache or centralized cache?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 304. What is the difference between local memory caching and distributed caching?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

### 305. What is IDistributedCache?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 306. How does IDistributedCache work with Redis?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 307. What is TTL (Time To Live)?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 308. What happens when a Redis key expires?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 309. What is cache eviction?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 310. Explain Redis eviction policies.

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 311. What is cache stampede?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 312. What is cache penetration?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 313. What is cache avalanche?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 314. What is cache warming?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 315. What is cache invalidation?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 316. How would you invalidate cached data when a product's price changes?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 317. What is cache-aside?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 318. What is Write-Through caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 319. What is Write-Behind caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 320. What is the difference between Redis Pub/Sub and Redis Streams?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 321. What are Redis persistence mechanisms?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 322. What is the difference between RDB and AOF?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 323. What happens when Redis runs out of memory?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 324. How would you implement distributed locking using Redis?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 325. How would you prevent multiple instances from refreshing the same expired cache simultaneously?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 326. Your application has five API instances sharing Redis. One instance updates a product price. How do you ensure the remaining instances don't serve stale data?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 327. Redis is operational, but response times suddenly increase. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 328. Your cache expires at the same time for thousands of users. How would you avoid a database traffic spike?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 329. A cache update succeeds, but the database update fails. How would you prevent inconsistent data?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 330. How would you implement a distributed cache with a fallback when Redis is unavailable?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 10. Security and authentication

### 331. What is authentication versus authorization?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 332. What is JWT?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 333. What are the three parts of a JWT?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 334. How does JWT authentication work in ASP.NET Core?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 335. What is the difference between access tokens and refresh tokens?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 336. Where should refresh tokens be stored?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 337. What is OAuth 2.0?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 338. What is OpenID Connect?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 339. What is the difference between OAuth 2.0 and OpenID Connect?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 340. What is the difference between role-based and policy-based authorization?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 341. What are claims and claims-based authorization?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 342. How does ASP.NET Core authentication middleware work?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

### 343. What is CSRF?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 344. What is XSS?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 345. What is SQL injection?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 346. How does parameterized SQL prevent SQL injection?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 347. What is CORS?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 348. What is the difference between HTTPS and HTTP?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 349. How would you securely store passwords?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 350. What is the principle of least privilege?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 351. Your JWT has not expired, but a user has been disabled. How would you immediately prevent further access?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 352. How would you implement refresh-token rotation and token revocation?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 353. How would you secure communication between internal microservices?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 354. How would you prevent replay attacks?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 355. How would you secure secrets in Docker containers and Kubernetes deployments?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 356. How would you implement authorization for a resource owned by a particular user?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 357. What are the security risks of storing JWTs in browser local storage?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 358. How would you protect an API against brute-force login attempts?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 11. SOLID principles and design patterns

### 359. Explain all five SOLID principles with real-world examples.

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 360. What is Dependency Inversion?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 361. What is the difference between Dependency Injection and Inversion of Control?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

### 362. What is the Repository pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 363. What is the Unit of Work pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 364. What is the Factory pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 365. What is the Abstract Factory pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 366. What is the Strategy pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 367. What is the Decorator pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 368. What is the Adapter pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 369. What is the Observer pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 370. What is the Mediator pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 371. What is the Singleton pattern?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

### 372. Why can Singleton become an anti-pattern?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 373. What is the Template Method pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 374. What is the Specification pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 375. What is the Chain of Responsibility pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 376. What is the difference between composition and inheritance?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 377. When should you prefer composition over inheritance?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 378. What is Clean Architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 379. What is Onion Architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 380. What is the difference between Clean Architecture and N-tier architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 381. Would you always use the Repository pattern with EF Core? Why or why not?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 382. How would you design a system that supports multiple payment providers without changing existing business logic?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 383. How would you implement a notification system supporting Email, SMS and Push notifications?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 384. How would you prevent a service class from becoming a God Object?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 385. How would you identify overengineering in an application?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 386. How would you refactor tightly coupled legacy code without breaking existing functionality?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 387. How would you design a plugin-based architecture in .NET?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 388. How would you introduce a new feature following Open/Closed Principle?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 12. Docker, Kubernetes and deployment

### 389. What is Docker?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 390. What is the difference between a Docker image and a container?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 391. What is a Dockerfile?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 392. What is Docker Compose?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 393. What is a Docker network?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 394. How do containers communicate with each other?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 395. What is the difference between a Docker volume and a bind mount?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 396. What is a multi-stage Docker build?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 397. Why should you avoid running containers as root?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 398. How do environment variables work inside containers?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 399. What happens when a container exits?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 400. What is a container health check?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 401. What is Kubernetes?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 402. What is a Pod?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 403. What is a Deployment?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 404. What is a Service in Kubernetes?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 405. What is the difference between ClusterIP, NodePort and LoadBalancer?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 406. What is an Ingress?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 407. What is a ConfigMap?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 408. What is a Secret?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

### 409. What are Liveness, Readiness and Startup Probes?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 410. What is horizontal pod autoscaling?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 411. How does Kubernetes perform rolling deployments?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

### 412. What is a CrashLoopBackOff?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 413. How would you troubleshoot a pod that keeps restarting?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 414. How would you perform a zero-downtime deployment?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 415. How would you handle database schema changes during rolling deployments?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

## 13. Unit testing and software quality

### 416. What is unit testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 417. What is integration testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 418. What is the difference between unit and integration testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 419. What is mocking?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 420. What is the difference between Mock, Stub and Fake?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 421. What is Moq?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 422. What is dependency isolation in unit testing?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

### 423. What is Test-Driven Development?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 424. What is the Arrange-Act-Assert pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

### 425. What is code coverage?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 426. What is the difference between a unit test and a component test?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 427. How would you test a service that depends on Redis?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 428. How would you test an API endpoint requiring JWT authentication?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 429. How would you test asynchronous methods?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 430. How would you test code that depends on DateTime or the current system time?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 431. How would you test a retry mechanism?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 432. How would you test a method that publishes RabbitMQ messages?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 433. What is a flaky test?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

### 434. How do you test concurrent operations?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 435. What is contract testing in microservices?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

## 14. API design and performance engineering

### 436. What are REST architectural constraints?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 437. What is idempotency in HTTP?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 438. Explain GET, POST, PUT, PATCH and DELETE.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 439. What is the difference between PUT and PATCH?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 440. What is the difference between 401 and 403?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 441. What is the difference between 400, 404, 409 and 422?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 442. What is pagination?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 443. What is the difference between offset pagination and cursor pagination?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 444. What is response compression?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 445. What is output caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 446. What is response caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 447. What is the difference between response caching and output caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 448. What is connection pooling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 449. What is asynchronous streaming?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 450. What is backpressure?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 451. What is request throttling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 452. How would you improve API throughput?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 453. How would you reduce API latency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 454. How would you investigate high memory consumption?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 455. How would you investigate high CPU consumption?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 456. How would you optimize a slow API without changing its business logic?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 457. What is BenchmarkDotNet?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 458. What is profiling?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 459. What is observability?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

### 460. Explain logs, metrics and distributed traces.

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

## 15. Advanced .NET runtime and modern platform concepts

### 461. What is the difference between .NET Framework, .NET Core and modern .NET?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 462. What is the difference between JIT and AOT compilation?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 463. What is Native AOT?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 464. What is ReadyToRun compilation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 465. What is tiered compilation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 466. What is Dynamic PGO?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 467. What is reflection?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 468. What are the performance implications of reflection?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 469. What are attributes in C#?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 470. How does attribute-based programming work internally?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 471. What is assembly loading?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 472. What is the difference between an assembly and a namespace?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 473. What is an AssemblyLoadContext?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 474. What is the Global Assembly Cache?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

### 475. What is NuGet package management?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 476. What is the difference between runtime and SDK?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 477. What are nullable reference types and how does the compiler use nullable annotations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 478. What is trimming in .NET?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

### 479. What are analyzers and code generators?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

### 480. What is the difference between source generation and runtime reflection?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

## 16. Advanced coding and problem-solving questions

### 481. Implement a thread-safe Singleton.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 482. Implement a custom middleware for request logging.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 483. Implement a custom exception-handling middleware.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 484. Implement a generic repository.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 485. Implement a custom extension method for IEnumerable`<T>`{=html}.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 486. Implement your own Where() extension method.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 487. Implement a simple LRU cache.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 488. Implement a thread-safe in-memory cache.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 489. Implement a rate limiter using the Token Bucket algorithm.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 490. Implement a fixed-window rate limiter.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 491. Implement a producer-consumer system using Channel`<T>`{=html}.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 492. Implement a retry mechanism with exponential backoff.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 493. Implement a circuit breaker.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 494. Implement an idempotency mechanism for a payment API.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 495. Implement pagination using EF Core.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 496. Implement a background worker using BackgroundService.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 497. Implement parallel processing with a maximum concurrency limit.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 498. Implement a distributed lock using Redis.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 499. Implement a custom LINQ aggregation method.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

### 500. Implement a service that processes 100,000 records without loading everything into memory.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

## 17. Extremely challenging production scenarios

### 501. Your API response time increases from 200 ms to 5 seconds after deployment. CPU usage is normal, but requests are piling up. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 502. Your SQL query executes in 100 ms in SQL Server Management Studio but takes 3 seconds through EF Core. What could explain the difference?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

### 503. Your application is experiencing high GC activity. How would you determine which part of the code creates excessive allocations?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 504. Two requests simultaneously purchase the last available product. How do you prevent overselling?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 505. A background worker and an API endpoint modify the same record simultaneously. How would you protect data consistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 506. Your application works perfectly with one instance but produces duplicate records when deployed with five instances. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 507. Your Order service writes to SQL Server and then publishes an event to RabbitMQ. The database operation succeeds, but RabbitMQ is unavailable. How would you guarantee eventual event delivery?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 508. Your consumer receives the same message multiple times. How would you guarantee that the business operation happens only once effectively?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 509. Your Redis cache and SQL Server contain different versions of a product. How would you resolve the inconsistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 510. Your microservice is dependent on three external services. One is slow, one is unavailable and one is returning intermittent errors. How would you design resilience?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 511. A production database has millions of records, and a frequently executed query suddenly becomes slow. What would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 512. A migration needs to modify a heavily used table without causing significant downtime. How would you approach it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 513. A transaction updates three tables. The first two succeed, but the third fails. How would you ensure atomicity?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 514. Your database connection pool is exhausted even though the number of incoming requests hasn't increased. What could be wrong?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 515. You are asked to convert a monolithic application into microservices. How would you identify the service boundaries?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 516. Your microservices share the same database. What problems could arise, and how would you migrate away from this design?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 517. How would you design a notification system that processes millions of notifications per day?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 518. How would you design a document management system with upload, storage, search, versioning and access control?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 519. How would you design an audit logging mechanism that records every important business operation?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

### 520. How would you design a system that supports 100,000 concurrent users?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

------------------------------------------------------------------------

## Interview answer framework

For advanced questions, structure your spoken answer like this:

1.  **Definition:** Explain the concept in plain language.
2.  **Mechanics:** Explain what happens internally.
3.  **Use case:** Give a realistic example from an ASP.NET Core
    application.
4.  **Trade-offs:** Explain when the approach is unsuitable.
5.  **Production angle:** Mention failure handling, observability,
    security or performance where relevant.

### Example: How would you prevent duplicate message processing?

Use an idempotent consumer. Give each message or business operation a
stable identifier, store the processed identifier with a unique
constraint, and perform the business update plus processed-marker write
in one database transaction. If the broker redelivers the message, the
unique constraint shows it has already been processed. Do not rely only
on an in-memory HashSet because it is lost on restart and is not shared
across instances.

### Example: Does async/await create a new thread?

No. `async` and `await` let the compiler build a state machine. For
I/O-bound work, the thread can return to the pool while the operation
waits. When the I/O completes, the continuation is scheduled. `Task.Run`
is different: it queues work to the ThreadPool and is generally for
CPU-bound work, not wrapping ordinary asynchronous HTTP or database
calls.

### Example: What is the difference between `IEnumerable<T>` and `IQueryable<T>`?

`IEnumerable<T>` represents iteration over objects and uses .NET
delegates. `IQueryable<T>` represents a query expression that a provider
can translate, such as EF Core translating a filter into SQL. Calling
`ToList()` materializes the results; subsequent filtering happens in
memory.

> **Version note:** .NET and C# evolve. Check the target framework and
> language version used by the job before relying on version-specific
> behaviour. Verify runtime details against Microsoft Learn, especially
> for GC, async, ASP.NET Core hosting and EF Core.
