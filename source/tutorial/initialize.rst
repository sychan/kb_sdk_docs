Initialize the Module
=====================

.. tip::

   A KBase module contains all of the components for one or more apps. The components are all stored in
   the same directory and are all uploaded to a single github repository. There are pros and cons to 
   bundling several apps in one module versus
   creating one app per module. See `terminology  |terminology_link| for more information. 


The KBase SDK provides a way to quickly bootstrap a new module by generating most of the required components.

The basic options of the command are:

.. code-block:: bash

    $ kb-sdk init [--example] [--verbose] [--language language] [--user username] ModuleName

where ``ModuleName`` must be unique across all SDK modules registered in KBase. For example:


.. code-block:: bash

    $ kb-sdk init --language python --user <your_kbase_user_name> HelloWorld


This creates a directory called ``HelloWord`` in your current directory. The directory has all of 
the components needed to start creating apps and is basically a clean slate.  


You must always include the ``-u`` or ``-user`` option with your username to set yourself as the module owner.

You can set your programming langauge to any of Python, Perl, or Java; for this tutorial, we're using Python.

The ``kb-sdk init`` options are:

.. code::

    -v, --verbose    Show verbose output about which files and directories
                     are being created.
    -u  --user       Set the KBase username of the first module owner
    -e, --example    Populate your repo with an example module in the language
                     set by -l
    -l, --language   Choose a programming language to base your repo on.
                     Currently, we support Perl, Python, and Java. Default is
                     Python

Build the Module
---------------------

.. code-block:: bash

    $ cd module_name
    $ make

The ``make`` command will run some initial code generation.

Module Highlights
---------------------

This module template has the following that you will customize in later steps:

#. A directory called ``module_name`` (see `Anatomy of a Module <../references/module_anatomy.html>`_ for more on the directory sturcture)
#. A description of the module, its version, and the authors in ``kbase.yml``
#. A specification file that defines the inputs, output, and functions for the module ``module_name.spec``
#. A script with code for running all the apps in the module called ``module_nameImpl.py``
#. Specifications for the user interface in the files ``spec.json`` and ``display.yaml``

Don't worry the location of these files. They will be discussed in more detail in later steps.

Create a GitHub Repo
---------------------

You will need to publish your code to a public git repository to make it available for building into a custom Docker Image.  Here we'll show a brief example using GitHub.  First, commit your codebase into a local git repository. Then, ``git add`` all files created by kb-sdk and commit. This creates a git repository locally.

.. code:: bash

    $ cd MyModule
    $ git init
    $ git add .
    $ git commit -m 'Initial commit'


Now, create a new GitHub repository on github.com (it can be in your personal GitHub account or in an organization, but it must be public). Make sure your github repository is initially empty (don't add an initial README.md).

* Direct link to create a repo on github.  |github_link|.
* Github documentation about creating repos: |github_help_link|.

Sync your local codebase to your repository on github:

.. code:: bash

    $ git remote add origin https://github.com/[GITHUB_USER_OR_ORG_NAME]/[GITHUB_MODULE_NAME].git
    $ git push -u origin master


Remember to continuously push your code changes to your github repo by using ``git push``.

Set up your developer credentials
------------------------------------

If you want, this step can wait until you want to test your module. 
However, it is somewhat disruptive to the thought process if you wait until later.
This step can be done anytime after the first ``make`` of a module.

The KBase file storage services require authenticated access. During development a dev ``token`` is generated 
and used instead of putting user IDs and passwords in clear text in your module. 
Tokens are good for 90 days and can be used on all modules developed and tested during the 90 days.

Go to |authacct_link|, click **Developer Tokens**, and generate a new token. The
token is only visible on the screen for 5 minutes so make sure you are ready to do the step below.

From the module's root directory, copy and paste that token into ``test_local/test.cfg`` in the value 
for ``test_token``. For example:

.. code::

    test_token=JQGGVCPKCAB2XYHRHZV4H3NF4TN3YEUSA

Where you substitute your own test_token. This one is unauthorized.

.. External links

.. |terminology_link| raw:: html

   <a href="../references/terminology.html" target="_blank">terminology</a>

.. |github_link| raw:: html

   <a href="https://github.com/new" target="_blank">https://github.com/new</a>

.. |github_help_link| raw:: html

   <a href="https://help.github.com/articles/creating-a-new-repository" target="_blank">https://help.github.com/articles/creating-a-new-repository</a>


.. |authacct_link| raw:: html

   <a href="https://narrative.kbase.us/#auth2/account" target="_blank">https://narrative.kbase.us/#auth2/account</a>
