+++
title = 'Python'
archetype = "chapter"
weight = 3.1
+++

## Poetry

Python projects using [`poetry`](https://python-poetry.org/docs/) are able to use the `poetry upgrade` command to upgrade a dependency within the version range as specified in `pyproject.toml`. This means that a specification of `^1.0.0` in the project would update the lock file from `1.0.0` to `1.0.1` but not `2.0.0`, as a rule this is desirable as changes outside the version specification may result in subtle breaking changes being introduced into the project.

### Prerequisites

To be able to update Poetry based projects, then a working installation of [Python 3](https://www.python.org/downloads/) is required, in many cases operating system repositories have an up-to-date version of Python available.

Additionally `pipx` is one of the most common ways to [install Poetry](https://pipx.pypa.io/stable/installation/).

### Update Run Book

#### Create a Virtual Environments

> [!TIP]
> These instructions describe how to install tools on a POSIX compliant operating system and shell, it's strongly advised that Windows Users install and use [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install).

It's advisable to run all `python` commands in a venv, this is especially important when working across many different projects.

Create a directory to store `venvs` in:

```sh
mkdir ~/.venvs
```

Create the `venv`:

```sh
python -m venv ~/.venvs/stacks-data
```

This `venv` can be activated with:

```sh
source ~/venv-stacks-data/bin/activate
```

At this point the shell has an activated and isolated python install with the `python` binary available in the path.

> [!IMPORTANT]
> If you need a specific version of python then you will need to do some additional work to install the appropriate version of Python, the following instructions are for Debian-based Distributions (i.e. Debian, Ubuntu, Kali, etc.):
>
> Add the `deadsnakes` repository (old Python versions):
>
> ```sh
> sudo add-apt-repository ppa:deadsnakes/ppa
> ```
>
> Update apt and install packages, using python 3.10 as an example:
>
> ```sh
> sudo apt update && sudo apt install -y python3.10 python3.10-venv
> ```
>
> Create a `venv` using a named python version:
>
> ```sh
> python3.10 -m venv ~/.venvs/stacks-data
> ```
>
> You can source it in the same way as above

#### Setup Development Environment

> [!NOTE]
> If there is a `Makefile`, `tasks.yaml` or `eirctl.yaml` then the appropriate task should be executed using [GNU Make](https://www.gnu.org/software/make/) or [eirctl](https://github.com/Ensono/eirctl) to execute setup commands. These tasks often include additional setup steps required to work with the project.

Poetry can be installed with the following command:

```sh
pipx install poetry
```

Next, install the packages for the project:

```sh
poetry install
```

If there is a `tests` folder you can run these with the following command:

```sh
poetry run python -m pytest tests
```

This should result in all tests passing, and provides a baseline for upgrading packages. If at any point in the future running the tests results in a failed test then it's likely a breaking change in a package.

#### Upgrading packages with `poetry update`

Poetry offers an `update` sub-command which does the equivalent of deleting `poetry.lock` and then running `poetry install`:

```sh
poetry update
```

This works because the default behaviour of Poetry is to install the latest version of a dependency, including transitive dependencies, that still matches the version specification.

This means that:

- If version `1.0.0` is installed initially, then `^1.0.0` will be listed in `pyproject.toml` and `1.0.0` will be listed in `poetry.lock`.
- If version `1.0.1` is released later, then poetry will not automatically upgrade the project as `1.0.0` is specified in `poetry.lock`.
- However when `poetry update` is run, the contents of `poetry.lock` is ignored and `1.0.1` will be installed along with updating `poetry.lock` with version `1.0.1`.

Running this command is normally enough to mitigate known vulnerabilities with fixes in the package repository.

#### Upgrading packages manually

In instances where the package version is too restrictive to automatically update a dependency, i.e. `=1.0.0` then it's necessary to upgrade the package manually. This can be done simply by modifying the line in `pyproject.toml` then re-running `poetry install`.

### Upgrading packages with breaking changes

When the semantic version changes the **major** version then care must be taken as it's likely a breaking change. Where frameworks have changed substantially they likely have specific instructions on upgrading the package including changing obsolete or deprecated API calls.
