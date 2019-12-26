cookiecutter-pypackage-minimodern
==============================

An opinionated, minimal and modern [cookiecutter](https://github.com/audreyr/cookiecutter) template
for Python packages, and some guidelines for Python packaging.

This is a fork of [pyproject-minimal](https://github.com/kragniz/cookiecutter-pypackage-minimal)
aiming at support of the latest developments in Python dev tools, python packaging, Github
functionalities and *de facto* standards.

This is meant to be as simple as possible but as powerful as necessary, however it won't help you
beyond the initial configuration.
If you would like more project management automation, consider [dephell](https://dephell.org).

## Usage

If you are not yet using cookiecutter, I recommend installing it via with
[pipx](https://pipxproject.github.io/pipx/).

```console
pipx install cookiecutter
cookiecutter https://github.com/Evpok/cookiecutter-pypackage-minimodern
```

**Use [virtualenv](https://virtualenv.pypa.io)** to develop your package

```console
python3 -m virtualenv .virtenv
source .virtenv/bin/activate
pip install -e .
```

Keep it updated with `pip install -U -e .` when its internal structure change.

You should then change the classifiers in [`{{ package_name
}}/setup.cfg`]({{cookiecutter.package_name}}/setup.cfg) - it is assumed that the project will run on
the latest versions of Python , so you should remove any classifiers that do not apply.
The full list of PyPI classifiers can be found [here](https://pypi.org/classifiers/).

Fill out [`README.md`]({{cookiecutter.package_name}}/README.md), and — if necessary — [choose a
license](https://choosealicense.com/) for the project.

The version is single-sourced in
[`{{cookiecutter.package_name}}/__init__.py`]({{cookiecutter.package_name}}/__init__.py) to avoid
having your project depend on setuptools, this is the place where you should change it when you push
a new version.

**Prefer [Semantic Versioning](https://semver.org)** if you don't have very specific needs.
It is not necessarily perfect, but it is sensible and is widely known.
Have a look at [PEP 440](https://www.python.org/dev/peps/pep-0440) for more on Python packages
version names standards, in particular, the pre- and post-release identifiers as pip can take
advantage of them.

## Explanations

The decisions we made are detailed here, feel free to file an issue with us if you have ideas or
opinions on how to improve them.

### README

- **[`README`]({{cookiecutter.package_name}}/README.md) should use Markdown**
  This is the format used and known by most people outside of the Python ecosystem.
  In most flavours it is a subset of the ReStructured Text format used for Python so it functions as
  a least common denominator.
  Since README as displayed on the Github repo page is *de facto* a common entry point to a
  project, it is important that it is displayed correctly there.
- **As few README files as possible but as many as necssary**
  In addition to README, [`CHANGELOG.md`]({{cookiecutter.package_name}}/CHANGELOG.md) is provided
  with the convention from [keep a changelog](https://keepachangelog.com). Please use it. Please.

### LICENSE

- **MIT license by default**
  
  - This template provides you the omnipresent [MIT](https://choosealicense.com/licenses/mit/)
  licence: it lets people do almost anything they want with your project, including to make and
  distribute closed source versions.
    This is not necessarily ideal, but it is known well enough to make people feel safe and reduce
    friction.

    If and when your project gains more traction, you might want to consider other options to e.g.
    ensure that major companies that benefit from you and your communities hard work are giving back
    accordingly to their capacities.
  - If you [choose another license](https://choosealicense.com/), you also need to update [`{{
  package_name}}/setup.cfg`]({{cookiecutter.package_name}}/setup.cfg): adjust the `classifiers` and
  `license` fields accordingly.
- **A license is a requirement**
  Nowadays, people who want to use your library/application want to make sure they can do it legally.
  If your library is proprietary, please reconsider.

### [`setup.cfg`]({{cookiecutter.package_name}}/setup.cfg)

- **Use setuptools**
  It's the standard packaging library for Python. That's it, that's the tweet.
- **`setup.py` should not exist**
  Metadata should not be Turing-complete.
  Sadly, for the time being, it seems that a [`setup.py`]({{cookiecutter.package_name}}/setup.py) is
  still mandatory, but we can keep it pure boilerplate and put all the actual data in
  [`setup.cfg`]({{cookiecutter.package_name}}/setup.cfg) and hopefully in `pyproject.toml` at [some point](https://github.com/pypa/setuptools/issues/1688).
  Have a look at [setuptools'
  doc](https://setuptools.readthedocs.io/en/latest/setuptools.html#configuring-setup-using-setup-cfg-files)
  to learn more about this.

  More complex installation stories (C extensions and so on) are out of scope for this template.
- **setup.cfg should be the canonical source of package dependencies**
  Requirements files and pinned dependencies have their uses, but not for installable packages in my
  opinion.
  If you don't agree, at least have a look at [Pipenv](https://pipenv.kennethreitz.org) to use an
  appropriate toolset.

  See also [Testing](#Testing) below for testing dependencies.

### Linting and formatting

- **Use a linter**
  The default here is [flake8](http://flake8.pycqa.org), chosen for its versatility and relatively
  low false-positive rate.
- **Use [black](https://pypi.org/project/black)**
  Not just any formatter, **black**.
  This project is the best thing that has happened to Python in a long time.
  It provides a sane, sensible, readable, deterministic and previsible format without any effort of
  configuration.
  If you don't like it, well, you will get used to it.
- **Use [mypy](http://www.mypy-lang.org)**
  And type your code as much as sensible, not only it will provide you with very powerful linting,
  but it will also make your documentation much clearer.
- **Lint EVERYTHING**
  We also add [a sane-ish config]({{cookiecutter.package_name}}/.markdownlint.json) for [markdownlint](https://github.com/DavidAnson/markdownlint).
  I am not a huge fan of it, but I don't know of a better alternative to lint Markdown files.

### Testing

- **Use [Tox](https://tox.readthedocs.io) to manage test environments**
  Tox provides isolation, runs tests across multiple Python versions, and ensures the package can be
  installed.
- **Uses [pytest](https://docs.pytest.org) as the default test runner**
  This can be changed easily, though pytest is a easier, more powerful test library and runner than
  the standard library's unittest.

  Consider using it in combination with [hypothesis](https://hypothesis.works/).
- **Define testing dependencies in [`tox.ini`]({{cookiecutter.package_name}}/tox.ini)**
  Avoid duplicating dependency definitions, and use `tox.ini` as the canonical description of how
  the unittests should be run.
- **[`tests/`]({{cookiecutter.package_name}}/tests) is not a package by default**
  Therefore, it has no `__init__.py` file, but have a look at [pytest's
  doc](https://docs.pytest.org/en/latest/goodpractices.html#choosing-a-test-layout-import-rules) on
  chosing a test layout.