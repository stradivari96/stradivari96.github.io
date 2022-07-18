---
title: 📝 System Design notes
excerpt: "Some notes about system design"
read_time: true
share: true
toc: true
related: true
comments: true
tags: [System Design, Software Engineering, Programming]
---

- [System design primer](https://github.com/donnemartin/system-design-primer)
- [High Scalability](http://highscalability.squarespace.com/blog/category/example)
- [Designing data intensive](https://www.goodreads.com/book/show/23463279-designing-data-intensive-applications?ref=nav_sb_ss_1_15)

## [System design interview](https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide)
### Scaling
- NoSQL for:
  - low latency
  - unstructured or non relational data
  - only need to serialize and deserialize (json, xml, yaml, etc)
  - store massive amount of data
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
- TODO


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

- [Cloudflare](https://blog.cloudflare.com/)
- [Uber](https://eng.uber.com/)
- [Airbnb](https://medium.com/airbnb-engineering/airbnb-engineering-infrastructure/home)
- [Discord](https://discord.com/category/engineering)
- [Twitter](https://blog.twitter.com/engineering/en_us)
- [Meta](https://engineering.fb.com/)
- [Linkedin](https://engineering.linkedin.com/blog)
- [Pinterest](https://medium.com/pinterest-engineering)
- [Dropbox](https://dropbox.tech/infrastructure)
- [Stripe](https://stripe.com/blog/engineering)
- [Riot](https://technology.riotgames.com/)
