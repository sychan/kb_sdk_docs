# KBase SDK Documentation

This repo holds the source code for [Sphinx](http://www.sphinx-doc.org/en/master/) documentation describing the KBase Software Development Kit.

[**You can visit the documentation website here**](http://kbase.github.io/kb_sdk_docs)

## Editing .rst files

Most of the documentation files use the "reStructuredText" format. Read the primer here: http://www.sphinx-doc.org/en/1.5/rest.html

There is also full support for using markdown files with the `.md` extension. However, you will be missing many features from reStructuredText, such as the ability to [document functions and parameters](http://www.sphinx-doc.org/en/1.5/tutorial.html#documenting-objects).

A good reference on reStructuredText directives is here: http://docutils.sourceforge.net/docs/ref/rst/directives.html

## Development

1. Make sure [pipenv](https://docs.pipenv.org/) is installed
1. Run `pipenv install` in the root directory of this repo on the master branch
1. Run `pipenv run make live` to launch the local development server and open `localhost:8000` in your browser. The server will automatically rebuild files and refresh your browser when you change a file.
1. To validate your RST and code block syntax, run `make check`.

If you want to add a dependency, make sure to use `pipenv install` to have it added in the `Pipfile`.

## How it works

Sphinx is a documentation website generator. It takes all of the `.md` and `.rst` markup files found in `/source/` and converts it to an HTML tree. Running `pipenv run make live` creates a dynamic server that builds all your markup on every file change.

The actual website is hosted on Github Pages and serves the `gh-pages` branch, which has all the HTML files. As an editor of the documentation source code, you don't need to worry about this branch.
 
To add a page and have it show up in the sidebar navigation, you need to add it to one of the table-of-contents sections in `index.rst`. Those `toctree` sections determine what gets listed in the navigation.

Read [Getting Started with Sphinx](http://www.sphinx-doc.org/en/1.5/tutorial.html#defining-document-structure) (Ignore the installation section)

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
