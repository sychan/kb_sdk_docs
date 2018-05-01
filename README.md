# KBase SDK Documentation

This repo holds the source code for [Sphinx](http://www.sphinx-doc.org/en/master/) documentation describing the KBase Software Development Kit.

[**You can visit the documentation website here**](http://kbase.github.io/kb_sdk_docs)

## Editing .rst files

All the documentation files use the "reStructuredText" format. Read the primer here: http://www.sphinx-doc.org/en/1.5/rest.html

A good reference on reStructuredText directives is here: http://docutils.sourceforge.net/docs/ref/rst/directives.html

## Development

1. Make sure [pipenv](https://docs.pipenv.org/) is installed
1. Run `pipenv install` in the root directory of this repo on the master branch
1. Run `make live` to launch the local development server and open `localhost:8000` in your browser. Your browser will refresh on any change.

If you want to add a dependency, make sure to use `pipenv` and have it added in the `Pipfile`.

### Deploying changes

> Note: Ignore this section if you just want to make docs updates and open a PR. The instructions below are for deploying changes from master to `gh-pages` and `kbase.github.io/kb_sdk_docs`

The `gh-pages` branch corresponds to the `/build/html` subdirectory. That directory is gitignored from master.

To deploy, you need write permissions for the `github.com/kbase/kb_sdk_docs` repository.

#### Setup

```sh
# Clone `gh-pages` into the html subdirectory
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
