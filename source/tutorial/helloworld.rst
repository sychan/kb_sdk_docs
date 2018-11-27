Hello World Example
========================

This "Hello World" example demonstrates where various components of the SDK are located and how to run a basic test. Most of this example is optional. The |ContigFilter_link| example adds more features of the SDK. 

For this example substitute your name/id for {uid} and use:

.. code-block:: bash

    kb-sdk init --language python --user {your_kbase_user_name} {uid}HelloWorld
    cd {uid}HelloWorld
    make


The setup for the module includes:

#. Customizing the description of the module
#. Specify inputs, outputs, and functions of the module
#. Validate the module and its apps
#. Customize the Narrative User Interface
#. Add statements to the app
#. Run a first test

Module Description
-------------------------------------------

In the root directory of the module is the file ``kbase.yml``.  Examine the file. 
It has a placeholder for information that you will eventually add to the module.
The ``module-name`` and ``service-language`` will not change but the other items will change as 
you prepare the module for release or update. 
The default ``module-description`` doesn't say anything about your module. You will want to edit this
before the module is released.  Be sure to describe what your module and its app will do. 
The ``module-version`` needs to be incremented before updates can be released. For the first version
of an app, it can remain *0.0.1*.
The ``owners`` is a list of users that can approve updates to a module.
The information in this file shows up in the  |ModuleCatalog_link| entry for the module. A more complete example is  |kb_SPAdes_link|.

The Specification File
-------------------------------------------

The specification file (or spec file) is called ``{uid}HelloWorld.spec`` and is in the root directory of the module. 
This file is highly structured and follows a KBase-specific Interface Description Language or KIDL type definition. 
IDLs allow different apps to communicate with one another, regardless of programming languages.
The steps below show the sections that are part of the HelloWorld module. 

|KIDLspecref_link|

The purpose of the spec file is to define:

* The inputs. For the HelloWorld module is a phrase.
* The outputs. For the HelloWorld module is a phrase.
* The functions. A default function will be created called run_{uid}HelloWorld - It will also be called an app or a method depending on context.

Plan the Inputs, Outputs, and Functions
```````````````````````````````````````````

First, a look at the default spec file, ``{uid}HelloWorld.spec``.

.. code:: text

    /*
       A KBase module: {uid}HelloWorld
    */

    module {uid}HelloWorld {
        typedef structure {
            string report_name;
            string report_ref;
        } ReportResults;

       /*
           This example function accepts any number of parameters and returns results in a KBaseReport
       */
       funcdef run_{uid}HelloWorld(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;
    };


The spec file has two comments. Comments in KIDL start with a line with ``/*`` and end with a 
line with ``*/``. The first comment is ``A KBase module: {uid}HelloWorld`` and the second comment is
``This example function accepts any number of parameters and returns results in a KBaseReport``.

The definition of the module starts with ``module {uid}HelloWorld {`` and ends with ``};``

Within the module definition is a ``typedef`` which can be used to define either inputs or outputs.
By default, the output called 'ReportResults' is defined. It is used to send results back to the user.  

The module definition also has a ``funcdef`` which is used to define functions. 
The function ``run_{uid}HelloWorld`` has been defined. Functions can become apps that are exposed 
in the user interface or they can be internal functions used by other apps. As seen in steps below, 
the default ``run_{uid}HelloWorld`` function will be exposed as an app. Internal functions are 
often found in modules with 'Util' in their name (|ModuleCatalog_link|).

The example module called |ContigFilter_link| goes through inputs, outputs, and functions in more detail.

Add input and output
```````````````````````

The HelloWorld module will run without making any changes. This step is optional for the HelloWorld module.

An input can be added to the HelloWorld spec file that captures a phrase the user inputs. 
In this example, we will call it parameter_1 and it will be a string. 

.. code:: text

        typedef structure {
            string parameter_1;
        } InParams;

We can also add an output string that is returned to the user. It will be added to the ReportResults 
and we will reuse parameter_1. Normally the input and output parameters are different types so they
can't share a name. The spec file now has:
 
