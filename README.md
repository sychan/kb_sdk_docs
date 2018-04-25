# KBase SDK Documentation

This repo holds the source code for [Sphinx](http://www.sphinx-doc.org/en/master/) documentation describing the KBase Software Development Kit.

[**You can visit the documentation website here**](http://spacejam.com)

## Development

* Python version: 3.6.5
  * If you use pyenv, the version is set locally in `.python-version`
* Using pipenv with `Pipfile` and `Pipfile.lock` tracking dependencies
* Install existing dependencies with `pipenv install`
* Build the documentation with `pipenv run make html`
* Deploy changes with `make deploy`
* To install a dependency, use `pipenv install package`

### Deploying changes

HTML is built into `build/html`. You can deploy it to the `gh-pages` branch with `make deploy`
