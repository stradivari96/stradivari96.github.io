---
title: "🐍 Python"
date: 2023-01-01
draft: false
weight: 2
showToc: false

tags: ["Software Engineering"]
---

[⬅️Back to Roadmap](/posts/roadmap)

[💼 Jobs](https://www.linkedin.com/jobs/search/?keywords=python&location=Spain)
/
[🥋 Codewards](https://www.codewars.com/kata/search/python)

## Basics

Download [Python](https://www.python.org/downloads/) and [Visual Studio Code](https://code.visualstudio.com/) with the Python extension.

Start with **single file scripts**.

- 📚[Introducing Python (2019)](https://www.oreilly.com/library/view/introducing-python-2nd/9781492051374/): Ignore everything after chapter 11
- 📝[Basic OOP](https://realpython.com/python3-object-oriented-programming/)
- 🎨Skim through [pep8](https://peps.python.org/pep-0008/)
- ⚡️Learn a bit of DSA (Data Structures and Algorithms): do **easy** in [neetcode](https://neetcode.io/roadmap)

{{< notice note >}}
Project ideas:

- Make a discord bot with [discord.py](https://discordpy.readthedocs.io/)
- Program a simple game with [PyGame](https://www.pygame.org/docs/) such as [Snake](https://www.geeksforgeeks.org/snake-game-in-python-using-pygame-module/)
- Deploy a simple [FastAPI](https://fastapi.tiangolo.com/) website in [Deta](https://www.deta.sh/) (maybe a TODO list)

{{< /notice >}}

## Advanced

Switch to [PyCharm](https://www.jetbrains.com/pycharm/) once you are familiar with the syntax and concepts such as virtualenvs, git, etc.

- 📚Read:
  - [Fluent Python (2022)](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/):
  magic methods, dataclasses, Advanced typing, decorators
  - [Architecture Patterns with Python](https://www.cosmicpython.com/book/preface.html):
    DDD, CQRS, Event-Driver Architecture, etc.
- 📺Watch: sort by popular
  - [ArjanCodes](https://www.youtube.com/@ArjanCodes/videos): Advanced, refactoring videos are great.
  - [mCoding](https://www.youtube.com/channel/UCaiL2GDNpLYH6Wokkk1VNcg): Trivia.
- 🧰Learn about [Design Patterns](https://refactoring.guru/design-patterns/what-is-pattern) in [python](https://github.com/faif/python-patterns) but don't go too crazy.
- 📝[StackOverflow Questions](https://stackoverflow.com/questions/tagged/python?sort=votes):
  should know the answers to the most voted questions.

{{< notice info>}}
Use [venv](https://docs.python.org/3/library/venv.html) and [pip-tools](https://github.com/jazzband/pip-tools) for dependency management,
[poetry](https://python-poetry.org/) is also a good option, **specially for packaging a library**.
Avoid [pipenv](https://github.com/pypa/pipenv).
{{< /notice >}}

- ⚡️Concurrency with superfast python:
  - [Why Learn Python Concurrency](https://superfastpython.com/why-learn-python-concurrency/)
  - [How to Choose the Right Concurrency API](https://superfastpython.com/python-concurrency-choose-api/)
- 🧪Learn about [testing](https://testdriven.io/guides/complete-python/)
- 🔄Task queues: [Celery](https://docs.celeryq.dev/) / [arq](https://arq-docs.helpmanual.io/)
- 🔧CI Tools ([sample precommit](https://github.com/arrow-py/arrow/blob/74a759b88447b6ecd9fd5de610f272c8fb6130a2/.pre-commit-config.yaml)):
  [MyPy](https://mypy.readthedocs.io/en/stable/),
  [Black](https://black.readthedocs.io/en/stable/),
  [isort](https://pycqa.github.io/isort/),
  [flake8](https://flake8.pycqa.org/en/latest/) / [ruff](https://github.com/charliermarsh/ruff),
  [pip-audit](https://github.com/pypa/pip-audit),
  [bandit](https://bandit.readthedocs.io/en/latest/).
- 👀Checkout some Open Source projects, better if you have used them before:
  - [requests](https://github.com/psf/requests) and [httpx](https://github.com/encode/httpx): probably familiar with them
  - [arq](https://github.com/samuelcolvin/arq): small modern project, take a good look at every file

{{< notice tip >}}

Keep an eye on:

- [Latest Python features](https://docs.python.org/3/whatsnew/index.html): read summaries
- [Reddit](https://www.reddit.com/r/Python/top/?t=month): Check top posts by year or month
- [Top libraries](https://tryolabs.com/blog/2022/12/26/top-python-libraries-2022): by year

{{< /notice >}}

## Web Frameworks

{{< notice >}}
See [notes about web](/posts/roadmap/#web-development)
{{< /notice >}}

Deploy using https://deta.space/ or https://www.pythonanywhere.com/

- [**Django**](https://www.djangoproject.com/): [tutorial](https://tutorial.djangogirls.org/en/)
  - [readthedocs](https://github.com/readthedocs/readthedocs.org): documentation hosting
  - [sentry](https://github.com/getsentry/sentry): monitoring platform
  - [old mdn](https://github.com/mdn/kuma): kuma, old MDN
  - [guya.moe](https://github.com/subject-f/guyamoe): manga reading website
- [**Flask**](https://flask.palletsprojects.com/): [tutorial](https://flask.palletsprojects.com/tutorial/)
  - [SimpleLogin](https://github.com/simple-login/app)
  - [Apache Airflow](https://github.com/apache/airflow)
  - [redash.io](https://github.com/getredash/redash)
  - [sourcehut](https://git.sr.ht/~sircmpwn/core.sr.ht/tree)
- [**FastAPI**](https://fastapi.tiangolo.com/): [tutorial](https://testdriven.io/blog/fastapi-crud/)
  - [TermPair](https://github.com/cs01/termpair)
  - [Dispatch](https://github.com/Netflix/dispatch)
  - [Mealie](https://github.com/hay-kot/mealie)
  - [Opal](https://github.com/permitio/opal)
