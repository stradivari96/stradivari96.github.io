---
title: System Design notes
excerpt: "Some notes about system design"
read_time: true
share: true
toc: true
related: true
comments: true
tags: [System Design, Software Engineering, Programming]
---

- https://github.com/donnemartin/system-design-primer
- https://www.youtube.com/c/ExponentTV/videos

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
