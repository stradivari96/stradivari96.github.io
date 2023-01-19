---
title: "🗺️ Personal Roadmap"
date: 2023-01-01
draft: false
weight: 2
TocOpen: true

tags: ["Software Engineering"]
---

References and things to learn.

<!--more-->

- [Backend Roadmap](https://roadmap.sh/backend/)
- [Architect Roadmap](https://roadmap.sh/software-architect/)

---

## General books

- 📚[The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)
- 📚[Software Engineering at Google](https://abseil.io/resources/swe-book)
- 📝[Clean Code](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29): read a summary, avoid the book

---

## Version Control (Git)

✨[Cheatsheet](https://training.github.com/downloads/github-git-cheat-sheet/)

- 📺[Git for Poets](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6ZF9C0YMKuns9sLDzK6zoiV)
- 🔒️[Generate and add SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- 📝[Github Quickstart](https://docs.github.com/en/get-started/quickstart/hello-world)
- 🌐[Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) / [Github Flow](https://docs.github.com/en/get-started/quickstart/github-flow)
  / [Gitmoji](https://gitmoji.dev/)

{{< notice info >}}
💩 https://ohshitgit.com/
{{< /notice >}}

---

## Python [💼](https://www.linkedin.com/jobs/search/?keywords=python&location=Spain)

https://lp.jetbrains.com/python-developers-survey-2021

### Basics

✨[Cheatsheet](https://gto76.github.io/python-cheatsheet/)

Download [Python](https://www.python.org/downloads/) and [Visual Studio Code](https://code.visualstudio.com/) with the Python extension.

Start with **single file scripts**.

- 📚[Introducing Python (2019)](https://www.oreilly.com/library/view/introducing-python-2nd/9781492051374/): Basics, ignore everything after chapter 11
- 📝[Basic OOP](https://realpython.com/python3-object-oriented-programming/)
- 🥋Practice [**Codewards**](codewars.com/): checkout **other solutions** after submitting
- 🎨Skim through [pep8](https://peps.python.org/pep-0008/)
- ⚡️Learn a bit of DSA (Data Structures and Algorithms): do **easy** in [neetcode](https://neetcode.io/roadmap)

{{< notice note >}}
Project ideas:

- Make a discord bot with [discord.py](https://discordpy.readthedocs.io/)
- Program a simple game with [PyGame](https://www.pygame.org/docs/) such as [Snake](https://www.geeksforgeeks.org/snake-game-in-python-using-pygame-module/)
- Deploy a simple [FastAPI](https://fastapi.tiangolo.com/) website in [Deta](https://www.deta.sh/) (maybe using [Riot API](https://developer.riotgames.com/))

{{< /notice >}}

### Advanced

Switch to [PyCharm](https://www.jetbrains.com/pycharm/) once you are familiar with the syntax and concepts such as virtualenvs, git, etc.

- 📚Read:
  - [Fluent Python (2022)](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/):
    comprehensions, dataclasses, Type Hints, Protocol, etc.
  - [Architecture Patterns with Python](https://www.cosmicpython.com/book/preface.html):
    DDD, CQRS, Event-Driver Architecture, etc.
- 📺Watch:
  - [ArjanCodes](https://www.youtube.com/@ArjanCodes/videos): Advanced, refactoring videos are great.
  - [mCoding](https://www.youtube.com/channel/UCaiL2GDNpLYH6Wokkk1VNcg): Trivia, watch popular videos.
- 📝[StackOverflow Questions](https://stackoverflow.com/questions/tagged/python?sort=votes):
  should know the answers to the most voted questions.

{{< notice info>}}
Use [venv](https://docs.python.org/3/library/venv.html) and [pip-tools](https://github.com/jazzband/pip-tools) for dependency management,
[poetry](https://python-poetry.org/) is also a good option, specially for packaging.
Avoid [pipenv](https://github.com/pypa/pipenv).
{{< /notice >}}

- ⚡️Concurrency:
  - [Why Learn Python Concurrency](https://superfastpython.com/why-learn-python-concurrency/)
  - [How to Choose the Right Concurrency API](https://superfastpython.com/python-concurrency-choose-api/)
- 🧪Learn about [testing](https://testdriven.io/guides/complete-python/)
- 🔄Task queues: [Celery](https://docs.celeryq.dev/) / [arq](https://arq-docs.helpmanual.io/)
- 🔧CI Tools ([sample precommit](https://github.com/arrow-py/arrow/blob/74a759b88447b6ecd9fd5de610f272c8fb6130a2/.pre-commit-config.yaml)):
  [MyPy](https://mypy.readthedocs.io/en/stable/),
  [Black](https://black.readthedocs.io/en/stable/),
  [isort](https://pycqa.github.io/isort/),
  [flake8](https://flake8.pycqa.org/en/latest/),
  [pip-audit](https://github.com/pypa/pip-audit),
  [bandit](https://bandit.readthedocs.io/en/latest/).
- 🧰Learn about [Design Patterns](https://refactoring.guru/design-patterns) in [python](https://github.com/faif/python-patterns), don't go too crazy.
- 👀Checkout some Open Source projects, better if you have used them before:
  - [arrow](https://github.com/arrow-py/arrow): highly recommended
  - [requests](https://github.com/psf/requests) and [httpx](https://github.com/encode/httpx): probably familiar with them
  - [arq](https://github.com/samuelcolvin/arq): small modern project, take a good look at every file

{{< notice tip >}}

- [Latest Python features](https://docs.python.org/3/whatsnew/index.html): read summaries
- [Reddit](https://www.reddit.com/r/Python/top/?t=month): Check top posts by year or month
- [Top libraries](https://tryolabs.com/blog/2022/12/26/top-python-libraries-2022): by year

{{< /notice >}}

{{< notice note >}}
[Leetcode](https://leetcode.com/) every day keeps unemployment away
{{< /notice >}}

---

## Terminal

- 📝[Command Line Crash Course](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line)
- 📚[The Linux Command Line](https://nostarch.com/tlcl2)

---

## Databases

### SQL

ORMs are great, but you should know how to use raw SQL.

- 📚[Practical SQL](https://www.practicalsql.com/)
- 📝[w3schools](https://www.w3schools.com/sql/)
- 🥋[Lintcode practice](https://www.lintcode.com/problem/?typeId=3)

### NoSQL

- 📗[MongoDB Manual](https://www.mongodb.com/docs/manual)
- 📕[Redis Documentation](https://redis.io/docs/data-types/tutorial/)

---

## Containerization

- 🐋[Docker for beginners](https://testdriven.io/blog/docker-for-beginners/)
- 🎨[Docker Best Practices](https://testdriven.io/blog/docker-best-practices/)
- ☸️[Kubernetes Flask Guide](https://testdriven.io/blog/running-flask-on-kubernetes/)

---

## Web Development

### Basics

- [How does the Internet work?](https://tutorial.djangogirls.org/en/how_the_internet_works/)
- [What is a web server?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
- [What is HTTP?](https://www.cloudflare.com/en-gb/learning/ddos/glossary/hypertext-transfer-protocol-http/)
- [Inspect network activity](https://developer.chrome.com/docs/devtools/network/)

### Advanced

- [Twelve-factor app](https://12factor.net/)

### Web Frameworks & Examples

#### Python

- 🐉[Django](https://www.djangoproject.com/): [tutorial](https://tutorial.djangogirls.org/en/) for beginners
  - [readthedocs](https://github.com/readthedocs/readthedocs.org): documentation hosting
  - [sentry](https://github.com/getsentry/sentry): monitoring platform
  - [old mdn](https://github.com/mdn/kuma): kuma, old MDN
  - [guya.moe](https://github.com/subject-f/guyamoe): manga reading website
- ⚗️[Flask](https://flask.palletsprojects.com/): [tutorial](https://flask.palletsprojects.com/tutorial/) for beginners
  - [SimpleLogin](https://github.com/simple-login/app)
  - [Apache Airflow](https://github.com/apache/airflow)
  - [redash.io](https://github.com/getredash/redash)
  - [sourcehut](https://git.sr.ht/~sircmpwn/core.sr.ht/tree)
- ⚡️[FastAPI](https://fastapi.tiangolo.com/):
  - [TermPair](https://github.com/cs01/termpair)
  - [Dispatch](https://github.com/Netflix/dispatch)
  - [Mealie](https://github.com/hay-kot/mealie)
  - [Opal](https://github.com/permitio/opal)

#### Rust

- ⚙️[Actix](https://actix.rs/): [tutorial](https://actix.rs/docs/getting-started) for beginners
  - [miniserve](https://github.com/svenstaro/miniserve/)
  - [mdn](https://github.com/mdn/rumba): superseeds kuma
- [axum](https://github.com/tokio-rs/axum)

#### Go

TODO

#### Java

TODO

---

## More Languages

### JavaScript [💼](https://www.linkedin.com/jobs/search/?keywords=nodejs&location=Spain)

https://stateofjs.com/en-us/

### Rust [💼](https://www.linkedin.com/jobs/search/?keywords=rust&location=Spain)

https://www.jetbrains.com/lp/devecosystem-2021/rust/

### Go [💼](https://www.linkedin.com/jobs/search/?keywords=Golang&location=Spain)

https://www.jetbrains.com/lp/devecosystem-2021/go/

### Java [💼](https://www.linkedin.com/jobs/search/?keywords=java&location=Spain)

https://www.jetbrains.com/lp/devecosystem-2021/java/

---
