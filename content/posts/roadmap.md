---
title: "🗺️ Personal Roadmap"
date: 2023-01-01
draft: false
weight: 2

tags: ["Software Engineering"]
---

🚧 WORK IN PROGRESS

References and things to learn.

<!--more-->

- [Backend Roadmap](https://roadmap.sh/backend/)
- [Architect Roadmap](https://roadmap.sh/software-architect/)

---

## General books

- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)
- [Software Engineering at Google](https://abseil.io/resources/swe-book)
- [Clean Code](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29): read a summary, avoid the book

---

## Python

✨[Cheatsheet](https://gto76.github.io/python-cheatsheet/)

### Basics

Download [Python](https://www.python.org/downloads/) and [Visual Studio Code](https://code.visualstudio.com/) with the Python extension.

Start with **single file scripts**.

- 📚[Introducing Python (2019)](https://www.oreilly.com/library/view/introducing-python-2nd/9781492051374/): Basics, ignore everything after chapter 11
- 📚[Basic OOP](https://realpython.com/python3-object-oriented-programming/)
- 🥋Practice [**Codewards**](codewars.com/): checkout **other solutions** after submitting
- 🎨Skim through [pep8](https://peps.python.org/pep-0008/)
- ⚡️Learn a bit of DSA (Data Structures and Algorithms): do **easy** in [neetcode](https://neetcode.io/roadmap)

{{< notice note >}}
Project ideas:

- Make a discord bot with [discord.py](https://discordpy.readthedocs.io/)
- Program a simple game with [PyGame](https://www.pygame.org/docs/) such as [Snake](https://www.geeksforgeeks.org/snake-game-in-python-using-pygame-module/)
- Deploy a simple [Flask](https://flask.palletsprojects.com/en/2.2.x/) website in [Deta](https://www.deta.sh/) (maybe using [Riot API](https://developer.riotgames.com/))

{{< /notice >}}

### Advanced

Switch to [PyCharm](https://www.jetbrains.com/pycharm/) once you are familiar with the syntax and concepts such as virtualenvs, git, etc.

- 📚Read:
  - [Fluent Python (2022)](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/):
    comprehensions, dataclasses, Type Hints, Protocol, etc.
  - [Architecture Patterns with Python](https://www.cosmicpython.com/book/preface.html):
    DDD, CQRS, Event-Driver Architecture, etc.
  - [StackOverflow Questions](https://stackoverflow.com/questions/tagged/python?sort=votes):
    should know the answers to the most voted questions.
- 📺Watch:
  - [ArjanCodes](https://www.youtube.com/@ArjanCodes/videos): Advanced, refactoring videos are great.
  - [mCoding](https://www.youtube.com/channel/UCaiL2GDNpLYH6Wokkk1VNcg): Trivia, watch popular videos.

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
- 🧰Learn about [Design Patterns](https://refactoring.guru/design-patterns) and their implementation in [python](https://github.com/faif/python-patterns), don't overdo it.
- 📜Checkout some Open Source projects, better if you have used them before:
  - [arrow](https://github.com/arrow-py/arrow): highly recommended
  - [minecraft](https://github.com/fogleman/Minecraft): 900 lines
  - [requests](https://github.com/psf/requests) and [httpx](https://github.com/encode/httpx)
  - [arq](https://github.com/samuelcolvin/arq): small project, take a good look at every file
  - [typer](https://github.com/tiangolo/typer)

{{< notice note >}}
News:

- [Latest Python features](https://docs.python.org/3/whatsnew/index.html): read summaries
- [Reddit](https://www.reddit.com/r/Python/top/?t=month): Check top posts by year or month
- [Top libraries](https://tryolabs.com/blog/2022/12/26/top-python-libraries-2022): by year

{{< /notice >}}

---

## Version Control (Git)

- 🔒️[Generate and add SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- 📚[Github Quickstart](https://docs.github.com/en/get-started/quickstart/hello-world)
- ✨[Cheatsheet](https://training.github.com/downloads/github-git-cheat-sheet/)
- 🌐Conventions: [Conventional](https://www.conventionalcommits.org/) / [Gitmoji](https://gitmoji.dev/)

```
git checkout develop
git pull
git checkout -b feature/new-feature
git add .
git commit -m ":sparkles: Some new thing"
git push
```

{{< notice info >}}
💩 https://ohshitgit.com/
{{< /notice >}}

---

## Web Development

https://wizardzines.com/comics/

### Basics

- [How does the Internet work?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work)
- [What is a web server?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
- [What is a domain name?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_domain_name)
- [What is HTTP?](https://www.cloudflare.com/en-gb/learning/ddos/glossary/hypertext-transfer-protocol-http/)
- [What is DNS?](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)
- [How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)
- [Inspect network activity](https://developer.chrome.com/docs/devtools/network/)

### HTML, CSS & JS

TODO

### Web Frameworks

- [FastAPI](https://fastapi.tiangolo.com/)
- [Django](https://www.djangoproject.com/)
