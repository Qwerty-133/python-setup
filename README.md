
<p align="center">
  <a href="LICENSE">
    <img alt="License" src="https://img.shields.io/github/license/Qwerty-133/python-setup">
  </a>
  <a href="https://github.com/Qwerty-133/python-setup/releases/latest">
    <img alt="Release" src="https://img.shields.io/github/v/release/Qwerty-133/python-setup">
  </a>
</p>

# Python Environment Setup Action

A Github action for installing and configuring
- [Python](https://www.python.org/)
- [Poetry](https://python-poetry.org/)
- [pre-commit](https://pre-commit.com/) (optionally)
- and your project's dependencies.

This action caches heavily, to speed up your workflow. It is a composite action which can be viewed at [action.yml](action.yml).

## Usage

```yaml
- name: Setup the Python Environment
  uses: Qwerty-133/python-setup@v1
  with:
    python-version: 3.11
```

The python-version input is required, however the action automatically chooses the latest Poetry version.

Poetry will be added to Path, and the venv will be created in the project directory.
Scripts can be run using `poetry run <script>`. For example, to run `pytest`, use `poetry run pytest`.

To source the venv, add this to your workflow:

```yaml
- name: Source the venv
  run: source $VENV
```

> Also see [#windows](#windows) if you are using Windows runners.

## Cache

This action caches the Poetry installation, it's venv, and pre-commit hooks.

To disable the cache entirely, the use-cache input can be set to false.

```yaml
- name: Setup the Python Environment
  uses: Qwerty-133/python-setup@v1
  with:
    python-version: 3.11
    use-cache: false
```

## pre-commit

To use pre-commit, it must be added to Poetry's dev-dependencies.

```bash
poetry add --group dev pre-commit
```

To skip installing pre-commit, the skip-precommit input can be set to true.

```yaml
- name: Setup the Python Environment
  uses: Qwerty-133/python-setup@v1
  with:
    python-version: 3.11
    skip-precommit: true
```

## Windows

You must set the default shell to be bash, by adding the following in your workflow.

```yaml
defaults:
  run:
    shell: bash
```

On Windows runners, only pre-commit hooks are cached, since caching the Poetry installation and venv is problematic.

## Credits

The [`snok/install-poetry`](https://github.com/snok/install-poetry) action is used to install Poetry.
