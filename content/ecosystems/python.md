+++
title = 'Python'
archetype = "chapter"
weight = 3.1
+++

## Poetry

Python projects using [`poetry`](https://python-poetry.org/docs/) are able to use the `poetry upgrade` command to upgrade a dependency within the version range as specified in `pyproject.toml`. This means that a specification of `^1.0.0` in the project would update the lock file from `1.0.0` to `1.0.1` but not `2.0.0`, as a rule this is desirable as changes outside the version specification may result in subtle breaking changes being introduced into the project.

### Update Runbook

It's advisable to run all `python` commands in a venv, this is especially important when working across many different projects. It's also strongly advised that Windows users use the Windows Subsystem for Linux (WSL) as some tests behave differently even in `pwsh`;

```sh
python -m venv ~/venv-stacks-data
```

This venv can be activated with:

```sh
source ~/venv-stacks-data/bin/activate
```

At this point the shell has an activated and isolated python install with the `python` binary available in the path.


