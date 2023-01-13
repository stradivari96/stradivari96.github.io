---
title: "🗺️ Personal Roadmap"
date: 2023-01-01
draft: false

tags: ["Software Engineering"]
---

🚧 WORK IN PROGRESS

References and things to learn.

<!--more-->

- https://roadmap.sh/backend/
- https://roadmap.sh/software-architect/

## General books

- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)
- [Software Engineering at Google](https://abseil.io/resources/swe-book)
- [Clean Code Summary](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29): don't read the book

## Python

[Cheatsheet](https://gto76.github.io/python-cheatsheet/)

### Basics

Download [Python](https://www.python.org/downloads/) and [Visual Studio Code](https://code.visualstudio.com/) with the Python extension,
start with simple single file scripts.

- [Introducing Python (2019)](https://www.oreilly.com/library/view/introducing-python-2nd/9781492051374/): Basics, ignore everything after chapter 11
- Practice [Codewards](codewars.com/), very important to checkout other solutions after submitting
- Skim through [pep8](https://peps.python.org/pep-0008/)
- Learn a bit of DSA (Data Structures and Algorithms), do easies in [neetcode](https://neetcode.io/roadmap)
- Projects:
  - Make a discord bot with [discord.py](https://discordpy.readthedocs.io/)
  - Program a simple game with [PyGame](https://www.pygame.org/docs/) such as [Snake](https://www.geeksforgeeks.org/snake-game-in-python-using-pygame-module/)
  - Deploy a simple [Flask](https://flask.palletsprojects.com/en/2.2.x/) website in [Deta](https://www.deta.sh/)

### Advanced

Switch to [PyCharm](https://www.jetbrains.com/pycharm/) once you are familiar with the syntax and concepts such as virtualenvs, git, etc.

- Read:
  - [Fluent Python (2022)](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)
  - [Architecture Patterns with Python](https://www.cosmicpython.com/book/preface.html)
- Watch:
  - [ArjanCodes](https://www.youtube.com/@ArjanCodes/videos): Advanced, refactoring videos are great.
  - [mCoding](https://www.youtube.com/channel/UCaiL2GDNpLYH6Wokkk1VNcg): Trivia, watch popular videos.
- Learn about testing: https://testdriven.io/guides/complete-python/
- Learn about [Design Patterns](https://refactoring.guru/design-patterns) and their implementation in [python](https://github.com/faif/python-patterns), but don't overdo it.
- News:
  - [Latest Python features](https://docs.python.org/3/whatsnew/index.html): read summaries
  - [Reddit](https://www.reddit.com/r/Python/top/?t=month): Check top posts by year or month
  - [Top libraries](https://tryolabs.com/blog/2022/12/26/top-python-libraries-2022): by year
- Checkout the code from some Open Source projects:
  - [minecraft](https://github.com/fogleman/Minecraft): 900 lines
  - [requests](https://github.com/psf/requests) and [httpx](https://github.com/encode/httpx)
  - [arq](https://github.com/samuelcolvin/arq): small project, take a good look at every file
  - [typer](https://github.com/tiangolo/typer)

{{< notice note>}}
Use [venv](https://docs.python.org/3/library/venv.html) and [pip-tools](https://github.com/jazzband/pip-tools) for dependency management,
[poetry](https://python-poetry.org/) is also a good option, specially for packaging.
Avoid [pipenv](https://github.com/pypa/pipenv).
{{< /notice >}}

## Version Control (Git)

- https://docs.github.com/en/get-started/quickstart/hello-world
- https://training.github.com/downloads/github-git-cheat-sheet/
- https://ohshitgit.com/
- https://gitmoji.dev/

```
git checkout develop
git pull
git checkout -b feature/new-feature
git add .
git commit -m ":sparkles: Some new thing"
git push
```

## Web Development

TODO

- [How does the Internet work?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work)
- [What is a web server?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
