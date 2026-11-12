<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d173f1a6-9826-4da6-9cc1-167b00360a01" />

# Rate Limiting Algorithms – Detailed Notes

# 1. Fixed Window Counter
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/201faa23-bac5-4183-b8fe-4b6ad015f669" />

## What is Fixed Window?

Fixed Window is a rate-limiting algorithm that maintains a counter to track the number of requests received within a fixed time period.

When the time period ends, the counter resets to zero.

**Example requirement:**
- Allow maximum 5 requests.
- Time window: 10 minutes.

## How does it work?

Imagine our API starts a new 10-minute window at 10:00 AM.

| Request | Request Time | Counter | Result |
|---|---|---:|---|
| 1 | 10:00 | 1 | Allowed |
| 2 | 10:01 | 2 | Allowed |
| 3 | 10:02 | 3 | Allowed |
| 4 | 10:03 | 4 | Allowed |
| 5 | 10:04 | 5 | Allowed |
| 6 | 10:05 | 5 | Rejected |
| 7 | 10:10 | 1 (new window) | Allowed |

### Step-by-step explanation

**Step 1:** At 10:00 AM, the first request arrives.

- Create a counter.
- Set counter = 1.
- Start the 10-minute window.

**Step 2:** More requests arrive.

- Every request increases the counter.
- Counter becomes 2, 3, 4 and 5.
- All five requests are allowed.

**Step 3:** The sixth request arrives.

- Current counter = 5.
- Maximum limit = 5.
- Reject the request with HTTP 429 (Too Many Requests).

**Step 4:** At 10:10 AM.

- The old window expires.
- Counter resets.
- A new 10-minute window starts when the next request arrives.

## What is the disadvantage?

Fixed Window has a boundary problem.

Imagine:

- 5 requests arrive between 10:09 and 10:10.
- Another 5 requests arrive between 10:10 and 10:11.

All 10 requests can be accepted within approximately 2 minutes, even though our limit is 5 requests per 10 minutes.

Why?

Because the requests fall into two different fixed windows.

## Advantages

- Very simple to implement.
- Requires very little storage.
- Easy to maintain using a counter and expiration time.
- Good for basic API rate limiting.

## Disadvantages

- Can allow sudden bursts around window boundaries.
- Doesn't always provide a smooth limit over the last 10 minutes.

**Remember:** Fixed Window = Counter + Fixed Time + Reset.

---

# 2. Sliding Window Log
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/cd50292c-ecb1-4377-bda7-188bf1a053bd" />

## What is Sliding Window Log?

Sliding Window Log is an algorithm that stores the timestamp of every incoming request.

Whenever a new request arrives:

1. Remove timestamps older than the allowed time window.
2. Count the remaining timestamps.
3. If the count is below the limit, allow the request and store its timestamp.
4. Otherwise, reject the request.

Unlike Fixed Window, it always checks the last 10 minutes from the current request time.

**Example requirement:**
- Allow maximum 5 requests.
- Time window: Last 10 minutes.

## How does it work?

Suppose requests arrive at these times:

| Request | Time | Stored timestamps (before new request) | Result |
|---|---|---|---|
| 1 | 10:00 | Empty | Allowed |
| 2 | 10:01 | 10:00 | Allowed |
| 3 | 10:02 | 10:00, 10:01 | Allowed |
| 4 | 10:05 | 10:00, 10:01, 10:02 | Allowed |
| 5 | 10:06 | 10:00, 10:01, 10:02, 10:05 | Allowed |
| 6 | 10:07 | 10:00, 10:01, 10:02, 10:05, 10:06 | Rejected |

At 10:07, all five previous requests are within the last 10 minutes.

Therefore, the sixth request is rejected.

### Now understand why it is called Sliding Window.

Suppose the next request arrives at 10:11 AM.

The algorithm checks the last 10 minutes:

```text
Current time: 10:11 AM

Last 10 minutes:
10:01 ─────────────────── 10:11

Stored requests:
10:01
10:02
10:05
10:06
```

The request at 10:00 is now older than 10 minutes, so it is removed.

The remaining count is 4.

Therefore:

- New request arrives.
- Current count = 4.
- Maximum limit = 5.
- Request is allowed.
- Its timestamp (10:11) is stored.

### Step-by-step logic

**Step 1:** Receive a new request.

**Step 2:** Get the current timestamp.

**Step 3:** Remove all timestamps older than 10 minutes.

**Step 4:** Count the remaining timestamps.

**Step 5:**
- If count is 5 or more → Reject.
- If count is less than 5 → Allow and store the new timestamp.

## Advantages

- More accurate than Fixed Window.
- Checks requests within the actual last 10 minutes.
- Prevents the fixed-window boundary burst problem.
- Provides fairer request limiting.