.. code:: text

    /*
       A KBase module: {uid}HelloWorld
    */

    module {uid}HelloWorld {
        typedef structure {
            string parameter_1;
        } InParams;
        typedef structure {
            string parameter_1;
            string report_name;
            string report_ref;
        } ReportResults;

       /*
           This example function accepts any number of parameters and returns results in a KBaseReport
       */
       funcdef run_{uid}HelloWorld(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;
    };

If you made any changes, return to your module's root directory and run ``make``. 

.. important::

    You must rerun *make* after each change to the KIDL specification to regenerate client and server code used in the codebase. 

Refer to the |KIDLspec_link|  for details about function types.


Validate your app
---------------------

When you make changes to your KIDL ``{uid}HelloWorld.spec`` file, validate the syntax of your changes by running:

.. code-block:: bash

    $ kb-sdk validate


If you get **WARNING** or **ERROR** messages similar to this:

.. code:: text

    xyzxyzxyz


.. code:: text

    **ERROR** - unknown method "your_method" defined within path [behavior/service-mapping/method] in spec.json


you either have a typo or you need to add something to the Narrative user interface (next section).

Edit the Narrative UI (optional)
--------------------------------

The specifications for the app's Narrative user interface are under the directory named 
``/ui/narrative/methods/run_{uid}HelloWorld``. Note that the name of the directory is the same as
the name of the function in the spec file above. Functions that become user-facing apps need a 
directory that defines the user interface.

This directory has two files ``spec.json`` and ``display.yaml``. The example module |ContigFilter_link| 
will go into more depth for these files.  The  |Documenting_link| page provides
information on the purpose of the subdirectory ``img``.

If you added input and output parameters to the spec file above, you can now add them to the Narrative UI.

Now open up ``ui/narrative/methods/run_{uid}HelloWorld/spec.json``. This file is in JSON format and 
defines a mapping between our KIDL ``{uid}HelloWorld.spec`` file and how our parameters will show up in the app's user interface.

In the section ``parameters`` already defines ``parameter_1``:

.. code:: json

    {
        "parameters": [
            {
                "id": "parameter_1",
                "optional": false,
                "advanced": false,
                "allow_multiple": false,
                "default_values": [ "" ],
                "field_type": "text",
                "text_options": {
                    "valid_ws_types": [ ]
                }
            }
        ]
    }

Additional parameters added to the spec file  need to be added to this section. This will be covered
in the next example module.

The ``behavior`` section describes how the parameters from the Narrative UI are passed to the 
Python code. For example, the Narrative UI automatically passes two hidden parameters about the
narrative, the ``workspace`` and the ``workspace_id``. When these are passed to the Python code,
the ``workspace`` from the Narrative is passed as ``workspace_name`` to Python.

The ``display.yaml`` file is in YAML format and defines how your app will appear in the |AppCatalog_link|.
Examine the file found in ``ui/narrative/methods/printhelloworld/display.yaml``.
View the |Documenting_link| page for more on the how this file is used.

Finally, if you made any changes, run ``kb-sdk validate`` again and make sure it passes! 
Now we can start to work on the functionality of the app.

.. note::

    For a more exhaustive overview of the ``spec.json`` and ``display.yaml`` files, take a look at
    the |UISpec_link|.  You can also experiment with UI generation
    with the |AppSpec_link| 

Code Implementation
-------------------

The actual code for your app will live in the python package under ``lib/{uid}HelloWorld``. 
The entry point, where your code is initially called, lives in the file: ``lib/{uid}HelloWorld/{uid}HelloWorldImpl.py``. 
It is sometimes called the "Implementation" file or simply the "Impl" file.  This is the file where you edit your own Python code.

This "Implementation" file defines the python methods available in the module. All of the functions
defined in the spec file correspond to Python methods 
and they are part of the class inside ``{uid}HelloWorldImpl.py``. 

Much of the Implementation file is auto-generated based on the spec file. The ``make`` command updates the Implementation file. To separate auto-generated code from developer code, developer code belongs between ``#BEGIN`` and ``#END`` comments. For example:

.. code-block:: python

        #BEGIN_HEADER
        #END_HEADER

        #BEGIN_CLASS_HEADER
        #END_CLASS_HEADER

        #BEGIN_CONSTRUCTOR
        #END_CONSTRUCTOR

        #BEGIN printhelloworld
        #END printhelloworld

The ``make`` command preserves everything between the ``#BEGIN`` and ``#END`` comments and replaces everything else. 

.. warning::

    Don't put any spaces between the '#' and 'BEGIN' or 'END'. It has bad consequences.

Check Inputs (optional)
-----------------------

Open ``{uid}HelloWorldImpl.py`` and find the ``run_{uid}Helloworld`` method, which should have some auto-generated boilerplate code and docstrings.

You want to limit your code edits to regions between the comments ``#BEGIN run_{uid}Helloworld`` 
and ``#END run_{uid}Helloworld``. 
These are special SDK-generated annotations that we have to keep in the code to get everything to compile 
correctly. If you run ``make`` again in the future, it will update the code outside these comments, 
but will not change the code you put between the ``#BEGIN`` and ``#END`` comments.

Between the comments, add a simple print statement, such as: ``print ("Input parameter",params['parameter_1'])``. Then add the ``parameter_1`` to the output. 
This let us see what is getting passed into our method.

.. code-block:: python

        #BEGIN run_{uid}HelloWorld
        print ("Input parameter",params['parameter_1'])
        report = KBaseReport(self.callback_url)
        report_info = report.create({'report': {'objects_created':[],
                                                'text_message': params['parameter_1']},
                                                'workspace_name': params['workspace_name']})
        output = {
			'output': params['parameter_1'],
            'report_name': report_info['name'],
            'report_ref': report_info['ref'],
        }
        #END run_{uid}HelloWorld


Don't try to change the docstring, or anything else outside the ``BEGIN run_{uid}Helloworld`` and ``END run_{uid}Helloworld`` comments, as your change will get overwritten by the ``make`` command.

Run First Test
---------------------

.. note:

    Tests are an important part of KBase modules and are a requirement for release of apps. The module's root 
    directory has a directory called ``test``. All tests should be added to this directory. A template for 
    initial tests should be named after the module and in the ``test`` directory. When you enter ``kb-sdk test`` 
    at the command line, it will run the tests in the test directory. 


As a default, your ``{uid}HelloWorldImpl.py`` file is tested using ``test/{uid}HelloWorld_server_test.py``. This file has a some auto-generated boilerplate code.  Python will automatically run all methods that start with the name ``test``. 


Near the bottom of the test file, find the method ``test_your_method``. 
The default test is to call ``run_{uid}HelloWorld`` with
a ``workspace_name`` for the test and a ``parameter_1`` of 'Hello World'.  
If you added the optional parameters in the
earlier steps, you can modify the test method to test the returned output.

Add a simple print statement to the end of the test method:

.. code-block:: python

    print ("Output parameter",ret[0]['output']) 

.. note::

    Make sure that you have put your developer token in the ``test_local/test.cfg`` as mentioned in the
    |Initialize_link| 

Run ``kb-sdk test`` and, if everything works, you'll see the docker container boot up, the ``run_{uid}Helloworld`` method will get called, and you will see some printed output. 
If you added the input and output parameters, the output should include the two lines.

.. code:: text

    Input parameter Hello World!
    Output parameter Hello World!

When running an app, the messages created by the Impl file and the test will show up in the log. 
For this module, setting up the docker container will take the most time and generate the most lines in the log. 
The next example includes a report builder that is used by the Narrative User Interface.

.. External links

.. |kb_SPAdes_link| raw:: html

   <a href="https://narrative.kbase.us/#catalog/modules/kb_SPAdes" target="_blank">kb_SPAdes</a>

.. |AppSpec_link| raw:: html

  <a href="https://narrative.kbase.us/narrative/ws.28370.obj.1" target="_blank">App Spec Editor Narrative </a>

.. |ModuleCatalog_link| raw:: html

  <a href="https://narrative.kbase.us/#catalog/modules" target="_blank">Module Catalog </a>

.. |AppCatalog_link| raw:: html

  <a href="https://narrative.kbase.us/#appcatalog" target="_blank">App Catalog </a>

.. Internal links

.. |ContigFilter_link| raw:: html

   <a href="setup.html">ContigFilter</a>

.. |KIDLspecref_link| raw:: html

   <a href="../references/KIDL_spec.html">View the KIDL tutorial and reference.</a>

.. |KIDLspec_link| raw:: html

   <a href="../references/KIDL_spec.html">KIDL specification.</a>

.. |Initialize_link| raw:: html

  <a href="../tutorial/initialize.html">Initialize the Module </a>
             
.. |UISpec_link| raw:: html

  <a href="../references/UI_spec.html">UI specification guide </a>

.. |Documenting_link| raw:: html

  <a href="../howtos/fill_out_app_information.html">documenting your app</a>



