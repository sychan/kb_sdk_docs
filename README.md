# KBase SDK Documentation

This repo holds the source code for [Sphinx](http://www.sphinx-doc.org/en/master/) documentation describing the KBase Software Development Kit.

[**You can visit the documentation website here**](http://kbase.github.io/kb_sdk_docs)

## Editing .rst files

All the documentation files use the "reStructuredText" format. Read the primer here: http://www.sphinx-doc.org/en/1.5/rest.html

A good reference on reStructuredText directives is here: http://docutils.sourceforge.net/docs/ref/rst/directives.html

## Development

* Python version: 3.6.5
  * If you use pyenv, the version is set locally in `.python-version`
* Using pipenv with `Pipfile` and `Pipfile.lock` tracking dependencies
* Install existing dependencies with `pipenv install`
* Run the live development server with `make live`
* Build the documentation with `pipenv run make html`
* To install a dependency, use `pipenv install package`

### Deploying changes

The `gh-pages` branch corresponds to the `/build/html` subdirectory. That directory is gitignored from master.

To deploy, you need write permissions for the `github.com/kbase/kb_sdk_docs` repository.

#### Setup

```sh
# If it's not already there:
$ mkdir build
$ git clone -b gh-pages --single-branch https://github.com/kbase/kb_sdk_docs build/html
```

#### Deploy

From the repo's root directory.

```sh
$ pipenv run make html
$ cd build/html
$ git add --all
$ git commit -m 'Build'
$ git push -u origin gh-pages
```
