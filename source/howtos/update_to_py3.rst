Updating a module to Python3
============================
Many KBase modules are written in Python 2.7 which will reach the end of it's service support in 2020. That means that beyond this date it will cease to receive critical security updates. Fortunately, it's fairly easy to convert any single module to Python 3 as long as the module has a reasonable comprehensive set of unit tests. In addition to the being secure for the future, these modules will access a host of performance and language features available in the 3.x releases.


Converting the source code with 2to3
------------------------------------
There are a number of compatibility tools available such as `six`_  or `futurize`_ which allow the execution of the same code by both Python 2 and Python 3 interpreters. Fortunately KBase's module system means that preserving back compatibility with Python 2 is not necessary and we can use the built in `2to3`_ executable. You can update all the python files in the directory in one fell swoop with the command ``2to3 -w <path_to_your_module>`` but you should be aware that you might no always agree with these changes (see more in Caveats). Additionally your Dockerfile will have  to be updated to build ``FROM kbase/sdkbase2:python``. Once the Dockerfile is updated, all future test will be run in Python 3, allowing you to verify that the updated code is running correctly. Check over the code to make sure that none of the caveats in the next section apply.

Caveats
-------
2to3 does a very good job catching most of the breaking changes between the python versions. However, there a few issues that are not caught by the library that may require some manual fixing:

- ``Maketrans`` imports from ``str`` in Python3 not ``string``.
- Strings in Python3 are by default unicode strings rather than byte strings. This solves many issues and inconsistencies but a few functions such as hashlib still expect byte strings. To produce these from a unicode string use the ``encode`` function like this: ``hashlib.md5(my_string.encode('utf-8')).hexdigest()``.
- ``2to3`` will wrap functions like ``dict.keys()`` ``dict.items()`` or ``range()`` in a ``list`` declaration to preserve the original behavior of these functions as they now return generators. This is the safest option and may be preferred in some cases (for example if the values are to be printed) but the intent is to iterate over the values, (e.x. ``for key in list(my_dict.keys())``) it is more efficient to remove this list extra declaration and use the generator.
- Some automatically generated files like ``baseclient.py`` and test files have fallback code for importing dependencies in 2 and 3 like this:

    .. code-block:: python

        try:
            from configparser import ConfigParser as _ConfigParser  # py 3
        except ImportError:
            from ConfigParser import ConfigParser as _ConfigParser  # py 2

  ``2to3`` will update the #py2 version to match the Python 3 syntax so it's recommended to ether revert this change or remove the try-except block

Resources
---------

* Neat new trick available in Python 3: `<http://whypy3.com/>`__
* Check Python 3 readiness of common packages: `<http://py3readiness.org/>`__


.. External links
.. _six: https://pythonhosted.org/six/
.. _futurize: http://python-future.org/automatic_conversion.html
.. _2to3: https://docs.python.org/2/library/2to3.htmlmended that you revert this change or simply use the