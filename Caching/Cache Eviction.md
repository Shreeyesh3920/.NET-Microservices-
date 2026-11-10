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
