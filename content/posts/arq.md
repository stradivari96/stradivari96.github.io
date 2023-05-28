---
title: "👨‍🎓 arq source analysis"
date: 2023-05-21
draft: false

tags: ["Python"]
---

Quick look at the repo for [arq](https://github.com/samuelcolvin/arq/) by Samuel Colvin.

<!--more-->

## Links:

- https://github.com/samuelcolvin/arq/
- https://pypi.org/project/arq/

## Codemap
- [`.github/`](https://github.com/samuelcolvin/arq/blob/main/.github/workflows/ci.yml): mainly [**GitHub Actions**](https://docs.github.com/en/actions)
- `arq/`: source code
- `docs/`: [**sphinx**](https://www.sphinx-doc.org/en/master/) documentation
- `requirements/`: requirements.in and txt, using [**pip-tools**](https://github.com/jazzband/pip-tools), for local dev
- `tests/`: [**pytest**](https://docs.pytest.org/), one test file per public module
- `.codecov.yml`: [**codecov**](https://about.codecov.io/) config
- `.gitignore`
- `.pre-commit-config.yaml`: [**pre-commit**](https://github.com/pre-commit/pre-commit) + `make lint` + `mypy`
- `HISTORY.rst`: changelog
- `LICENSE`: MIT
- `Makefile`: `install`, `format`, `lint` (flake8, isort, black), `test`, `all`, `docs`, etc
- `README.md`: simple readme for github and pypi
- [`pyproject.toml`](https://github.com/samuelcolvin/arq/blob/main/pyproject.toml): [**hatchling**](https://github.com/pypa/hatch), [PEP 621](https://packaging.python.org/en/latest/specifications/declaring-project-metadata/), config for pytest, coverage, black, isort, mypy

## Dependencies
### Direct
- [redis[hiredis]](https://github.com/redis/redis-py): redis interface with faster parser
- [click](https://github.com/pallets/click): Command Line Interface Creation Kit, from the pallets team
- [typing-extensions](https://github.com/python/typing_extensions): backport of new typing features for older python versions
### docs.in
- [Sphinx](https://github.com/sphinx-doc/sphinx): documentation generator for multiple languages
### linting.in
- [black](https://github.com/psf/black): code formatter
- [flake8](https://github.com/PyCQA/flake8): linter
- flake8-quotes
- [isort[colors]](https://github.com/PyCQA/isort): import sorter with colorama
- [mypy](https://github.com/python/mypy): static type checker
- types-pytz: [typeshed](https://github.com/python/typeshed) stubs for pytz
- types-redis: typeshed stubs for redis
### testing.in
- coverage[toml]
- dirty-equals
- msgpack
- pydantic
- pytest
- pytest-asyncio
- pytest-mock
- pytest-sugar
- pytest-timeout
- pytz
- redislite

## Code examples
- Use `__all__` in `__init__.py` to control what is exported
```python
__all__ = (
    'ArqRedis',
    'create_pool',
    'cron',
    'VERSION',
    'Retry',
    'Worker',
    'check_health',
    'func',
    'run_worker',
)
```
- `__main__.py` file to run the module as a script, e.g. `arq --help`:
```python
from .cli import cli

if __name__ == '__main__':
    cli()
```
- `from typing import TYPE_CHECKING` to avoid circular imports:
```python
if TYPE_CHECKING:
    from .typing import WorkerSettingsType
```
- `py.typed` file to indicate that the package is fully typed, for tools like mypy
- sphinx markup in docstrings, type only with type hints
```python
async def abort(self, *, timeout: Optional[float] = None, poll_delay: float = 0.5) -> bool:
    """
    Abort the job.

    :param timeout: maximum time to wait for the job result before raising ``TimeoutError``,
        will wait forever on None
    :param poll_delay: how often to poll redis for the job result
    :return: True if the job aborted properly, False otherwise
    """
```
- Use `sys.version_info` to check for python version
```python
if sys.version_info >= (3, 8):
    from typing import Literal, Protocol
else:
    from typing_extensions import Literal, Protocol
```
- `typing.py` file with type aliases and Protocol classes instead of Callable
```python
OptionType = Union[None, Set[int], int]
WeekdayOptionType = Union[OptionType, Literal['mon', 'tues', 'wed', 'thurs', 'fri', 'sat', 'sun']]
SecondsTimedelta = Union[int, float, timedelta]

class WorkerCoroutine(Protocol):
    __qualname__: str

    async def __call__(self, ctx: Dict[Any, Any], *args: Any, **kwargs: Any) -> Any:  # pragma: no cover
        pass
```
- Use `cast` to avoid typing errors
```python
redis_settings = cast(Optional[RedisSettings], cls_kwargs.get('redis_settings'))
health_check_key = cast(Optional[str], cls_kwargs.get('health_check_key'))
queue_name = cast(Optional[str], cls_kwargs.get('queue_name'))
```
- Keyword only arguments using `*`
```python
async def create_pool(
    settings_: RedisSettings = None,
    *,
    retry: int = 0,
    job_serializer: Optional[Serializer] = None,
    job_deserializer: Optional[Deserializer] = None,
    default_queue_name: str = default_queue_name,
    expires_extra_ms: int = expires_extra_ms,
) -> ArqRedis:
```