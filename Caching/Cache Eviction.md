Cache eviction is the process of removing data from a cache when it becomes full to make space for new or more relevant data.

Imagine Redis has limited RAM. You keep adding products to the cache, and eventually Redis reaches its configured memory limit (maxmemory).
Ab Redis ko decide karna padega ki kaunsa cached data remove karna hai. This process is called Cache Eviction.

<img width="1300" height="630" alt="image" src="https://github.com/user-attachments/assets/d4693cb2-0790-489d-a1de-51df7a1d77a8" />

<img width="1300" height="630" alt="image" src="https://github.com/user-attachments/assets/63d1923e-2c79-43ca-be50-7aa90ba3112b" />

LRU – Least Recently Used

Removes the key that hasn't been accessed for the longest time.

Example: Product A was accessed 10 hours ago, Product B was accessed 2 minutes ago. Product A is a stronger eviction candidate.


LFU – Least Frequently Used

Removes keys that have been accessed less frequently.

Example: Product A was accessed 2 times, Product B was accessed 500 times. Product A is a stronger eviction candidate.

Suppose we have three cache entries:
Key	Access count	Last accessed
A	100 times	2 hours ago
B	5 times	1 minute ago
C	20 times	10 minutes ago

Now the cache is full, and we need to insert D.
LRU eviction
Evicts A
A was accessed the longest time ago, even though it has the highest access count.

LFU eviction
Evicts B
B has the lowest access count, even though it was accessed recently.


### LRU Cache – Simple C# Solution

```
public class LRUCache
{
    int capacity;
    Dictionary<int, LinkedListNode<(int key, int value)>> cache = new();
    LinkedList<(int key, int value)> list = new();

    public LRUCache(int capacity)
    {
        this.capacity = capacity;
    }

    public int Get(int key)
    {
        if (!cache.ContainsKey(key))
            return -1;

        var node = cache[key];

        list.Remove(node);
        list.AddFirst(node);

        return node.Value.value;
    }

    public void Put(int key, int value)
    {
        if (cache.ContainsKey(key))
        {
            var node = cache[key];
            node.Value = (key, value);

            list.Remove(node);
            list.AddFirst(node);
            return;
        }

        if (cache.Count == capacity)
        {
            var last = list.Last;

            cache.Remove(last.Value.key);
            list.RemoveLast();
        }

        cache[key] = list.AddFirst((key, value));
    }
}
```

### One test case

```
LRUCache cache = new LRUCache(2);

cache.Put(1, 10);
cache.Put(2, 20);

Console.WriteLine(cache.Get(1)); // 10

cache.Put(3, 30);                // Evicts key 2

Console.WriteLine(cache.Get(2)); // -1
Console.WriteLine(cache.Get(3)); // 30
Console.WriteLine(cache.Get(1)); // 10
```

Expected output:

```
10
-1
30
10
```

Complexity: Get = O(1) average, Put = O(1) average.
