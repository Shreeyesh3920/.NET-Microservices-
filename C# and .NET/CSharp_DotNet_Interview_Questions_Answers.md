# C# and .NET Interview Questions & Answers — Expanded Edition

> **520 questions with interview-oriented answers.** The question wording and topic order are retained from the original 520-question set. Answers are expanded with explanation, practical reasoning and common trade-offs.
>
> **How to use this handbook:** Read the answer, close the file, and explain it aloud in your own words. For scenario questions, structure your response as: clarify requirements → investigate → propose a safe solution → discuss trade-offs → verify with tests/metrics.
>
> Examples assume modern C# and ASP.NET Core unless a question explicitly concerns .NET Framework. Framework-specific APIs can differ by version; check the target framework before copying code into a project.

## Contents

- [1. C# fundamentals and OOP](#1-c-fundamentals-and-oop)
- [2. Advanced C# language concepts](#2-advanced-c-language-concepts)
- [3. Memory management and Garbage Collection](#3-memory-management-and-garbage-collection)
- [4. Asynchronous programming, Tasks and Multithreading](#4-asynchronous-programming-tasks-and-multithreading)
- [5. ASP.NET Core and Web API internals](#5-aspnet-core-and-web-api-internals)
- [6. Entity Framework Core and data access](#6-entity-framework-core-and-data-access)
- [7. SQL Server and database engineering](#7-sql-server-and-database-engineering)
- [8. Microservices and distributed systems](#8-microservices-and-distributed-systems)
- [9. Redis and caching](#9-redis-and-caching)
- [10. Messaging and event-driven architecture](#10-messaging-and-event-driven-architecture)
- [11. SOLID, design patterns and architecture](#11-solid-design-patterns-and-architecture)
- [12. Docker, Kubernetes and deployment](#12-docker-kubernetes-and-deployment)
- [13. Unit testing and software quality](#13-unit-testing-and-software-quality)
- [14. API design and performance engineering](#14-api-design-and-performance-engineering)
- [15. Advanced .NET runtime and modern platform concepts](#15-advanced-net-runtime-and-modern-platform-concepts)
- [16. Advanced coding and problem-solving questions](#16-advanced-coding-and-problem-solving-questions)
- [17. Extremely challenging production scenarios](#17-extremely-challenging-production-scenarios)

---


## 1. C# fundamentals and OOP

### 1. What is the difference between C# and .NET?

**Answer:** C# is a programming language; .NET is the platform/runtime
and libraries used to build and run applications. Example: you write C#
code and run it on .NET.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 2. What is CLR, and what responsibilities does it handle?

**Answer:** The Common Language Runtime executes .NET code and provides
services such as JIT compilation, garbage collection, exception
handling, and type safety. Example: CLR manages memory for your objects.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 3. What are CTS and CLS?

**Answer:** CTS defines how types are represented across .NET languages.
CLS is a set of rules that language-facing APIs should follow to work
across .NET languages. Example: a CLS-compliant library can be consumed
by C# and VB.NET.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 4. What is IL (Intermediate Language)?

**Answer:** IL is the CPU-independent instruction set produced by the C#
compiler. The runtime later compiles it to native machine code. Example:
a method in a DLL contains IL rather than x64 instructions.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 5. What happens internally when you compile and execute a C# application?

**Answer:** The compiler converts C# into IL plus metadata in an
assembly. The CLR loads it, verifies/types it as needed, and
JIT-compiles methods to native code when required. Example: the first
call to a method may trigger JIT compilation.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 6. What is the difference between value types and reference types?

**Answer:** A value variable contains its value; a reference variable
contains a reference to an object. Copying a value copies the value,
while copying a reference points both variables at the same object.
Example: changing a copied int does not change the original.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 7. What is the difference between class, struct and record?

**Answer:** A class is a reference type; a struct is a value type; a
record provides value-oriented equality and concise data modelling.
Example: use a class for an entity with identity, a record for an
immutable DTO.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 8. What is boxing and unboxing? What are its performance implications?

**Answer:** Boxing wraps a value type in an object; unboxing extracts
it. Boxing can allocate and add GC pressure. Example: assigning an int
to object boxes it; casting it back to int unboxes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 9. What is the difference between const, readonly and static readonly?

**Answer:** const is a compile-time constant; readonly can be assigned
at declaration or in a constructor; static readonly is one value per
type. Example: use static readonly for a value calculated at startup.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 10. What is the difference between ref, out and in?

**Answer:** ref passes a variable by reference and requires
initialization; out must be assigned by the method; in passes by
readonly reference. Example: TryParse uses out for its parsed result.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 11. What is the difference between is and as?

**Answer:** is tests whether a value matches a type or pattern; as
attempts a reference/nullable conversion and returns null on failure.
Example: if (obj is Customer c) uses a safe type test.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 12. What is the difference between method overloading and method overriding?

**Answer:** Overloading uses the same method name with different
parameter lists; overriding replaces a virtual base implementation in a
derived class. Example: Print(int) and Print(string) overload; Dog
overrides Animal.Speak().

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 13. What is method hiding using new?

**Answer:** The new keyword hides a base member rather than overriding
it. The selected member depends on the compile-time reference type.
Example: Base x = new Derived(); calls the base hidden method.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 14. What is the difference between an abstract class and an interface?

**Answer:** An abstract class can hold state, constructors and shared
implementation; an interface defines a contract and can also have
default members. A class can implement many interfaces but inherit only
one class.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 15. Can an interface contain implementations?

**Answer:** Yes. Modern C# supports default interface implementations
and static abstract members. They help evolve contracts, but should not
replace clear implementation ownership.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 16. What are default interface methods?

**Answer:** They are interface methods with a body, used to add
functionality without forcing every existing implementer to add the
method immediately. Example: an interface can provide a default logging
helper.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 17. What is explicit interface implementation?

**Answer:** A class implements an interface member so it is callable
through the interface reference, not directly through the class
instance. Useful when two interfaces have same-named members.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 18. What is polymorphism? Explain compile-time and runtime polymorphism.

**Answer:** Polymorphism lets one interface represent different
behaviours. Overloading is compile-time polymorphism; virtual dispatch
is runtime polymorphism. Example: Shape.Draw() behaves differently for
Circle and Rectangle.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 19. What are encapsulation, inheritance and abstraction?

**Answer:** Encapsulation protects internal state; inheritance
reuses/extends a base type; abstraction exposes essential behaviour
while hiding details. Example: BankAccount.Deposit() validates changes
without exposing its balance setter.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 20. What is the difference between shallow copy and deep copy?

**Answer:** A shallow copy duplicates the outer object but shares
referenced children; a deep copy duplicates the object graph. Example:
two copied orders may still share the same mutable Address in a shallow
copy.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

## 2. Advanced C# language concepts

### 21. What are delegates, and how do they work internally?

**Answer:** A delegate is a type-safe reference to one or more methods,
including their target object when applicable. Example:
Action`<string>`{=html} can refer to Console.WriteLine.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 22. What is the difference between Func, Action and Predicate?

**Answer:** Func returns a value, Action returns void, and
Predicate`<T>`{=html} returns bool for a T. Example: Func\<int,bool\>
checks a number; Action`<int>`{=html} prints it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 23. What are multicast delegates?

**Answer:** A multicast delegate holds an invocation list and calls each
registered method in order. If one handler throws, later handlers
normally are not called unless exceptions are handled.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 24. What is an event in C#? How is it different from a delegate?

**Answer:** An event wraps a delegate and restricts external code to
subscribing or unsubscribing; only the declaring type can raise it.
Example: OrderPlaced event notifies subscribers.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 25. What is the difference between IEnumerable and IEnumerator?

**Answer:** IEnumerable represents something that can provide an
enumerator; IEnumerator performs the iteration using MoveNext and
Current. Example: foreach obtains an enumerator from a list.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 26. What is deferred execution in LINQ?

**Answer:** A query is defined now but runs when enumerated. Example:
var q = items.Where(...); filtering happens when you iterate q, not when
q is created.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 27. What is the difference between deferred execution and immediate execution?

**Answer:** Deferred operators build a query; immediate operators
execute it and return a result. Where is deferred; ToList, Count and
First execute.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 28. What is the difference between IEnumerable`<T>`{=html} and IQueryable`<T>`{=html}?

**Answer:** IEnumerable runs delegates over in-process objects;
IQueryable builds expression trees that a provider can translate, often
into SQL. Example: EF Core filters can execute in the database.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 29. What is the difference between IQueryable`<T>`{=html} and IAsyncEnumerable`<T>`{=html}?

**Answer:** IQueryable describes a provider-translatable query;
IAsyncEnumerable streams results asynchronously. EF Core can expose
async enumeration with AsAsyncEnumerable().

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 30. What is an expression tree?

**Answer:** An expression tree represents code as data, allowing
inspection or translation. Example: EF Core reads x =\> x.Price \> 100
and translates it into SQL.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 31. How does LINQ translate expressions into SQL?

**Answer:** EF Core's provider inspects the expression tree and
translates supported operations into SQL. Unsupported expressions may
fail translation or, in limited cases, run after materialization.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 32. What happens when you call ToList() in the middle of a LINQ query?

**Answer:** The query executes immediately and results are materialized
in memory. Later filters run in the application rather than being added
to the SQL query.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 33. What is the difference between Select and SelectMany?

**Answer:** Select maps each item to one result; SelectMany maps each
item to a sequence and flattens the sequences. Example: flatten all
order line items.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 34. What is the difference between First, FirstOrDefault, Single and SingleOrDefault?

**Answer:** First takes the first match; Single requires exactly one
match. The OrDefault variants return default when none exists; Single
still throws if multiple matches exist.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 35. What are extension methods, and how does the compiler resolve them?

**Answer:** An extension method is a static method called with instance
syntax. The compiler resolves it from imported namespaces and applicable
static classes. Example: Enumerable.Where(list, predicate).

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 36. What are generics and generic constraints?

**Answer:** Generics let types and methods work with multiple types
while retaining compile-time safety. Constraints restrict allowed type
arguments, such as where T : class or new().

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 37. What is covariance and contravariance in C#?

**Answer:** Covariance allows a more derived output type; contravariance
allows a less derived input type. Example: IEnumerable`<string>`{=html}
can be used as IEnumerable`<object>`{=html}.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 38. What is the difference between object, dynamic and var?

**Answer:** object is statically typed as object; dynamic defers member
binding to runtime; var is compile-time type inference. Example: var n =
5 is still an int.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 39. What are nullable reference types?

**Answer:** They are compiler annotations and analysis that help detect
possible null dereferences. Example: string? means null is allowed;
string means the code expects non-null.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 40. What are pattern matching and switch expressions?

**Answer:** Pattern matching tests shape, type, values or properties
concisely. Example: return value switch { \> 0 =\> "positive", 0 =\>
"zero", \_ =\> "negative" };

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 41. What are records and how does value-based equality work?

**Answer:** Records generate equality based on their data members rather
than object identity. Example: two Person records with the same property
values compare equal.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 42. What are init-only setters?

**Answer:** An init accessor allows a property to be set during object
initialization or a permitted construction phase, then prevents ordinary
reassignment. Example: new User { Name = "A" }.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 43. What is the difference between yield return and returning a collection?

**Answer:** yield return produces items lazily as the caller iterates,
avoiding building the whole collection first. Example: stream a large
file line by line.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 44. What are iterators and how are they implemented by the compiler?

**Answer:** Iterator methods use yield return; the compiler generates a
state machine that remembers the current position between MoveNext
calls.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 45. What is the difference between lock, Monitor and SemaphoreSlim?

**Answer:** lock is convenient syntax over Monitor for mutual exclusion;
SemaphoreSlim limits concurrent entrants and supports async WaitAsync.
Do not await inside lock.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 46. What are Span`<T>`{=html} and Memory`<T>`{=html}?

**Answer:** They represent contiguous memory without requiring a new
array allocation. Span is stack-only and synchronous; Memory can be
stored and used across async boundaries.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 47. What is stackalloc, and when should you avoid it?

**Answer:** stackalloc allocates a small memory region on the stack.
Avoid large or unbounded allocations because they can cause stack
overflow.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 48. What are ref structs and why do they have restrictions?

**Answer:** Ref structs such as Span`<T>`{=html} may reference stack
memory, so the compiler prevents them escaping to unsafe lifetimes such
as heap fields or ordinary async state machines.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 49. What are source generators in .NET?

**Answer:** Source generators inspect compilation information and emit
C# source during compilation. They can remove runtime reflection or
generate repetitive code, such as serializers.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 50. What is the difference between a normal method and a local function?

**Answer:** A local function is declared inside another method and can
access its variables. It is useful for private helper logic scoped to
one operation.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

## 3. Memory management and Garbage Collection

### 51. How does Garbage Collection work in .NET?

**Answer:** The GC finds objects reachable from roots, reclaims
unreachable objects, and may compact managed heaps. Developers normally
do not free managed objects manually.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 52. Explain Generation 0, Generation 1 and Generation 2.

**Answer:** New short-lived objects start in Gen 0. Survivors may be
promoted to Gen 1 and Gen 2; Gen 2 collections are generally more
expensive.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 53. What is the Large Object Heap (LOH)?

**Answer:** The LOH stores large objects, commonly arrays around 85 KB
or larger. Large allocations can increase memory pressure and
fragmentation.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 54. What is the difference between Stack and Heap?

**Answer:** The stack stores call frames and local execution state; the
managed heap stores objects whose lifetimes are managed by GC. A
reference local may be on the stack while its object is on the heap.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 55. How does the GC identify objects that can be collected?

**Answer:** It traces references starting from GC roots. Objects not
reachable from roots are eligible for collection.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 56. What are GC roots?

**Answer:** Roots are references the GC treats as starting points, such
as stack locals, static fields, CPU registers and GC handles.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 57. What is object promotion between generations?

**Answer:** An object that survives a collection can move to an older
generation. This reflects the observation that long-lived objects often
remain alive longer.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 58. What is the difference between workstation GC and server GC?

**Answer:** Workstation GC is optimized for client responsiveness and
resource use; server GC uses multiple heaps/threads to improve
throughput on server workloads.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 59. What is a memory leak in a garbage-collected language?

**Answer:** A leak occurs when objects remain reachable even though the
application no longer needs them. Example: an event subscription keeps
an abandoned view model alive.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 60. How can static events cause memory leaks?

**Answer:** A long-lived publisher's event delegate references the
subscriber, keeping it alive. Unsubscribe when the subscriber's lifetime
ends or use a suitable weak-event design.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 61. What is the purpose of IDisposable?

**Answer:** IDisposable provides deterministic release of resources such
as file handles, streams and database connections. Use using to ensure
Dispose is called.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 62. What is the difference between Dispose() and a finalizer?

**Answer:** Dispose is explicit and deterministic; a finalizer is
nondeterministic cleanup performed by the GC for unmanaged-resource
safety. Prefer SafeHandle and Dispose patterns.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 63. What is the using statement and how does it work internally?

**Answer:** using ensures Dispose is called even when an exception
occurs, effectively compiling to try/finally. Example: using var stream
= File.OpenRead(path).

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 64. What is IAsyncDisposable?

**Answer:** It provides asynchronous cleanup through DisposeAsync,
useful when releasing a resource requires asynchronous work. Use await
using.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 65. What is the difference between GC.Collect() and normal automatic GC?

**Answer:** Normal GC decides when collection is beneficial. GC.Collect
forces a collection request and usually harms performance if used
routinely.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 66. What is a pinned object?

**Answer:** A pinned object cannot be moved by the GC, often because
native code holds its address. Excessive pinning can fragment the heap.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 67. What is LOH fragmentation?

**Answer:** It is unused gaps between large-object allocations that may
prevent efficient reuse. Reduce churn, reuse buffers and measure before
changing GC settings.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 68. What is ArrayPool`<T>`{=html} and when would you use it?

**Answer:** ArrayPool rents and returns reusable arrays to reduce
repeated allocations. Example: rent a byte buffer for high-throughput
network processing and return it in finally.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 69. What is ObjectPool`<T>`{=html}?

**Answer:** An object pool reuses expensive objects instead of
repeatedly creating them. Pool only when profiling shows
construction/allocation is costly and objects can be safely reset.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 70. How would you investigate increasing memory consumption in production?

**Answer:** Capture memory dumps, compare heap snapshots, inspect GC
counters and allocation stacks, then find retained object paths. Tools
include dotnet-counters, dotnet-dump and profilers.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 71. Can an object be alive even when there are no obvious references to it?

**Answer:** Yes. It may be retained by a static field, event, closure,
timer, cache, finalization queue or native handle. Inspect GC roots in a
dump.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 72. What is the difference between weak references and strong references?

**Answer:** A strong reference keeps an object alive; a weak reference
does not prevent collection. Weak references suit optional caches, not
required business state.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 73. How do finalization queues work?

**Answer:** Objects with finalizers are queued for finalization after
becoming unreachable; the finalizer thread runs them, and reclamation
may require another collection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 74. What is the difference between GC.SuppressFinalize() and GC.KeepAlive()?

**Answer:** SuppressFinalize prevents a finalizer from running after
deterministic cleanup. KeepAlive ensures an object remains alive until a
specified point in execution.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 75. How would excessive object allocation affect application throughput?

**Answer:** It increases GC frequency and CPU spent collecting,
potentially increasing latency. Reduce temporary allocations, reuse
buffers and stream data where appropriate.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 76. How can boxing increase GC pressure?

**Answer:** Boxing value types creates object allocations. Prefer
generic collections and typed APIs instead of object-based APIs where
possible.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 77. How would you identify LOH allocation issues?

**Answer:** Use allocation profiling and GC counters/dumps to find large
arrays or strings. Reuse buffers or stream content instead of
constructing huge temporary objects.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 78. What is the difference between latency mode and throughput-oriented GC?

**Answer:** GC latency modes trade collection timing and pause behaviour
against throughput. Choose only after measuring the workload and
understanding memory limits.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 79. How does Span`<T>`{=html} help reduce allocations?

**Answer:** Span slices existing memory without copying it. Example:
parse part of a string or byte buffer without allocating a
substring/array.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 80. How would you optimize an allocation-heavy API without sacrificing maintainability?

**Answer:** Profile first; then remove avoidable copies, stream large
results, use pooling selectively and keep APIs clear. Verify
improvements with benchmarks and production metrics.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

## 4. Asynchronous programming, Tasks and Multithreading

### 81. What is the difference between synchronous and asynchronous programming?

**Answer:** Synchronous work blocks the caller until completion;
asynchronous work allows the caller to await completion without holding
a thread idle during I/O. Example: await an HTTP request.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 82. What is the difference between a Task and a Thread?

**Answer:** A Thread is an OS execution thread; Task represents an
operation or promise of completion and may run on a ThreadPool thread or
complete through I/O callbacks.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 83. Does async/await create a new thread?

**Answer:** No. async/await is a compiler-supported way to compose
asynchronous operations. I/O awaits usually do not need a dedicated
thread while waiting.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 84. How does the compiler transform an async method?

**Answer:** The compiler generates a state machine that stores execution
state across await points and resumes when awaited operations complete.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 85. What is a Task state machine?

**Answer:** It is generated code that tracks the async method's current
state, locals and continuation so execution can resume after an await.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 86. What is the difference between Task, Task`<T>`{=html} and ValueTask`<T>`{=html}?

**Answer:** Task represents completion without a result;
Task`<T>`{=html} returns a result; ValueTask`<T>`{=html} can avoid
allocation when results often complete synchronously, but is more
complex.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 87. When should you use ValueTask instead of Task?

**Answer:** Use it only when profiling shows Task allocation is
significant and the operation frequently completes synchronously.
Otherwise Task is simpler and safer.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 88. What happens when you call .Result or .Wait() on an asynchronous operation?

**Answer:** The calling thread blocks. This can cause thread-pool
starvation and, in some contexts, deadlocks. Prefer await.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 89. What is sync-over-async?

**Answer:** It means blocking on asynchronous work using Result, Wait or
GetAwaiter().GetResult(). It can waste threads and cause starvation or
deadlocks.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 90. What is a deadlock in asynchronous programming?

**Answer:** A deadlock occurs when continuations cannot run because
required resources or contexts are blocked. Blocking on async work in a
captured synchronization context is a classic case.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 91. What is ConfigureAwait(false)?

**Answer:** It tells an await not to require resuming on the captured
synchronization context. It is commonly useful in reusable library code;
ASP.NET Core normally has no classic request SynchronizationContext.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 92. What is SynchronizationContext?

**Answer:** It represents a context for scheduling continuations, such
as a UI thread. Await may capture it and resume there unless configured
otherwise.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 93. What is TaskScheduler?

**Answer:** It determines where Tasks are scheduled. The default
scheduler generally uses the ThreadPool.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 94. What is the difference between Task.Run() and directly awaiting an asynchronous method?

**Answer:** Task.Run queues CPU-bound work to the ThreadPool. Do not
wrap naturally asynchronous I/O in Task.Run in server code; just await
it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 95. What is the difference between CPU-bound and I/O-bound operations?

**Answer:** CPU-bound work spends time computing; I/O-bound work waits
for disk, network or database operations. Parallel CPU work may use
threads; async I/O avoids blocking while waiting.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 96. What is the ThreadPool?

**Answer:** It is a pool of reusable worker threads used by Tasks,
timers and many runtime operations. It reduces the cost of creating
threads repeatedly.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 97. What is ThreadPool starvation?

**Answer:** It occurs when available worker threads are blocked or busy,
delaying queued work. Common causes include sync-over-async and long
blocking operations.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 98. What is the difference between Task.WhenAll() and Task.WhenAny()?

**Answer:** WhenAll completes when every supplied task completes;
WhenAny completes when the first task completes. Example: run
independent calls concurrently with WhenAll.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 99. How does Task.WhenAll() handle multiple exceptions?

**Answer:** The returned task faults if any task faults; its Exception
contains aggregated exceptions. Await typically throws one exception,
while inspecting task.Exception reveals the aggregate.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 100. What is CancellationToken?

**Answer:** It is a cooperative signal that a caller requests
cancellation. Pass it through APIs and check/observe it; it does not
forcibly terminate code.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 101. How does cooperative cancellation work?

**Answer:** The caller cancels a CancellationTokenSource; operations
receive the token and stop at safe points or pass it to cancellable
APIs.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 102. What happens if a method ignores a cancellation token?

**Answer:** That method continues running until it finishes or
independently stops. Cancellation only works when the operation observes
the token.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 103. What is a race condition?

**Answer:** A race occurs when outcome depends on timing of concurrent
operations. Example: two requests read stock=1 and both decrement it
without atomic protection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 104. What is a thread-safe application?

**Answer:** Its shared operations remain correct when accessed
concurrently. Use immutable data, synchronization, concurrent
collections or atomic database operations.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 105. What is the difference between lock and SemaphoreSlim?

**Answer:** lock provides synchronous exclusive access; SemaphoreSlim
can limit concurrency and supports asynchronous waiting. Use WaitAsync
in async methods.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 106. What is a deadlock in multithreaded programming?

**Answer:** Two or more threads wait forever for resources held by each
other. Avoid inconsistent lock ordering and keep critical sections
short.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 107. What is the difference between parallelism and concurrency?

**Answer:** Concurrency manages overlapping work; parallelism executes
work at the same time on multiple cores. Async I/O can be concurrent
without parallel CPU execution.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 108. What is Parallel.ForEachAsync()?

**Answer:** It processes an enumerable with bounded parallel
asynchronous delegates. Set MaxDegreeOfParallelism to protect downstream
resources.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 109. What is a producer-consumer pattern?

**Answer:** Producers create work items and consumers process them
through a queue/channel. It decouples production speed from processing
capacity.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 110. How would you limit the number of concurrent API calls?

**Answer:** Use SemaphoreSlim or a bounded Channel, acquire before
calling and release in finally. Also configure downstream timeouts and
rate limits.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 111. Your API suddenly experiences high CPU usage and increasing request latency. How would you investigate ThreadPool starvation?

**Answer:** Check ThreadPool counters, queue length, thread count and
blocked stacks. Search for sync-over-async, locks and blocking I/O;
capture traces before changing thread settings.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 112. You have 10 independent API calls. How would you execute them concurrently without creating unnecessary threads?

**Answer:** Start all asynchronous calls and await Task.WhenAll. Use
HttpClient and cancellation tokens; limit concurrency if the downstream
service has capacity constraints.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 113. One task fails inside Task.WhenAll(). What happens to the remaining tasks?

**Answer:** WhenAll does not automatically cancel siblings. Other tasks
continue unless you use a linked CancellationTokenSource and cancel them
explicitly.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 114. Your application processes 10,000 records. How would you process them concurrently while limiting database connections?

**Answer:** Use bounded batches or a bounded Channel with a small worker
count. Reuse scoped DbContexts per worker and tune concurrency to
database capacity.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 115. Two threads simultaneously update the same account balance. How would you prevent inconsistent data?

**Answer:** Use a database transaction with a conditional update or
optimistic concurrency token. An in-process lock is insufficient across
multiple app instances.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 116. Your API receives a cancellation request, but a downstream HTTP request continues running. How would you fix it?

**Answer:** Pass HttpContext.RequestAborted or the operation token to
HttpClient.SendAsync and downstream methods; ensure no layer drops the
token.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 117. How would you implement a background worker that processes messages without blocking incoming HTTP requests?

**Answer:** Use a hosted BackgroundService with a bounded Channel or
broker consumer. Keep work off the request path and create a DI scope
per unit of work.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 118. How would you troubleshoot a deadlock that occurs only occasionally in production?

**Answer:** Capture dumps/traces during the incident, inspect waiting
threads and lock ownership, then map stacks to code. Add correlation and
lock-duration telemetry.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 119. When would you use Channel`<T>`{=html} instead of ConcurrentQueue`<T>`{=html}?

**Answer:** Channel provides async producer/consumer coordination,
completion and bounded capacity/backpressure. ConcurrentQueue is a
thread-safe queue but does not provide async waiting.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 120. How would you implement a thread-safe singleton without introducing unnecessary locking?

**Answer:** Use a static readonly instance or a nested static holder;
CLR type initialization is thread-safe. Prefer DI singleton registration
in application code.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

## 5. ASP.NET Core and Web API internals

### 121. Explain the complete ASP.NET Core request lifecycle.

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 122. What is middleware?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 123. How does middleware execute internally?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 124. What is the difference between Use, Run and Map?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 125. What is middleware short-circuiting?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 126. Why does middleware ordering matter?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 127. What happens if exception-handling middleware is registered after authentication middleware?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 128. What is dependency injection?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 129. Explain Transient, Scoped and Singleton lifetimes.

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 130. What happens when a Singleton depends on a Scoped service?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 131. What is a captive dependency?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 132. How does the built-in .NET DI container resolve dependencies?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 133. What happens if multiple implementations of the same interface are registered?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 134. What is IServiceScope?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 135. How do you resolve scoped services inside a BackgroundService?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 136. What is IHttpClientFactory?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 137. Why is creating a new HttpClient for every request problematic?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 138. What is socket exhaustion?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 139. How does IHttpClientFactory manage HttpMessageHandler lifetimes?

**Answer:** IHttpClientFactory creates configured HttpClient instances
while pooling handlers and managing handler lifetime. Repeatedly
creating/discarding handlers can exhaust sockets; use a named/typed
client and configure timeouts and resilience.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 140. What is the Options pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 141. Difference between IOptions, IOptionsSnapshot and IOptionsMonitor.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 142. How does configuration loading work in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 143. What is the difference between Minimal APIs and Controllers?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 144. What is model binding?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 145. What is model validation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 146. What is the difference between filters and middleware?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 147. Explain authorization filters, action filters and exception filters.

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 148. What is content negotiation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 149. What is the difference between IActionResult, ActionResult`<T>`{=html} and IResult?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 150. What is API versioning?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 151. How do you implement global exception handling?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 152. What is Problem Details in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 153. What is endpoint routing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 154. How does attribute routing differ from conventional routing?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 155. What is Kestrel?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 156. What is a reverse proxy?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 157. How does HTTPS termination work?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 158. What is CORS, and why is it enforced by browsers?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 159. What is rate limiting in ASP.NET Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 160. How would you implement IP-based and user-based rate limiting?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 161. An API works locally but returns HTTP 500 after deployment. How would you investigate it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 162. Your API receives 5,000 simultaneous requests. How would you protect the database from being overwhelmed?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 163. Your middleware is not executing. How would you debug the request pipeline?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 164. Your Singleton service accidentally depends on a Scoped DbContext. What can go wrong?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 165. How would you implement global exception handling without exposing internal stack traces to clients?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 166. Your API is responding slowly even though the database query is fast. What would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 167. How would you implement API versioning without breaking existing clients?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 168. How would you secure an API that is consumed by multiple independent applications?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 169. How would you implement request correlation across multiple microservices?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 170. How would you prevent a slow downstream service from exhausting your API's available resources?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

## 6. Entity Framework Core and database interaction

### 171. What is Entity Framework Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 172. What is the difference between EF Core and Dapper?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 173. What is DbContext?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 174. Why is DbContext not thread-safe?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 175. What is Change Tracking?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 176. Explain the entity states: Added, Modified, Deleted, Unchanged and Detached.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 177. What is AsNoTracking()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 178. What is AsNoTrackingWithIdentityResolution()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 179. What is the difference between eager loading, lazy loading and explicit loading?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 180. What is the N+1 query problem?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 181. What is the difference between Include() and ThenInclude()?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 182. What is the difference between Find() and FirstOrDefault()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 183. What is the difference between SaveChanges() and SaveChangesAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 184. How does EF Core manage database transactions?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 185. What is optimistic concurrency?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 186. What is a concurrency token?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 187. What is SQL Server rowversion?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 188. What is the difference between IQueryable and IEnumerable in EF Core?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 189. What is client-side evaluation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 190. What is the difference between single-query and split-query execution?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 191. What is Cartesian explosion in EF Core?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 192. What are compiled queries?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 193. What is DbContext pooling?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 194. What is connection pooling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 195. What are global query filters?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 196. How would you implement soft deletion using global query filters?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 197. What is the difference between ExecuteUpdateAsync() and loading an entity followed by SaveChangesAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 198. What happens to the ChangeTracker when you use ExecuteDeleteAsync()?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 199. What are EF Core migrations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 200. How would you apply database migrations safely in production?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 201. A query is taking 8 seconds to execute. How would you identify whether the problem is EF Core, SQL Server or network latency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 202. Your API loads 10,000 records but only displays 20. How would you optimize this?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 203. You have multiple Include() operations and the query generates huge amounts of duplicated data. How would you solve it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 204. Two users update the same record simultaneously. How would you handle concurrency conflicts?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 205. How would you execute multiple database operations atomically?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 206. You have a very complex LINQ query. How would you inspect the SQL generated by EF Core?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 207. Your application executes the same query thousands of times. How would you optimize query compilation and execution?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 208. When would you choose raw SQL over LINQ?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 209. When would you choose Dapper instead of EF Core?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 210. How would you implement bulk updates for millions of records?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 211. How would you prevent accidental loading of large object graphs?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 212. Why should you generally avoid sharing one DbContext between multiple concurrent operations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 213. How would you implement multi-tenancy using EF Core global query filters?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 214. How would you safely introduce a new non-nullable column into a production database with millions of existing records?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 215. How would you avoid database connection pool exhaustion?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

## 7. SQL Server and query optimization

### 216. What is the difference between clustered and non-clustered indexes?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 217. What is a composite index?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 218. What is index fragmentation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 219. What is an execution plan?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 220. What is a table scan versus an index seek?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 221. What is a covering index?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 222. What is the difference between a primary key and a unique key?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 223. What is a foreign key?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 224. What is normalization and denormalization?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 225. Explain database normalization forms.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 226. What are ACID properties?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 227. What is transaction isolation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 228. Explain Read Uncommitted, Read Committed, Repeatable Read and Serializable.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 229. What is Snapshot Isolation?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 230. What is a dirty read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 231. What is a non-repeatable read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 232. What is a phantom read?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 233. What is a database deadlock?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 234. What is the difference between blocking and deadlocking?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 235. What is a CTE?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 236. What is the difference between CTE, temporary table and table variable?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 237. What are window functions?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 238. Explain ROW_NUMBER(), RANK() and DENSE_RANK().

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 239. What is the difference between UNION and UNION ALL?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 240. What is parameter sniffing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 241. What is the difference between stored procedures and parameterized queries?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 242. What is a SARGable query?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 243. Why can applying functions to indexed columns affect query performance?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 244. How would you optimize a query involving millions of rows?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 245. How would you troubleshoot a SQL Server deadlock?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

## 8. Microservices and distributed systems

### 246. What is a microservice architecture?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 247. What is the difference between monolithic and microservice architecture?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 248. What are the advantages and disadvantages of microservices?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 249. What is an API Gateway?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 250. What is a reverse proxy?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 251. What is the difference between an API Gateway and a Load Balancer?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 252. What is service discovery?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 253. What is inter-service communication?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 254. What is synchronous versus asynchronous communication?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 255. What is the difference between REST and gRPC?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 256. What is eventual consistency?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 257. What is distributed caching?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 258. What is cache invalidation?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 259. What is the Cache-Aside pattern?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 260. What are Write-Through and Write-Behind caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 261. What is the Circuit Breaker pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 262. What is the Retry pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 263. What is exponential backoff?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 264. Why is jitter important in retry policies?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 265. What is the Bulkhead pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 266. What is the Timeout pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 267. What is the difference between Retry and Circuit Breaker?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 268. What is the Saga pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 269. What is the difference between orchestration and choreography?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 270. What is the Outbox pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 271. What is the Inbox pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 272. What is idempotency?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 273. How do you prevent duplicate message processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 274. What is distributed tracing?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 275. What is a correlation ID?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 276. What is a distributed transaction?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 277. Why is two-phase commit problematic in distributed systems?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 278. What is CQRS?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 279. What is Event Sourcing?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 280. What is the Strangler Fig pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 281. Your Order service successfully creates an order, but the Payment service goes down before payment processing. How would you handle this?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 282. A message is delivered twice by RabbitMQ. How would you prevent duplicate order processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 283. Your Product service is unavailable. Should the Order service retry indefinitely?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 284. Redis becomes unavailable. Should your API fail or fall back to the database?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 285. Two microservices update different databases as part of one business operation. How would you maintain consistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 286. Your service processes a message successfully but crashes before acknowledging it. What happens next?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 287. How would you implement an idempotent payment API?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 288. How would you handle a poison message that continuously fails processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 289. Your API Gateway is working, but one downstream service becomes extremely slow. How would you prevent cascading failures?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 290. How would you implement distributed rate limiting across five instances of the same API?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 291. How would you trace one request across an API Gateway, Order service, Product service and database?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 292. When would you use RabbitMQ instead of Kafka?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 293. What is the difference between RabbitMQ Direct, Topic, Fanout and Headers exchanges?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 294. What happens if a consumer dies while processing a message?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 295. How would you ensure reliable event publishing when a database transaction commits?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 296. How would you decide whether a business operation should use synchronous HTTP communication or asynchronous messaging?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 297. What happens when you introduce a breaking change in an event contract?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 298. How would you scale a microservice horizontally without causing duplicate processing?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 299. What is the difference between at-most-once, at-least-once and effectively-once processing?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 300. How would you design a distributed order-processing system that remains operational during partial failures?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

## 9. Redis and distributed caching

### 301. What is Redis, and how does it store data?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 302. Why is Redis called an in-memory data store?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 303. Is Redis a distributed cache or centralized cache?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 304. What is the difference between local memory caching and distributed caching?

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 305. What is IDistributedCache?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 306. How does IDistributedCache work with Redis?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 307. What is TTL (Time To Live)?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 308. What happens when a Redis key expires?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 309. What is cache eviction?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 310. Explain Redis eviction policies.

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 311. What is cache stampede?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 312. What is cache penetration?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 313. What is cache avalanche?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 314. What is cache warming?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 315. What is cache invalidation?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 316. How would you invalidate cached data when a product's price changes?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 317. What is cache-aside?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 318. What is Write-Through caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 319. What is Write-Behind caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 320. What is the difference between Redis Pub/Sub and Redis Streams?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 321. What are Redis persistence mechanisms?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 322. What is the difference between RDB and AOF?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 323. What happens when Redis runs out of memory?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 324. How would you implement distributed locking using Redis?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 325. How would you prevent multiple instances from refreshing the same expired cache simultaneously?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 326. Your application has five API instances sharing Redis. One instance updates a product price. How do you ensure the remaining instances don't serve stale data?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 327. Redis is operational, but response times suddenly increase. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 328. Your cache expires at the same time for thousands of users. How would you avoid a database traffic spike?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 329. A cache update succeeds, but the database update fails. How would you prevent inconsistent data?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 330. How would you implement a distributed cache with a fallback when Redis is unavailable?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

## 10. Security and authentication

### 331. What is authentication versus authorization?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 332. What is JWT?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 333. What are the three parts of a JWT?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 334. How does JWT authentication work in ASP.NET Core?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 335. What is the difference between access tokens and refresh tokens?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 336. Where should refresh tokens be stored?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 337. What is OAuth 2.0?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 338. What is OpenID Connect?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 339. What is the difference between OAuth 2.0 and OpenID Connect?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 340. What is the difference between role-based and policy-based authorization?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 341. What are claims and claims-based authorization?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 342. How does ASP.NET Core authentication middleware work?

**Answer:** Middleware is an ordered component in the HTTP request
pipeline. Each component can inspect/change the request or response,
call the next component, or short-circuit. Example: exception handling
should wrap downstream middleware so it can catch their failures.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 343. What is CSRF?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 344. What is XSS?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 345. What is SQL injection?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 346. How does parameterized SQL prevent SQL injection?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 347. What is CORS?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 348. What is the difference between HTTPS and HTTP?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 349. How would you securely store passwords?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 350. What is the principle of least privilege?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 351. Your JWT has not expired, but a user has been disabled. How would you immediately prevent further access?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 352. How would you implement refresh-token rotation and token revocation?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 353. How would you secure communication between internal microservices?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 354. How would you prevent replay attacks?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 355. How would you secure secrets in Docker containers and Kubernetes deployments?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 356. How would you implement authorization for a resource owned by a particular user?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 357. What are the security risks of storing JWTs in browser local storage?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 358. How would you protect an API against brute-force login attempts?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

## 11. SOLID principles and design patterns

### 359. Explain all five SOLID principles with real-world examples.

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 360. What is Dependency Inversion?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 361. What is the difference between Dependency Injection and Inversion of Control?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 362. What is the Repository pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 363. What is the Unit of Work pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 364. What is the Factory pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 365. What is the Abstract Factory pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 366. What is the Strategy pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 367. What is the Decorator pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 368. What is the Adapter pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 369. What is the Observer pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 370. What is the Mediator pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 371. What is the Singleton pattern?

**Answer:** DI supplies a class's dependencies rather than having it
construct them. Transient creates each resolution, Scoped is one per
scope/request, and Singleton is one per container. Never capture a
scoped DbContext in a singleton; create a scope when background work
needs it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 372. Why can Singleton become an anti-pattern?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 373. What is the Template Method pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 374. What is the Specification pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 375. What is the Chain of Responsibility pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 376. What is the difference between composition and inheritance?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 377. When should you prefer composition over inheritance?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 378. What is Clean Architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 379. What is Onion Architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 380. What is the difference between Clean Architecture and N-tier architecture?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 381. Would you always use the Repository pattern with EF Core? Why or why not?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 382. How would you design a system that supports multiple payment providers without changing existing business logic?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 383. How would you implement a notification system supporting Email, SMS and Push notifications?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 384. How would you prevent a service class from becoming a God Object?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 385. How would you identify overengineering in an application?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 386. How would you refactor tightly coupled legacy code without breaking existing functionality?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 387. How would you design a plugin-based architecture in .NET?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 388. How would you introduce a new feature following Open/Closed Principle?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

## 12. Docker, Kubernetes and deployment

### 389. What is Docker?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 390. What is the difference between a Docker image and a container?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 391. What is a Dockerfile?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 392. What is Docker Compose?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 393. What is a Docker network?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 394. How do containers communicate with each other?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 395. What is the difference between a Docker volume and a bind mount?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 396. What is a multi-stage Docker build?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 397. Why should you avoid running containers as root?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 398. How do environment variables work inside containers?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 399. What happens when a container exits?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 400. What is a container health check?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 401. What is Kubernetes?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 402. What is a Pod?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 403. What is a Deployment?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 404. What is a Service in Kubernetes?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 405. What is the difference between ClusterIP, NodePort and LoadBalancer?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 406. What is an Ingress?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 407. What is a ConfigMap?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 408. What is a Secret?

**Answer:** Authentication establishes identity; authorization checks
permissions. Use standard ASP.NET Core authentication/authorization
middleware, short-lived access tokens, secure refresh-token storage,
HTTPS, parameterized SQL and least privilege. Example: a policy checks a
user's claim and resource ownership.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 409. What are Liveness, Readiness and Startup Probes?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 410. What is horizontal pod autoscaling?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 411. How does Kubernetes perform rolling deployments?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 412. What is a CrashLoopBackOff?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 413. How would you troubleshoot a pod that keeps restarting?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 414. How would you perform a zero-downtime deployment?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 415. How would you handle database schema changes during rolling deployments?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

## 13. Unit testing and software quality

### 416. What is unit testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 417. What is integration testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 418. What is the difference between unit and integration testing?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 419. What is mocking?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 420. What is the difference between Mock, Stub and Fake?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 421. What is Moq?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 422. What is dependency isolation in unit testing?

**Answer:** Start with the actual execution plan, query duration, waits
and row counts. Use suitable indexes and SARGable predicates, keep
transactions short, and understand isolation/locking. Example: avoid
WHERE CAST(CreatedAt AS date)=... when a range predicate can use an
index.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 423. What is Test-Driven Development?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 424. What is the Arrange-Act-Assert pattern?

**Answer:** A design principle or pattern is a tool for managing change
and dependencies, not a goal by itself. Use the simplest design that
isolates likely variation. Example: Strategy lets different payment
providers implement the same payment contract.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 425. What is code coverage?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 426. What is the difference between a unit test and a component test?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 427. How would you test a service that depends on Redis?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 428. How would you test an API endpoint requiring JWT authentication?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Security answers should identify the threat being addressed and where enforcement occurs. Validate on the server, apply least privilege, protect secrets, and avoid logging credentials, tokens or sensitive personal data.

### 429. How would you test asynchronous methods?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 430. How would you test code that depends on DateTime or the current system time?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 431. How would you test a retry mechanism?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 432. How would you test a method that publishes RabbitMQ messages?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 433. What is a flaky test?

**Answer:** A good test verifies observable behaviour with controlled
dependencies. Unit tests isolate logic; integration tests exercise real
infrastructure boundaries. Example: test a service with a fake
repository, then test EF Core queries against a disposable database.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 434. How do you test concurrent operations?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 435. What is contract testing in microservices?

**Answer:** Docker packages an application and its dependencies;
Kubernetes schedules and manages containers. Use immutable images,
external configuration/secrets, resource requests/limits, health probes
and rolling deployments. Example: readiness controls whether a pod
receives traffic.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

## 14. API design and performance engineering

### 436. What are REST architectural constraints?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 437. What is idempotency in HTTP?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 438. Explain GET, POST, PUT, PATCH and DELETE.

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 439. What is the difference between PUT and PATCH?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 440. What is the difference between 401 and 403?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 441. What is the difference between 400, 404, 409 and 422?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 442. What is pagination?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 443. What is the difference between offset pagination and cursor pagination?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 444. What is response compression?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 445. What is output caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 446. What is response caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 447. What is the difference between response caching and output caching?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 448. What is connection pooling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 449. What is asynchronous streaming?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 450. What is backpressure?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 451. What is request throttling?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 452. How would you improve API throughput?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 453. How would you reduce API latency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 454. How would you investigate high memory consumption?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 455. How would you investigate high CPU consumption?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 456. How would you optimize a slow API without changing its business logic?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 457. What is BenchmarkDotNet?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 458. What is profiling?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 459. What is observability?

**Answer:** Design the API around clear resource semantics, correct HTTP
status codes, validation, pagination and consistent errors. Measure
latency by dependency and use logs, metrics and traces. Example: cursor
pagination avoids expensive large OFFSET scans.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 460. Explain logs, metrics and distributed traces.

**Answer:** Design for partial failure: bounded timeouts, limited
retries with exponential backoff/jitter, circuit breakers, idempotent
consumers, durable messaging and observability. Example: an Outbox
stores the event in the same DB transaction as the order, then a worker
publishes it.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

## 15. Advanced .NET runtime and modern platform concepts

### 461. What is the difference between .NET Framework, .NET Core and modern .NET?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 462. What is the difference between JIT and AOT compilation?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 463. What is Native AOT?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 464. What is ReadyToRun compilation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 465. What is tiered compilation?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 466. What is Dynamic PGO?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 467. What is reflection?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 468. What are the performance implications of reflection?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 469. What are attributes in C#?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 470. How does attribute-based programming work internally?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 471. What is assembly loading?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 472. What is the difference between an assembly and a namespace?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 473. What is an AssemblyLoadContext?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 474. What is the Global Assembly Cache?

**Answer:** Redis is a fast in-memory data store often used as a shared
cache. Choose a TTL and invalidation strategy, tolerate cache misses,
and decide whether Redis failure should fall back to the database.
Example: cache product reads by ID and invalidate/update after a
committed price change.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 475. What is NuGet package management?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 476. What is the difference between runtime and SDK?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

### 477. What are nullable reference types and how does the compiler use nullable annotations?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** For the internal flow, describe the sequence rather than only naming the feature: what the caller supplies, what the runtime/framework does, what result is produced, and what can fail. This shows that you understand the mechanism instead of memorising a definition.

### 478. What is trimming in .NET?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 479. What are analyzers and code generators?

**Answer:** Explain the core concept, when it is useful, its limitations
and a concrete production example. Validate the answer with a small
implementation or test rather than relying only on a definition.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

### 480. What is the difference between source generation and runtime reflection?

**Answer:** The .NET runtime loads assemblies, executes IL and may
JIT-compile methods; AOT compiles ahead of time. Reflection discovers
metadata dynamically but can affect performance and trimming. Example:
source-generated JSON serialization reduces runtime reflection.

**How to explain it in an interview:** In an interview, make the distinction explicit: explain what each option represents, where the work happens, and the consequence for correctness, performance or maintainability. Then state the condition under which you would choose each one; avoid claiming that one is always better.

## 16. Advanced coding and problem-solving questions

### 481. Implement a thread-safe Singleton.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 482. Implement a custom middleware for request logging.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 483. Implement a custom exception-handling middleware.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 484. Implement a generic repository.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 485. Implement a custom extension method for IEnumerable`<T>`{=html}.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 486. Implement your own Where() extension method.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 487. Implement a simple LRU cache.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 488. Implement a thread-safe in-memory cache.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 489. Implement a rate limiter using the Token Bucket algorithm.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 490. Implement a fixed-window rate limiter.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 491. Implement a producer-consumer system using Channel`<T>`{=html}.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 492. Implement a retry mechanism with exponential backoff.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 493. Implement a circuit breaker.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 494. Implement an idempotency mechanism for a payment API.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 495. Implement pagination using EF Core.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 496. Implement a background worker using BackgroundService.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 497. Implement parallel processing with a maximum concurrency limit.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

### 498. Implement a distributed lock using Redis.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 499. Implement a custom LINQ aggregation method.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

### 500. Implement a service that processes 100,000 records without loading everything into memory.

**Answer:** First define the contract, edge cases and concurrency
requirements. Implement the smallest correct version, add
cancellation/error handling where relevant, and write tests for normal,
boundary and concurrent cases. Example: for a rate limiter, use a
monotonic time source and atomic state updates.

**How to explain it in an interview:** When presenting an implementation, explain the contract first, then the key data structures, the happy path, and the failure/concurrency cases. In production code, also validate inputs, propagate CancellationToken where appropriate, and add tests for boundary conditions.

## 17. Extremely challenging production scenarios

### 501. Your API response time increases from 200 ms to 5 seconds after deployment. CPU usage is normal, but requests are piling up. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

In deployment discussions, include configuration/secrets, health checks, resource limits, logs, networking and rollout/rollback behaviour. A container being running does not necessarily mean the application inside it is ready to serve traffic.

### 502. Your SQL query executes in 100 ms in SQL Server Management Studio but takes 3 seconds through EF Core. What could explain the difference?

**Answer:** EF Core maps .NET objects to relational data and translates
LINQ to SQL. Measure generated SQL and database execution plans; use
projections, pagination, appropriate tracking, indexes and concurrency
tokens. Example: AsNoTracking for read-only lists.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 503. Your application is experiencing high GC activity. How would you determine which part of the code creates excessive allocations?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 504. Two requests simultaneously purchase the last available product. How do you prevent overselling?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 505. A background worker and an API endpoint modify the same record simultaneously. How would you protect data consistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 506. Your application works perfectly with one instance but produces duplicate records when deployed with five instances. How would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 507. Your Order service writes to SQL Server and then publishes an event to RabbitMQ. The database operation succeeds, but RabbitMQ is unavailable. How would you guarantee eventual event delivery?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 508. Your consumer receives the same message multiple times. How would you guarantee that the business operation happens only once effectively?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 509. Your Redis cache and SQL Server contain different versions of a product. How would you resolve the inconsistency?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed applications, remember that process-local state is not shared between instances. Discuss expiration, invalidation, failures, key design and consistency. A cache or lock should not be treated as the authoritative source of business data.

### 510. Your microservice is dependent on three external services. One is slow, one is unavailable and one is returning intermittent errors. How would you design resilience?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 511. A production database has millions of records, and a frequently executed query suddenly becomes slow. What would you investigate?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 512. A migration needs to modify a heavily used table without causing significant downtime. How would you approach it?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 513. A transaction updates three tables. The first two succeed, but the third fails. How would you ensure atomicity?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 514. Your database connection pool is exhausted even though the number of incoming requests hasn't increased. What could be wrong?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A useful interview explanation connects the definition to a real application: state the problem the concept solves, explain the important behaviour, and give one example from an API or service. Also mention one limitation or trade-off so the answer sounds practical rather than memorised.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 515. You are asked to convert a monolithic application into microservices. How would you identify the service boundaries?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For distributed-system answers, assume networks and dependencies can fail. Explain timeouts, bounded retries, idempotency, duplicate delivery, observability and eventual consistency. Do not promise exactly-once delivery unless you define precisely what is guaranteed.

### 516. Your microservices share the same database. What problems could arise, and how would you migrate away from this design?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

For database answers, distinguish work performed by the database from work performed in application memory. Check generated SQL, execution plans, indexes, round trips, transaction boundaries and connection lifetime before assuming the ORM itself is the bottleneck.

### 517. How would you design a notification system that processes millions of notifications per day?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 518. How would you design a document management system with upload, storage, search, versioning and access control?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 519. How would you design an audit logging mechanism that records every important business operation?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

### 520. How would you design a system that supports 100,000 concurrent users?

**Answer:** Clarify the failure mode and business invariant, gather
logs/metrics/traces, reproduce safely, then choose a bounded and
observable fix. Protect data with transactions/constraints, make retries
idempotent, and test failure paths. Example: prevent overselling with an
atomic conditional UPDATE where Stock \> 0.

------------------------------------------------------------------------

**How to explain it in an interview:** A strong practical answer starts by clarifying the expected behaviour and constraints. Measure or inspect the current behaviour, identify the bottleneck or failure boundary, make the smallest safe change, and verify it with tests and observable evidence. Mention rollback or failure handling when the change affects production.

Important distinction: asynchronous waiting does not automatically mean parallel execution. For I/O, prefer true asynchronous APIs; for CPU-heavy work, use bounded parallelism only when the workload and available CPU justify it. Avoid blocking async code with Result or Wait().

## Worked implementations for questions 481–500

### Implementation notes for question 481

```csharp
public sealed class AppSettings
{
    private static readonly Lazy<AppSettings> _instance =
        new(() => new AppSettings(), LazyThreadSafetyMode.ExecutionAndPublication);

    public static AppSettings Instance => _instance.Value;
    private AppSettings() { }
}
```
`Lazy<T>` ensures thread-safe, once-only initialization. In ASP.NET Core, prefer registering a service with the DI container as a singleton rather than hand-writing this pattern, because DI handles dependencies and lifetime management.

### Implementation notes for question 482

```csharp
public sealed class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<RequestLoggingMiddleware> _logger;

    public RequestLoggingMiddleware(RequestDelegate next,
        ILogger<RequestLoggingMiddleware> logger)
        => (_next, _logger) = (next, logger);

    public async Task InvokeAsync(HttpContext context)
    {
        var started = Stopwatch.GetTimestamp();
        try
        {
            await _next(context);
        }
        finally
        {
            var elapsed = Stopwatch.GetElapsedTime(started);
            _logger.LogInformation(
                "{Method} {Path} returned {StatusCode} in {ElapsedMs} ms",
                context.Request.Method, context.Request.Path,
                context.Response.StatusCode, elapsed.TotalMilliseconds);
        }
    }
}
```
Register with `app.UseMiddleware<RequestLoggingMiddleware>();`. Avoid logging query strings or bodies indiscriminately because they may contain secrets or personal data.

### Implementation notes for question 483

```csharp
public sealed class ExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ExceptionHandlingMiddleware> _logger;

    public ExceptionHandlingMiddleware(RequestDelegate next,
        ILogger<ExceptionHandlingMiddleware> logger)
        => (_next, _logger) = (next, logger);

    public async Task InvokeAsync(HttpContext context)
    {
        try { await _next(context); }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Unhandled request exception");
            if (context.Response.HasStarted) throw;

            context.Response.Clear();
            context.Response.StatusCode = StatusCodes.Status500InternalServerError;
            context.Response.ContentType = "application/problem+json";
            await context.Response.WriteAsJsonAsync(new ProblemDetails
            {
                Status = 500,
                Title = "An unexpected error occurred."
            });
        }
    }
}
```
Place it early in the pipeline so it can catch exceptions from later middleware and endpoints. Do not expose stack traces or internal exception messages to clients.

### Implementation notes for question 484

```csharp
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(int id, CancellationToken ct = default);
    Task AddAsync(T entity, CancellationToken ct = default);
}

public sealed class EfRepository<T> : IRepository<T> where T : class
{
    private readonly AppDbContext _db;
    public EfRepository(AppDbContext db) => _db = db;

    public Task<T?> GetByIdAsync(int id, CancellationToken ct = default) =>
        _db.Set<T>().FindAsync(new object?[] { id }, ct).AsTask();

    public async Task AddAsync(T entity, CancellationToken ct = default) =>
        await _db.Set<T>().AddAsync(entity, ct);
}
```
This is a deliberately small example. A generic repository should not pretend every entity has the same query needs. EF Core's `DbContext` already implements Unit of Work-like behaviour; add a repository only when it provides a useful domain abstraction.

### Implementation notes for question 485

```csharp
public static class EnumerableExtensions
{
    public static IEnumerable<T> WhereNotNull<T>(
        this IEnumerable<T?> source) where T : class
    {
        foreach (var item in source)
            if (item is not null) yield return item;
    }
}
```
Extension methods are static methods called with instance syntax. The compiler resolves them from imported namespaces; they do not actually modify the original type.

### Implementation notes for question 486

```csharp
public static class MyLinq
{
    public static IEnumerable<T> Where<T>(
        this IEnumerable<T> source, Func<T, bool> predicate)
    {
        ArgumentNullException.ThrowIfNull(source);
        ArgumentNullException.ThrowIfNull(predicate);

        foreach (var item in source)
            if (predicate(item))
                yield return item;
    }
}
```
This iterator is deferred: the source is enumerated only when the caller iterates the result. A production implementation should also consider how exceptions and repeated enumeration behave.

### Implementation notes for question 487

```csharp
public sealed class LruCache<TKey, TValue> where TKey : notnull
{
    private readonly int _capacity;
    private readonly Dictionary<TKey, LinkedListNode<(TKey Key, TValue Value)>> _map = new();
    private readonly LinkedList<(TKey Key, TValue Value)> _list = new();

    public LruCache(int capacity)
    {
        if (capacity <= 0) throw new ArgumentOutOfRangeException(nameof(capacity));
        _capacity = capacity;
    }

    public bool TryGet(TKey key, out TValue value)
    {
        if (!_map.TryGetValue(key, out var node)) { value = default!; return false; }
        _list.Remove(node);
        _list.AddFirst(node);
        value = node.Value.Value;
        return true;
    }

    public void Put(TKey key, TValue value)
    {
        if (_map.TryGetValue(key, out var existing))
        {
            _list.Remove(existing);
            _map.Remove(key);
        }
        var node = _list.AddFirst((key, value));
        _map[key] = node;
        if (_map.Count <= _capacity) return;
        var last = _list.Last!;
        _map.Remove(last.Value.Key);
        _list.RemoveLast();
    }
}
```
This basic version is not thread-safe. Add a lock around both operations if it is shared across threads, and consider expiration and memory limits for production use.

### Implementation notes for question 488

```csharp
public sealed class AsyncMemoryCache<T>
{
    private readonly ConcurrentDictionary<string, (T Value, DateTimeOffset Expires)> _items = new();

    public void Set(string key, T value, TimeSpan ttl) =>
        _items[key] = (value, DateTimeOffset.UtcNow.Add(ttl));

    public bool TryGet(string key, out T value)
    {
        if (_items.TryGetValue(key, out var item) &&
            item.Expires > DateTimeOffset.UtcNow)
        { value = item.Value; return true; }

        _items.TryRemove(key, out _);
        value = default!;
        return false;
    }
}
```
This illustrates thread-safe dictionary access and TTL, not a complete production cache. Production implementations also need size limits, eviction policy, protection against cache stampedes, metrics and a decision about whether values are immutable.

### Implementation notes for question 489

```csharp
public sealed class TokenBucket
{
    private readonly object _gate = new();
    private readonly double _capacity, _tokensPerSecond;
    private double _tokens;
    private DateTime _lastRefill = DateTime.UtcNow;

    public TokenBucket(double capacity, double tokensPerSecond)
    {
        _capacity = capacity;
        _tokensPerSecond = tokensPerSecond;
        _tokens = capacity;
    }

    public bool TryConsume(double amount = 1)
    {
        lock (_gate)
        {
            var now = DateTime.UtcNow;
            _tokens = Math.Min(_capacity,
                _tokens + (now - _lastRefill).TotalSeconds * _tokensPerSecond);
            _lastRefill = now;
            if (_tokens < amount) return false;
            _tokens -= amount;
            return true;
        }
    }
}
```
This is process-local. In a multi-instance API, each instance would have a separate bucket; use a shared store such as Redis with an atomic Lua script or a gateway-level distributed limiter when the limit must apply globally.

### Implementation notes for question 490

```csharp
public sealed class FixedWindowCounter
{
    private readonly ConcurrentDictionary<string, (long Window, int Count)> _counts = new();
    private readonly int _limit;
    private readonly TimeSpan _window;

    public FixedWindowCounter(int limit, TimeSpan window)
        => (_limit, _window) = (limit, window);

    public bool TryAcquire(string key)
    {
        var windowId = DateTimeOffset.UtcNow.ToUnixTimeSeconds() / (long)_window.TotalSeconds;
        while (true)
        {
            var old = _counts.GetOrAdd(key, (windowId, 0));
            var next = old.Window == windowId ? (windowId, old.Count + 1) : (windowId, 1);
            if (next.Item2 > _limit) return false;
            if (_counts.TryUpdate(key, next, old)) return true;
        }
    }
}
```
A fixed window is simple but allows a burst around a window boundary. The sample is in-memory and needs key cleanup; a distributed API needs an atomic shared counter with expiry.

### Implementation notes for question 491

```csharp
var channel = Channel.CreateBounded<WorkItem>(new BoundedChannelOptions(100)
{
    FullMode = BoundedChannelFullMode.Wait,
    SingleReader = true,
    SingleWriter = false
});

// Producer
await channel.Writer.WriteAsync(item, cancellationToken);

// Consumer
await foreach (var work in channel.Reader.ReadAllAsync(cancellationToken))
{
    await ProcessAsync(work, cancellationToken);
}
```
A bounded channel provides backpressure: producers wait when the buffer is full. Complete the writer when production ends, and handle cancellation and processing exceptions explicitly.

### Implementation notes for question 492

```csharp
static async Task<T> RetryAsync<T>(
    Func<CancellationToken, Task<T>> operation,
    int maxAttempts, CancellationToken ct)
{
    for (var attempt = 1; ; attempt++)
    {
        try { return await operation(ct); }
        catch (HttpRequestException) when (attempt < maxAttempts)
        {
            var exponentialMs = Math.Min(10_000, 200 * Math.Pow(2, attempt - 1));
            var jitterMs = Random.Shared.Next(0, 200);
            await Task.Delay(TimeSpan.FromMilliseconds(exponentialMs + jitterMs), ct);
        }
    }
}
```
Retry only transient failures and only when the operation is safe to repeat (or protected by idempotency). Respect `Retry-After`, use an overall timeout and avoid retry storms. In production .NET, Polly's resilience pipelines are often preferable to maintaining custom policy code.

### Implementation notes for question 493

```csharp
// Prefer a tested resilience library for production. Conceptual circuit-breaker states:
// Closed: calls pass through; repeated failures open the circuit.
// Open: calls fail fast for a cool-down period.
// Half-open: allow a limited probe; success closes, failure re-opens.
var pipeline = new ResiliencePipelineBuilder<HttpResponseMessage>()
    .AddCircuitBreaker(new HttpCircuitBreakerStrategyOptions
    {
        FailureRatio = 0.5,
        MinimumThroughput = 10,
        SamplingDuration = TimeSpan.FromSeconds(30),
        BreakDuration = TimeSpan.FromSeconds(15)
    })
    .Build();
```
This uses Polly v8 APIs from `Polly` and `Polly.Extensions` packages. Tune thresholds from dependency behaviour and telemetry; a circuit breaker is not a substitute for timeouts.

### Implementation notes for question 494

```csharp
// Pseudocode for the database transaction:
// 1. INSERT IdempotencyKey + request hash under a UNIQUE constraint.
// 2. If the key already exists, return its stored result (if hash matches).
// 3. Execute the payment operation using the same stable business reference.
// 4. Persist the outcome and response against the key.
// 5. Commit atomically where possible.
```
The unique constraint is essential: two concurrent requests with the same key must not both pass an application-only check. Reject reuse of the same key with a different request body. For external payment providers, pass their idempotency key too and reconcile uncertain outcomes.

### Implementation notes for question 495

```csharp
var pageSize = Math.Clamp(request.PageSize, 1, 100);
var query = db.Products.AsNoTracking()
    .Where(p => p.IsActive)
    .OrderBy(p => p.Id);

var total = await query.CountAsync(ct);
var rows = await query.Skip((request.Page - 1) * pageSize)
    .Take(pageSize)
    .Select(p => new ProductDto(p.Id, p.Name, p.Price))
    .ToListAsync(ct);
```
Apply filtering and ordering before pagination, use a deterministic unique ordering, project only required columns, and cap page size. For very deep pages, consider keyset/cursor pagination instead of large offsets.

### Implementation notes for question 496

```csharp
public sealed class CleanupWorker : BackgroundService
{
    private readonly IServiceScopeFactory _scopeFactory;
    private readonly ILogger<CleanupWorker> _logger;

    public CleanupWorker(IServiceScopeFactory scopeFactory,
        ILogger<CleanupWorker> logger)
        => (_scopeFactory, _logger) = (scopeFactory, logger);

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                await using var scope = _scopeFactory.CreateAsyncScope();
                var job = scope.ServiceProvider.GetRequiredService<ICleanupJob>();
                await job.RunAsync(stoppingToken);
            }
            catch (OperationCanceledException) when (stoppingToken.IsCancellationRequested) { break; }
            catch (Exception ex) { _logger.LogError(ex, "Cleanup iteration failed"); }

            await Task.Delay(TimeSpan.FromMinutes(5), stoppingToken);
        }
    }
}
```
Create a scope per iteration because hosted services are singletons and must not directly capture scoped services such as `DbContext`. In a real worker, consider durable queues, graceful shutdown and retry/dead-letter behaviour.

### Implementation notes for question 497

```csharp
await Parallel.ForEachAsync(items,
    new ParallelOptions
    {
        MaxDegreeOfParallelism = 8,
        CancellationToken = ct
    },
    async (item, token) =>
    {
        await ProcessAsync(item, token);
    });
```
Choose the limit based on downstream capacity, not just CPU count. If each operation uses SQL connections, ensure the concurrency limit is compatible with the connection pool and database capacity.

### Implementation notes for question 498

```csharp
// Conceptual Redis lease:
// SET lock:resource unique-token NX PX 10000
// Only the caller receiving OK owns the lease.
// Release with a Lua script that deletes the key only if its value
// still equals unique-token.
```
Never release with an unconditional DEL: the lease may have expired and another process may own the lock. Use a unique token, bounded lease, renewal where justified, and fencing tokens for systems where stale lock holders could still perform writes. For critical correctness, prefer database constraints/transactions when they can enforce the invariant.

### Implementation notes for question 499

```csharp
public static TAccumulate Aggregate<T, TAccumulate>(
    this IEnumerable<T> source,
    TAccumulate seed,
    Func<TAccumulate, T, TAccumulate> accumulator)
{
    ArgumentNullException.ThrowIfNull(source);
    ArgumentNullException.ThrowIfNull(accumulator);

    var result = seed;
    foreach (var item in source)
        result = accumulator(result, item);
    return result;
}
```
This is a single-pass, deferred-source-consuming operation: it executes immediately when called and returns one accumulated value. It does not need to materialize the sequence.

### Implementation notes for question 500

```csharp
await foreach (var batch in db.Records
    .AsNoTracking()
    .Where(x => x.IsReady)
    .OrderBy(x => x.Id)
    .Select(x => new RecordDto(x.Id, x.Payload))
    .AsAsyncEnumerable()
    .ChunkAsync(500, ct))
{
    await ProcessBatchAsync(batch, ct);
}
```
`ChunkAsync` above represents an application helper that yields bounded batches; alternatively use keyset pagination (`Id > lastSeenId`) and fetch a bounded page each time. Avoid `ToListAsync()` over the entire 100,000-row result. Keep each batch small enough to control memory and database load.

## Production scenario playbooks for questions 501–520

### Interview approach for question 501

1. Confirm the latency regression by endpoint, percentile (p50/p95/p99), instance and release version.
2. Check traces to see where time is spent: inbound queue, thread pool, SQL, HTTP dependency, locks or serialization. Normal CPU with piling requests often points to waiting, connection-pool exhaustion, thread-pool starvation or a slow dependency.
3. Compare before/after deployment: configuration, query plans, connection strings, thread-pool blocking, retry behaviour and dependency latency.
4. Inspect thread-pool queue length, active requests, SQL pool usage, HTTP connection pools and timeout/retry counts.
5. Mitigate safely (rollback, disable a problematic feature, reduce concurrency or restore dependency capacity), then fix the root cause and verify p95/p99 under load.

### Interview approach for question 502

Compare the actual SQL and parameters EF sends with the SSMS test. Common causes include different parameter values/types, parameter sniffing, implicit conversions, different SET options, extra round trips, lazy-loading N+1 queries, tracking/materialization cost, client-side projection and connection acquisition time. Enable EF command logging/interceptors, capture the exact SQL and parameter metadata, inspect the actual execution plan and measure database execution separately from end-to-end request time. Do not assume SSMS and the application execute equivalent commands.

### Interview approach for question 503

Capture a memory/GC trace or allocation profile in a representative environment. Identify allocation rate, Gen 0/1/2 collections, LOH allocations and the call stacks responsible for bytes allocated. Look for repeated large strings/arrays, unnecessary LINQ materialization, serialization buffers, per-request object graphs and boxing. Fix the highest-volume allocation sites first; stream or project data, reuse buffers with `ArrayPool<T>` where justified, and avoid pooling objects without evidence. Re-run the same workload and compare allocation rate, GC pause time and throughput.

### Interview approach for question 504

Enforce the stock invariant in the database, not with a read-then-write check in application code. For example, execute an atomic conditional update: `UPDATE Products SET Stock = Stock - 1 WHERE Id = @id AND Stock > 0`; check affected rows. Exactly one request can decrement the last unit. Put the purchase/order changes in a transaction as needed, use a unique order/payment idempotency key, and return an out-of-stock response when no row was updated. A concurrency token/rowversion with retry is another option, but the database must arbitrate the race.

### Interview approach for question 505

Use a database transaction and concurrency control. Add a `rowversion` column (SQL Server) or another concurrency token; both writers read the same version, and the second update detects a conflict instead of silently overwriting the first. Decide whether to reject with a conflict, reload and retry, or merge based on business rules. If the update is an invariant such as inventory decrement, prefer an atomic conditional SQL update. Do not rely on an in-process lock when the API and worker may run on different instances.

### Interview approach for question 506

First establish whether the duplicate is caused by retries, multiple consumers, race conditions or a non-idempotent create path. Inspect correlation IDs, request/message IDs, deployment instance IDs and database audit timestamps. Enforce uniqueness in the database for the business key; make create operations idempotent; use transactions and atomic upserts where appropriate. Check whether each instance runs the same scheduled job or consumes the same queue without coordination. A local static lock cannot coordinate five processes. Test by sending concurrent duplicate requests and replaying messages.

### Interview approach for question 507

This is the dual-write problem: SQL commit and RabbitMQ publish are separate operations, so either can succeed alone. Use the transactional outbox: write the order and an outbox event in the same SQL transaction. A background publisher reads unsent outbox rows, publishes with a stable event ID, waits for broker confirmation, then marks the row sent. If publishing fails, retry later. Consumers must be idempotent because a crash after publish but before marking sent can cause duplicate delivery. Monitor outbox age, retry count and dead-letter handling.

### Interview approach for question 508

Assume at-least-once delivery and make the consumer idempotent. Store the message/event ID in a `ProcessedMessages` table with a unique constraint in the same transaction as the business update. If the ID already exists, acknowledge the duplicate without applying the effect again. Acknowledge only after the transaction commits. For external side effects, use the provider's idempotency mechanism or an outbox/saga. Explain that this gives effectively-once business processing under the defined transaction boundary, not magical exactly-once transport.

### Interview approach for question 509

Choose SQL Server as the source of truth and define the cache consistency policy. On a successful product update, commit the database transaction and invalidate the relevant cache key (or update it with a versioned value). Use short TTLs as a safety net, avoid caching mutable entities blindly, and consider cache-aside with explicit invalidation. Concurrent updates may need version numbers or an outbox-driven invalidation event. Monitor cache hit rate and stale-read incidents. Never let a cache value overwrite newer database data without checking version/ordering.

### Interview approach for question 510

Give each dependency a timeout and bounded concurrency. Retry only transient failures, with exponential backoff and jitter, and only when the operation is idempotent. Use a circuit breaker to stop repeatedly calling an unhealthy dependency, plus bulkheads so one dependency cannot consume all request capacity. Define fallback behaviour only if the business can tolerate it (for example, partial response). Propagate cancellation and an overall request deadline. Record per-dependency latency, timeout, retry and breaker metrics. Do not stack unbounded retries at several layers.

### Interview approach for question 511

Capture the slow query, parameters and actual execution plan. Check whether data volume/distribution changed, statistics are stale, an index is missing or no longer selective, the plan regressed, parameter sniffing occurred, implicit conversions prevent index seeks, or blocking/locks are present. Review logical reads, CPU, duration and wait statistics. Make a targeted index/query change and validate with representative parameter values and load. Avoid blindly adding indexes because they increase write and storage costs.

### Interview approach for question 512

Use an expand-and-contract migration. First add nullable/new columns or new tables in a backward-compatible way; deploy code that can read old and new forms and writes both if needed. Backfill in bounded batches with checkpoints and throttling. Add constraints/indexes online where supported and validate the data. Deploy code that reads the new shape, then remove old columns in a later release after all old application instances are gone. Have a rollback plan and monitor locks, log growth and replication/availability impact.

### Interview approach for question 513

Wrap the three updates in one database transaction so they commit together or roll back together. With EF Core, `SaveChanges` is transactional for a single relational provider call, but multiple separate calls or external systems may require an explicit transaction. Catch the exception, roll back, log with a correlation ID and return an appropriate failure. Do not perform irreversible external calls inside a database transaction; use an outbox or saga for cross-system workflows. Keep transactions short to reduce locking.

### Interview approach for question 514

Inspect connection-pool metrics and connection lifetime. Common causes are connections/readers not disposed, long-running queries, transactions held open while doing HTTP calls, unbounded concurrency, leaked `DbContext` instances, or a sudden increase in connection hold time. Ensure each `DbContext` is scoped and disposed, materialize/consume results promptly, use `await using` where applicable, and never keep a connection open across unrelated work. Check SQL Server session state and application pool settings; increasing pool size may only hide the leak.

### Interview approach for question 515

Do not split services by technical layers or by tables. Map business capabilities and workflows, identify bounded contexts, data ownership, change frequency, team ownership and scaling/security boundaries. Start with a modular monolith or a well-understood seam; extract one capability at a time using the strangler pattern. Define APIs/events and ownership of data. Account for operational cost: network failures, deployment, observability, eventual consistency and distributed transactions. Microservices are justified when independent deployment/ownership/scaling benefits exceed that complexity.

### Interview approach for question 516

Shared databases create schema coupling, coordinated releases, cross-service joins, unclear ownership and the risk that one service bypasses another's business rules. Move incrementally: assign a clear owner to each data domain, expose APIs/events, create service-owned schemas/databases, and migrate reads/writes using an expand-and-contract or strangler approach. Use CDC/outbox or controlled replication for read models where needed. Avoid a big-bang split and do not replace shared tables with synchronous chatty calls for every query.

### Interview approach for question 517

Separate ingestion from delivery. Accept requests through an API, validate and persist notification intent, then publish jobs to a durable broker. Partition queues by channel/priority/tenant if required; use horizontally scaled workers with bounded concurrency and provider-specific rate limits. Store templates/preferences and delivery state; make each notification idempotent. Retry transient provider errors with backoff, dead-letter permanent failures, and track status. Apply user consent, unsubscribe rules, quiet hours and privacy controls. Monitor queue age, throughput, delivery rate, failures and cost.

### Interview approach for question 518

Use an upload API that validates file size/type and authorisation, streams content to object storage, and stores metadata/version records in a relational database. Scan files for malware and calculate checksums. Keep document bytes outside the relational database unless requirements justify otherwise. Use a search/indexing pipeline asynchronously; store index status and retry failures. Enforce tenant/document-level access checks on every read/download/search result. Version documents immutably, audit access and changes, encrypt data at rest/in transit, and use signed short-lived download URLs where appropriate.

### Interview approach for question 519

Capture audit events at the business-operation boundary, not only from generic HTTP logs. Record actor/service identity, action, target entity, timestamp, correlation/trace ID, outcome and carefully selected before/after values. Write the business change and an outbox audit event in one transaction, then send it to append-oriented durable storage. Restrict write/delete permissions, protect sensitive fields, define retention and tamper-evidence requirements, and ensure audit failures have an explicit policy. Do not store passwords, tokens or unnecessary personal data.

### Interview approach for question 520

Start by clarifying what 100,000 concurrent means: connected clients, active requests per second, latency SLO, geography and workload mix. Use stateless API instances behind a load balancer; scale horizontally based on measured CPU, memory, request and queue metrics. Use asynchronous I/O, bounded work queues, caching for read-heavy data, database indexes/read replicas where appropriate, and partitioning only when justified. Protect dependencies with rate limits, timeouts, bulkheads and backpressure. Load-test realistic traffic, identify the actual bottleneck, and plan capacity, failure zones, observability and graceful degradation.

---

## A reliable structure for answering technical interview questions

For a concept question:
1. Define the concept in one or two sentences.
2. Explain how it works.
3. Give a small example from an API or service.
4. Mention a limitation or trade-off.

For a design or production scenario:
1. Clarify the requirement and the invariant that must never be violated.
2. Identify likely failure points and gather evidence before changing code.
3. Explain the proposed design and why it fits.
4. Cover concurrency, retries, timeouts, security and data consistency where relevant.
5. Explain how you would test, monitor and roll back the change.

**Interview tip:** Do not memorise paragraphs word for word. Understand the mechanism, then explain it naturally. If you have implemented something, describe your own implementation, the problem it solved, the trade-off you made and what you would improve next.