## Disadvantages

- Must store individual request timestamps.
- Uses more memory than Fixed Window.
- Requires more processing to remove old timestamps and calculate the count.

**Remember:** Sliding Window Log = Store timestamps + Remove old timestamps + Count remaining requests.

---

# 3. Leaky Bucket
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/bfb2db12-a1c3-4067-8294-34ad674b96f2" />

## What is Leaky Bucket?

Leaky Bucket is an algorithm that processes incoming requests at a fixed rate.

Imagine a bucket containing water:

- Incoming requests are like water entering the bucket.
- The bucket has a limited capacity.
- Requests leave the bucket at a fixed speed.
- If the bucket is full, new requests are rejected.

The main purpose is to smooth out sudden traffic spikes.

## Example

Suppose:

- Bucket capacity = 3 requests.
- Processing rate = 1 request every 2 minutes.

This means we can hold a maximum of 3 waiting requests, and process one request every 2 minutes.

## How does it work?

Imagine requests arrive quickly:

| Time | Event | Queue | Result |
|---|---|---|---|
| 10:00 | Request 1 arrives | [R1] | Accepted |
| 10:00:30 | Request 2 arrives | [R1, R2] | Accepted |
| 10:01 | Request 3 arrives | [R1, R2, R3] | Accepted |
| 10:01:30 | Request 4 arrives | [R1, R2, R3] | Rejected (full) |
| 10:02 | Process R1 | [R2, R3] | Processed |
| 10:04 | Process R2 | [R3] | Processed |
| 10:06 | Process R3 | [] | Processed |

### Step-by-step explanation

**Step 1:** Request 1 arrives.

- Add it to the bucket.
- Queue contains R1.

**Step 2:** Request 2 and Request 3 arrive quickly.

- Add both requests to the queue.
- Queue is now full.

**Step 3:** Request 4 arrives.

- Bucket capacity is 3.
- All three slots are occupied.
- Reject Request 4.

**Step 4:** At 10:02.

- Process the first request.
- Remove R1 from the queue.
- One slot becomes available.

**Step 5:** Continue processing.

- Process R2 at 10:04.
- Process R3 at 10:06.
- Requests are processed at a consistent rate.

## What is the main benefit?

Imagine your API suddenly receives 100 requests.

Instead of processing all 100 simultaneously:

- Requests enter a queue.
- The application processes them at a controlled speed.
- Excess requests are rejected if the queue is full.

This helps prevent sudden traffic spikes from overwhelming downstream systems.

## Advantages

- Smooths sudden traffic spikes.
- Processes requests at a predictable rate.
- Protects backend services from excessive simultaneous work.

## Disadvantages

- Requests may have to wait in a queue.
- Adds latency.
- Requests are rejected when the queue is full.
- Requires queue management.

**Remember:** Leaky Bucket = Queue + Fixed Processing Speed.

Note: Some implementations use a leaky-bucket rate limiter without actually maintaining a waiting queue. They simply reject requests that exceed the permitted processing rate. The queued version above demonstrates the traffic-shaping concept.

---

# 4. Comparison of all three algorithms

| Feature | Fixed Window | Sliding Window Log | Leaky Bucket |
|---|---|---|---|
| Main mechanism | Counter | Timestamps | Queue |
| Tracks | Number of requests | Request timestamps | Waiting requests |
| Time behaviour | Fixed intervals | Rolling time period | Fixed processing rate |
| Allows bursts? | Yes, especially at boundaries | Limited by rolling count | Accepts bursts up to queue capacity |
| Memory usage | Low | Higher | Depends on queue size |
| Main purpose | Limit request count | Accurately limit request count | Smooth incoming traffic |

# 5. When should we use each?

### Fixed Window
Use when:
- You need a simple request limit.
- Small boundary bursts are acceptable.
- You want easy implementation.

Example: Maximum 100 API requests per minute.

### Sliding Window Log
Use when:
- You need an accurate limit over the last N minutes.
- You want to avoid fixed-window boundary bursts.

Example: Maximum 5 Forgot Password requests per email in the last 10 minutes.

### Leaky Bucket
Use when:
- You want to process requests at a consistent rate.
- You want to protect a backend service from sudden spikes.

Example: Processing document-generation jobs at a controlled rate.

# 6. Quick interview revision

**Fixed Window:** Maintains a counter and resets it after a fixed time period.

**Sliding Window Log:** Stores request timestamps and removes older timestamps to calculate how many requests occurred in the last time period.

**Leaky Bucket:** Places incoming requests in a limited queue and processes them at a fixed rate. If the queue is full, new requests are rejected.

## One-line memory trick

- Fixed Window → Counter.
- Sliding Window Log → Timestamps.
- Leaky Bucket → Queue.

