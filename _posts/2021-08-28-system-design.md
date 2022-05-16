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

- https://github.com/donnemartin/system-design-primer
- http://highscalability.squarespace.com/blog/category/example

## Examples:

- Twitter: [Primer](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/twitter/README.md), [CodelyTV](https://youtu.be/6o0usvW5bqY), [Exponent](https://youtu.be/QF8JNSoJD8E)
  - Fanout
- Pastebin: [Primer](https://github.com/donnemartin/system-design-primer/blob/master/solutions/system_design/pastebin/README.md)
- Dropbox: [Stanford](https://youtu.be/PE4gwstWhmc),
- TikTok: [Exponent](https://youtu.be/Z-0g_aJL5Fw)
- Instagram: [Exponent](https://www.youtube.com/watch?v=VJpfO6KdyWE)

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

## [Blogs]

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
