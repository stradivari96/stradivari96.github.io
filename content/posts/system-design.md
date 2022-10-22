---
title: "📝 System Design notes"
date: 2021-08-28
draft: false

aliases:
  - /system-design

tags: ["Software Engineering"]
---

Some notes about system design

<!--more-->

## References

- [System design primer](https://github.com/donnemartin/system-design-primer)
- [High Scalability](http://highscalability.squarespace.com/blog/category/example)
- [Designing data intensive](https://www.goodreads.com/book/show/23463279-designing-data-intensive-applications?ref=nav_sb_ss_1_15)
- [System design interview](https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide)

### Scaling

- NoSQL for:
  - low latency
  - unstructured or non relational data
  - only need to serialize and deserialize (json, xml, yaml, etc)
  - store massive amount of data
  - consistency is not a big deal, consistent scaling.
- Cache:
  - read frequenly, modified infrequently
  - implement expiration policy
  - consistency is hard for multiple regions
  - eviction policy: LRU, LFU, FIFO
- CDN:
  - Dynamic content caching??
  - Cache static content near the user
  - The origin returns TTL to describe how long to cache.
- Move session data out of web servers and into a cache / NoSQL.
- Message queue for async tasks, pub/sub.
- Sharding: choose a good sharding key / partition key
  - reshard when a single shard is not enough, have exhausted shards due to uneven distribution.
  - beware of celebrity problem, what if Justin Bieber and Lady gaga are in the same shard.
  - joins are hard in sharded databases, try denormalizing.

### Back of the envelope estimation

Powers of two

```
Power           Exact Value         Approx Value        Bytes
---------------------------------------------------------------
7                             128
8                             256
10                           1024   1 thousand           1 KB
16                         65,536                       64 KB
20                      1,048,576   1 million            1 MB
30                  1,073,741,824   1 billion            1 GB
32                  4,294,967,296                        4 GB
40              1,099,511,627,776   1 trillion           1 TB
```

Latency

```
Latency Comparison Numbers
--------------------------
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                      14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                      20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy            10,000   ns       10 us
Send 1 KB bytes over 1 Gbps network     10,000   ns       10 us
Read 4 KB randomly from SSD*           150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
HDD seek                            10,000,000   ns   10,000 us   10 ms  20x datacenter roundtrip
Read 1 MB sequentially from 1 Gbps  10,000,000   ns   10,000 us   10 ms  40x memory, 10X SSD
Read 1 MB sequentially from HDD     30,000,000   ns   30,000 us   30 ms 120x memory, 30X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

Notes
-----
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns
```

![](https://camo.githubusercontent.com/77f72259e1eb58596b564d1ad823af1853bc60a3/687474703a2f2f692e696d6775722e636f6d2f6b307431652e706e67)

| Availability % | Downtime per day    | Downtime per year |
| -------------- | ------------------- | ----------------- |
| 99%            | 14.40 minutes       | 3.65 days         |
| 99.9%          | 1.44 minutes        | 8.77 hours        |
| 99.99%         | 8.64 seconds        | 52.60 minutes     |
| 99.999%        | 864.00 milliseconds | 5.26 minutes      |
| 99.9999%       | 86.40 milliseconds  | 31.56 seconds     |

## [The Twelve-Factor App](https://12factor.net/)

1. **Codebase**: Use VCS (git), one codebase per app. Refactor shared code into libraries.
2. **Dependencies**: Declare and isolate dependencies, vendor if necessary.
3. **Config**: Use environment variables, do not group them together.
4. **Backing services**: Treat them (MySQL, RabbitMQ) as attached resources via config.
5. **Build, release, run**: Strict separation between these stages, releases cannot be mutated and are append-only.
6. **Processes**: Processes are stateless and share-nothing. Implement sticky sessions using Memcached or Redis.
7. **Port binding**: Export services via port binding.
8. **Concurrency**: Scale out via the process model horizontally.
9. **Disposability**: Fast startup and graceful shutdown using disposable processes.
10. **Dev/prod parity**: Keep development, staging, and production as similar as possible, continuous deployment.
11. **Logs**: Print unbuffered to stdout and should be captured by the executing environment.
12. **Admin processes**: Run admin/management tasks as one-off processes.

## [Scalability for Dummies](https://www.lecloud.net/tagged/scalability)

- Hide servers behing a load balancer.
- Servers does not store any user related data, store sessions in an external cache.
- Denormalize and use NoSQL + cache
- Try to retrieve data from cache and use database on miss.
- Cache objects (sessions, articles, activity streams, user relations) instead of queries.
- Async
  - Precomputing (pre-render static html for CDN)
  - Job queues

## Examples:

- Twitter: [Primer](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/twitter/README.md), [CodelyTV](https://youtu.be/6o0usvW5bqY), [Exponent](https://youtu.be/QF8JNSoJD8E)
  - Fanout
- Pastebin: [Primer](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README.md)
- Dropbox: [Stanford](https://youtu.be/PE4gwstWhmc),
- TikTok: [Exponent](https://youtu.be/Z-0g_aJL5Fw)
- Instagram: [Exponent](https://www.youtube.com/watch?v=VJpfO6KdyWE)

## Blogs

- [Airbnb](https://medium.com/airbnb-engineering/airbnb-engineering-infrastructure/home)
- [Cloudflare](https://blog.cloudflare.com/)
- [Discord](https://discord.com/category/engineering)
- [DoorDash](https://doordash.engineering/blog/)
- [Dropbox](https://dropbox.tech/infrastructure)
- [Linkedin](https://engineering.linkedin.com/blog)
- [Meta](https://engineering.fb.com/)
- [Pinterest](https://medium.com/pinterest-engineering)
- [Riot](https://technology.riotgames.com/)
- [Stripe](https://stripe.com/blog/engineering)
- [Twitter](https://blog.twitter.com/engineering/en_us)
- [Uber](https://eng.uber.com/)
