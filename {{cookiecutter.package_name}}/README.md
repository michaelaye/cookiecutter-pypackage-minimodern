{{ cookiecutter.package_name }}
{{ cookiecutter.package_name|count * "=" }}

{% if cookiecutter.readme_pypi_badge -%}
[[!Latest PyPI version](https://img.shields.io/pypi/v/{{ cookiecutter.package_name }}.svg)](https://pypi.org/project/{{ cookiecutter.package_name }})
{%- endif %}

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

{{ cookiecutter.package_description }}

## Usage

## Installation

### Requirements

### Compatibility

## Licence

## Authors

`{{ cookiecutter.package_name }}` was written by `{{ cookiecutter.author_name }} <{{ cookiecutter.author_email }}>`_.
