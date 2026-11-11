# python-template

A Python package template for Roman GRACE projects. It gives you:

- A packaged Python project built from a single `pyproject.toml`, following the [Python Packaging Authority recommendations](https://packaging.python.org/en/latest/tutorials/packaging-projects/).
- Automatic versioning from git tags with `setuptools-scm`.
- Tests with `pytest` and coverage, run on Python 3.10–3.13 by GitHub Actions on every push and pull request (`.github/workflows/ci.yml`).
- Linting and formatting with [Ruff](https://docs.astral.sh/ruff/), checked in CI and optionally on every commit with `pre-commit`.
- Sphinx documentation in `docs/`, published to GitHub Pages on every push to `main` (`.github/workflows/docs.yml`).

The template is not set up for PyPI releases or ReadTheDocs, but both can be added.

## Layout

```
├── pyproject.toml            # package metadata, dependencies and tool settings
├── src/my_package/           # your package code
├── tests/                    # pytest tests
├── docs/                     # Sphinx documentation
├── .github/workflows/        # CI and docs workflows
├── .pre-commit-config.yaml   # optional pre-commit hooks
├── CHANGELOG.md
├── CITATION.cff
├── CODE_OF_CONDUCT.md
└── LICENSE
```

# Creating a New Project

1. On GitHub, click **Use this template** → **Create a new repository**. Projects related to the work of Roman GRACE should live in the [`GRACE-Roman`](https://github.com/GRACE-Roman) organization; you are welcome to use the template for other projects in your personal account. The examples below use `new_project` as the repository name.

2. Clone the new repository:

        git clone git@github.com:GRACE-Roman/new_project.git
        cd new_project

3. Create and activate an environment, for example with `conda`:

        conda create --name new_project python=3.12
        conda activate new_project

    Install your package's dependencies with `pip` from `pyproject.toml` (step 6), not with `conda`.

4. Edit `pyproject.toml`:

        [project]
        name = "new_package_name"
        authors = [{name = "Your Name", email = "you@example.com"}]
        description = "A short description"
        dependencies = [
            "numpy>=1.20",
        ]

        [project.urls]
        "Homepage" = "https://github.com/GRACE-Roman"
        "Bug Tracker" = "https://github.com/GRACE-Roman/new_project/issues"
        "Source Code" = "https://github.com/GRACE-Roman/new_project"

        [tool.setuptools_scm]
        write_to = "src/new_package_name/_version.py"

    `name` is the name you will `import`. It does not have to match the repository name; use lower case letters and underscores ([PEP 8](https://peps.python.org/pep-0008/#package-and-module-names)). List every package you import directly under `dependencies`. Test and documentation dependencies live under `[project.optional-dependencies]`.

5. Rename the package directory and update the references to `my_package`:

        git mv src/my_package src/new_package_name

    Then replace `my_package` in `tests/test_example.py`, `docs/conf.py` and `docs/index.rst`.

6. Install the package in editable mode with the test and docs extras:

        pip install -e ".[test,docs]"

7. Update the remaining files:
    - `README.md`: replace this text with a description of your package.
    - `CITATION.cff`: set the title, authors, ORCIDs, URL and release date so GitHub can offer a **Cite this repository** entry. See the [AstroBetter post](https://www.astrobetter.com/blog/2019/07/01/citing-astronomy-software-inline-text-examples/) on citing astronomy software and the [GitHub docs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files).
    - `LICENSE`: replace `Author Name` in the copyright line.
    - `docs/conf.py`: set `project`, `copyright` and `author`.
    - `CHANGELOG.md`: replace the example entries.

8. Turn on GitHub Pages: in the repository **Settings** → **Pages**, set **Source** to **GitHub Actions**. The docs are published on the next push to `main`.

🎉 You have a package.

## What Not to Change

- Keep the `LICENSE` file; only update the copyright holder. See [this article](https://www.astrobetter.com/blog/2014/03/10/the-whys-and-hows-of-licensing-scientific-code/) on licensing scientific code.
- Within the `GRACE-Roman` organization, leave `CODE_OF_CONDUCT.md` as is. In a personal account, remove the bracketed text and change the reporting section to read "Any violations of the Code of Conduct should be reported to the owners of this repository."
- You do not need `setup.py`, `setup.cfg` or `requirements.txt`. Everything belongs in `pyproject.toml`, unless you have compiled C or Fortran extensions.

# Development

## Installing

Install from the repository root in editable mode, so changes to the source take effect without reinstalling:

```
pip install -e .
```

Do not add the package to your `$PYTHONPATH`. In an interactive session (`ipython`, Jupyter) you still need to reload a module you have edited:

```python
import importlib

importlib.reload(module_x)
```

Users who are not developing the code can install it directly from GitHub:

```
pip install git+https://github.com/GRACE-Roman/new_project
```

## Testing

```
pip install -e ".[test]"
pytest --cov=src/new_package_name tests
```

The coverage report shows how much of your code the tests exercise; aim for more than 80%. CI runs the same tests on Python 3.10–3.13 and checks that the package builds.

## Linting and Formatting

CI runs Ruff and fails if either check does:

```
ruff check .
ruff format --check .
```

Run `ruff check --fix .` and `ruff format .` to fix most issues automatically. Ruff's settings are in `[tool.ruff]` in `pyproject.toml`.

To run these checks automatically before every commit, install the `pre-commit` hooks once:

```
pip install pre-commit
pre-commit install
```

## Documentation

Edit `docs/index.rst` (reStructuredText; see the [Sphinx tutorial](https://www.sphinx-doc.org/en/master/tutorial/index.html)) and build locally:

```
pip install -e ".[docs]"
make -C docs html
open docs/_build/html/index.html
```

The docs workflow publishes the same build to GitHub Pages on every push to `main`, once Pages is turned on (step 8). This template's own documentation is at [https://grace-roman.github.io/python-template/](https://grace-roman.github.io/python-template/). To stop publishing, disable the workflow or delete `.github/workflows/docs.yml`, and unpublish the site under **Settings** → **Pages**.

## Versioning

`setuptools-scm` sets the version from git tags in the semantic versioning format `A.B.C`. To make a release, tag the commit and push the tag:

```
git tag v0.1.0
git push origin v0.1.0
```

Between tags, the version includes the distance from the last tag, for example `0.1.1.dev3+g1a2b3c4`. The version is available as `new_package_name.__version__`. See the [setuptools-scm docs](https://setuptools-scm.readthedocs.io/) for details.
