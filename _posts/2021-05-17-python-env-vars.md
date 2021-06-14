---
title: Environment variables in Python
excerpt: Some libraries for handling environment variables
read_time: true
share: true
toc: true
related: true
comments: true
header:
  teaser: "https://www.bruker.com/es/applications/detection-and-environmental/environmental/_jcr_content/teaserImage.coreimg.jpeg/1597140428584/nature-environmental-grass-bird.jpeg"
tags: [Python, Programming, Libraries]
---

![img](https://www.bruker.com/es/applications/detection-and-environmental/environmental/_jcr_content/teaserImage.coreimg.jpeg/1597140428584/nature-environmental-grass-bird.jpeg)

Read [The Twelve Factor App](https://12factor.net/config) for context.

**Note**: Sorted by number of downloads.

## [Pydantic](https://pydantic-docs.helpmanual.io/)

[![Downloads](https://img.shields.io/pypi/dw/pydantic)](https://pypi.org/project/pydantic/)
[![GitHub stars](https://img.shields.io/github/stars/samuelcolvin/pydantic.svg?style=social)](https://github.com/samuelcolvin/pydantic)
[![GitHub last commit](https://img.shields.io/github/last-commit/samuelcolvin/pydantic)](https://github.com/samuelcolvin/pydantic)

- Recommended by [FastAPI](https://fastapi.tiangolo.com/advanced/settings/#the-env-file)

```python
from pydantic import BaseSettings


class Settings(BaseSettings):
    app_name: str = "Awesome API"
    admin_email: str
    items_per_user: int = 50

settings = Settings(_env_file='.env')
```

## [Python-dotenv](https://saurabh-kumar.com/python-dotenv/)

[![Downloads](https://img.shields.io/pypi/dw/python-dotenv)](https://pypi.org/project/python-dotenv/)
[![GitHub stars](https://img.shields.io/github/stars/theskumar/python-dotenv.svg?style=social)](https://github.com/theskumar/python-dotenv)
[![GitHub last commit](https://img.shields.io/github/last-commit/theskumar/python-dotenv)](https://github.com/theskumar/python-dotenv)

- Recommended by [Flask](https://flask.palletsprojects.com/en/2.0.x/cli/#environment-variables-from-dotenv)
- Used internally by Pydantic, environs, Dynaconf, etc.

```python
from dotenv import load_dotenv, dotenv_values

load_dotenv()  # read using os.environ["KEY"]
```

Does **NOT** handle types.
{: .notice--danger}

## [Django-environ](https://django-environ.readthedocs.io/en/latest/)

[![Downloads](https://img.shields.io/pypi/dw/django-environ)](https://pypi.org/project/django-environ/)
[![GitHub stars](https://img.shields.io/github/stars/joke2k/django-environ.svg?style=social)](https://github.com/joke2k/django-environ)
[![GitHub last commit](https://img.shields.io/github/last-commit/joke2k/django-environ)](https://github.com/joke2k/django-environ)

```python
import environ
env = environ.Env(
    # set casting, default value
    DEBUG=(bool, False)
)
# reading .env file
environ.Env.read_env()

# False if not in os.environ
DEBUG = env('DEBUG')

# Raises django's ImproperlyConfigured exception if SECRET_KEY not in os.environ
SECRET_KEY = env('SECRET_KEY')

# Parse database connection url strings like psql://user:pass@127.0.0.1:8458/db
DATABASES = {
    # read os.environ['DATABASE_URL'] and raises ImproperlyConfigured exception if not found
    'default': env.db(),
    # read os.environ['SQLITE_URL']
    'extra': env.db('SQLITE_URL', default='sqlite:////tmp/my-tmp-sqlite.db')
}

CACHES = {
    # read os.environ['CACHE_URL'] and raises ImproperlyConfigured exception if not found
    'default': env.cache(),
    # read os.environ['REDIS_URL']
    'redis': env.cache('REDIS_URL')
}
```

Mainly for [Django](https://www.djangoproject.com/).
{: .notice--info}

## [Python Decouple](https://github.com/henriquebastos/python-decouple)

[![Downloads](https://img.shields.io/pypi/dw/python-decouple)](https://pypi.org/project/python-decouple/)
[![GitHub stars](https://img.shields.io/github/stars/henriquebastos/python-decouple.svg?style=social)](https://github.com/henriquebastos/python-decouple)
[![GitHub last commit](https://img.shields.io/github/last-commit/henriquebastos/python-decouple)](https://github.com/henriquebastos/python-decouple)

```python
from decouple import config

SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
EMAIL_HOST = config('EMAIL_HOST', default='localhost')
EMAIL_PORT = config('EMAIL_PORT', default=25, cast=int)
```

## [Environs](https://github.com/sloria/environs)

[![Downloads](https://img.shields.io/pypi/dw/environs)](https://pypi.org/project/environs/)
[![GitHub stars](https://img.shields.io/github/stars/sloria/environs.svg?style=social)](https://github.com/sloria/environs)
[![GitHub last commit](https://img.shields.io/github/last-commit/sloria/environs)](https://github.com/sloria/environs)

- Used by [cookiecutter-flask](https://github.com/cookiecutter-flask/cookiecutter-flask)

```python
from environs import Env

env = Env()
env.read_env()  # read .env file, if it exists
# required variables
gh_user = env("GITHUB_USER")  # => 'sloria'
secret = env("SECRET")  # => raises error if not set

# casting
max_connections = env.int("MAX_CONNECTIONS")  # => 100
ship_date = env.date("SHIP_DATE")  # => datetime.date(1984, 6, 25)
ttl = env.timedelta("TTL")  # => datetime.timedelta(0, 42)
log_level = env.log_level("LOG_LEVEL")  # => logging.DEBUG

# providing a default value
enable_login = env.bool("ENABLE_LOGIN", False)  # => True
enable_feature_x = env.bool("ENABLE_FEATURE_X", False)  # => False

# parsing lists
gh_repos = env.list("GITHUB_REPOS")  # => ['webargs', 'konch', 'ped']
coords = env.list("COORDINATES", subcast=float)  # => [23.3, 50.0]

# parsing dicts
gh_repos_priorities = env.dict(
    "GITHUB_REPO_PRIORITY", subcast_values=int
)  # => {'webargs': 2, 'konch': 3}
```

## [Dynaconf](https://www.dynaconf.com/)

[![Downloads](https://img.shields.io/pypi/dw/dynaconf)](https://pypi.org/project/dynaconf/)
[![GitHub stars](https://img.shields.io/github/stars/rochacbruno/dynaconf.svg?style=social)](https://github.com/rochacbruno/dynaconf)
[![GitHub last commit](https://img.shields.io/github/last-commit/rochacbruno/dynaconf)](https://github.com/rochacbruno/dynaconf)

```python
# config.py
from dynaconf import Dynaconf

settings = Dynaconf(
    settings_files=['settings.toml', '.secrets.toml'],
)

assert settings.key == "value"
```

```toml
# settings.toml
key = "value"
a_boolean = false
number = 1234
a_float = 56.8
a_list = [1, 2, 3, 4]
a_dict = {hello="world"}

[a_dict.nested]
other_level = "nested value"
```

```toml
# .secrets.toml
password = "s3cr3t"
token = "dfgrfg5d4g56ds4gsdf5g74984we5345-"
message = "This file doesn't go to your pub repo"
```

```bash
export DYNACONF_NUMBER=789
export DYNACONF_FOO=false
```

Also loads `.env` file automatically.
{: .notice--info}

## [Hydra](https://hydra.cc/)

[![Downloads](https://img.shields.io/pypi/dw/hydra-core)](https://pypi.org/project/hydra-core/)
[![GitHub stars](https://img.shields.io/github/stars/facebookresearch/hydra.svg?style=social)](https://github.com/facebookresearch/hydra)
[![GitHub last commit](https://img.shields.io/github/last-commit/facebookresearch/hydra)](https://github.com/facebookresearch/hydra)

- By Facebook

Mainly for Machine Learning.
{: .notice--warning}
