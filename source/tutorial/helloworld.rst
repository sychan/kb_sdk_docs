Hello World Example
========================

This "Hello World" example demonstrates where various components of the SDK are located and how to run a basic test. Some of the options in the blank template will be simplified or removed for this example. The |ContigFilter_link| example adds more features of the SDK. 


.. |ContigFilter_link| raw:: html

   <a href="setup.html" target="_blank">ContigFilter</a>

For this example use:

.. code-block:: bash

    kb-sdk init --language python --user <your_kbase_user_name> HelloWorld
    cd HelloWorld
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

Open and edit the ``kbase.yml`` file to include a better description of your module. It is in  is in the root 
directory of the module. Be sure to describe what your module and its app will do. The information here shows up
in the App Catalog entry for the module.  |kb_SPAdes_link|

.. |kb_SPAdes_link| raw:: html

   <a href="https://narrative.kbase.us/#catalog/modules/kb_SPAdes" target="_blank">See the page for kb_SPAdes. </a>


The Specification File
-------------------------------------------

The specification file is called ``HelloWorld.spec`` and is in the root directory of the module. 
This file is highly structured and follows a KIDL type definition. The steps below show the sections that need
to be modified for the HelloWorld module. 

|KIDLspec_link|

.. |KIDLspec_link| raw:: html

   <a href="../references/KIDL_spec.html" target="_blank">View the KIDL tutorial and reference.</a>



Plan the Inputs, Outputs, and Functions
```````````````````````````````````````````

* The input for the HelloWorld module is a phrase.
* The output for the HelloWorld module is a phrase.
* The function will be called printhelloworld - It will also be called an app or a method depending on context.


First, add the input parameter types to your app's KIDL specification, which lives in ``<module_name>.spec`` in the root directory of your codebase.

Edit the Spec file with KIDL 
`````````````````````````````

The syntax comes from a custom type language called KIDL, which is used as a common interface definition language, allowing different apps to communicate with one another, regardless of programming languages.

`View the KIDL tutorial and reference <../references/KIDL_spec.html>`_

We'll start with the ``HelloWorld.spec`` file. **Comments** in KIDL start with a line with ``/*`` and end with a 
line with ``*/``. 
For example, the following has two comments and an empty specification for the module called HelloWorld. Substitute the name of your app for "Hello World".


.. code-block:: cpp

    /*
		A KBase module: HelloWorld
    */
    module HelloWorld {
        /*
            Insert your typespec information here.
        */
    };

Expand the module definition to define the inputs (comments removed):

.. code-block:: cpp

     module HelloWorld {
        typedef structure {
            string phrase;
        } InParams;
    };

The inputs and outputs are defined with typedef statements and functions are defined with funcdef statements. The example module called |ContigFilter_link| has a more examples of what can occur in these definitions. 

Now let's look at the output. In the HelloWorld module, the following ``typedef`` lines define the outputs:

.. code-block:: cpp

     module HelloWorld {
        typedef structure {
            string phrase;
        } InParams;
        typedef structure {
            string phrase;
        } OutParams;
    };

Now let us look at the function declaration for our app, which we can call ``printhelloworld``. 

.. code-block:: cpp

     module HelloWorld {
        typedef structure {
            string phrase;
        } InParams;
        typedef structure {
            string phrase;
        } OutParams;
        funcdef printhelloworld(InParams params)
            returns (OutParams) authentication required;
    };

This function definition (``funcdef``) defines a function called ``printhelloworld`` with input parameters of
``InParams`` and returns output parameters of ``OutParams``.
The function is set as ``authentication required`` because all SDK apps that run in the 
Narrative will require the authentication to interact with a user's workspace. It isn't needed in this example but it is a good practice to get into.

Now return to your module's root directory and run ``make``. 

.. important::

    You must rerun *make* after each change to the KIDL specification to regenerate client and server code used in the codebase. 

Refer to the `KIDL specification <../references/KIDL_spec.html>`_ for details about function types.


Validate your app
---------------------

When you make changes to your KIDL ``HelloWorld.spec`` file, validate the syntax of your changes by running:

.. code-block:: bash

    $ kb-sdk validate


For now, you will get an error that looks something like this:

.. code:: bash

    **ERROR** - unknown method "your_method" defined within path [behavior/service-mapping/method] in spec.json


That's because we need to set up some things in our ``/ui/narrative`` directory in the app.

Update spec.json
--------------------

The directory named ``/ui/narrative/methods/example_method`` is a placeholder. Rename it to the name of the actual function we defined in our KIDL ``HelloWorld.spec`` file:

.. code-block:: bash

    # From your app's root directory:
    $ mv ui/narrative/methods/example_method ui/narrative/methods/printhelloworld


``printhelloworld`` matches the ``funcdef`` name we used in the KIDL ``HelloWorld.spec`` file.

Now open up ``ui/narrative/methods/printhelloworld/spec.json``. This file defines a mapping between our KIDL ``HelloWorld.spec`` file and how our parameters will show up in the app's user interface.

In the section under ``parameters``, you will define more details about your input parameter (change ``parameter_1`` to ``phrase``):

.. code:: json

    ...
    "parameters": [
        {
            "id": "phrase",
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
    ...


Find line 29 where it says ``your_method`` -- change that to say ``printhelloworld`` instead.

Below that section, you will see some default ``input_mapping`` options. Change that section so that it contains entries for each of your input and output parameters. 

.. code:: json 

    ...
    "input_mapping": [
        {
            "input_parameter": "phrase",
            "target_property": "phrase"
        }
    ],
    "output_mapping": [
    ]
    ...


When you run ``kb-sdk validate`` again, you will get an error about your ``display.yaml``, which we can update next.

Update display.yaml
-----------------------

The YAML file found in ``ui/narrative/methods/printhelloworld/display.yaml`` holds text content for your app.

In the ``parameters`` section change ``parameter_1`` to ``phrase``.  You can leave the rest of the template as-is. View `Fully documenting your app <../howtos/fill_out_app_information.html>`_ for more on the how this file is used.

Finally, run ``kb-sdk validate`` again and it should pass! Now we can start to actually work on the functionality of the app.

.. note::

    For a more exhaustive overview of the ``spec.json`` and ``display.yaml`` files, take a look at
    the `UI specification guide <../references/UI_spec.html>`_  You can also experiment with UI generation
    with the `App Spec Editor Narrative <https://narrative.kbase.us/narrative/ws.28370.obj.1>`_

Implement Code
---------------

The actual code for your app will live in the python package under ``lib/HelloWorld``. The entry point, where your code is initially called, lives in the file: ``lib/HelloWorld/HelloWorldImpl.py``. It is sometimes called the "Implementation" file or simply the "Impl" file.  This is the file where you edit your own Python code.

This "Implementation" file defines the python methods available in the module. The methods correspond to apps and they are part of the class inside ``HelloWorldImpl.py``. 

Much of the Implementation file is auto-generated based on the KIDL .spec file. The ``make`` command updates the Implementation file. To separate auto-generated code from developer code, developer code belongs between ``#BEGIN`` and ``#END`` comments. For example:

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

Receive and Return parameter
----------------------------

Open ``HelloWorldImpl.py`` and find the ``printhelloworld`` method, which should have some auto-generated boilerplate code and docstrings.

You want to edit code between the comments ``#BEGIN printhelloworld`` and ``#END printhelloworld``. These are special SDK-generated annotations that we have to keep in the code to get everything to compile correctly. If you run ``make`` again in the future, it will update the code outside these comments, but will not change the code you put between the ``#BEGIN`` and ``#END`` comments.

Between the comments, add a simple print statement, such as: ``print(params['phrase'])``. This let us see what is getting passed into our method.


.. code-block:: python

    def printhelloworld(self, ctx, params):
        """
        :param params: instance of type "InParams" (Insert your typespec
           information here.) -> structure: parameter "phrase" of String
        :returns: instance of type "OutParams" -> structure: parameter
           "phrase" of String
        """
        # ctx is the context object
        # return variables are: returnVal
        #BEGIN printhelloworld
        print "IMPL file and your phrase is: " + params['phrase'] + "\n"
        returnVal = {'phrase':params['phrase']}
        #END printhelloworld
        return [returnVal]

Don't try to change the docstring, or anything else outside the ``BEGIN printhelloworld`` and ``END printhelloworld`` comments, as your change will get overwritten by the ``make`` command.

Run First Test
---------------------

.. note:

    Tests are an important part of KBase modules and are a requirement for release of apps. The module's root 
    directory has a directory called ``test``. All tests should be added to this directory. A template for 
    initial tests should be named after the module and in the ``test`` directory. When you enter ``kb-sdk test`` 
    at the command line, it will runs the tests in the test directory. 


Your ``HelloWorldImpl.py`` file is tested using ``test/HelloWorldImpl_server_test.py``. This file also has a variety of auto-generated boilerplate code.  Phython will automatically run all all methods that start with the name ``test``. 


Near the bottom, find the method ``test_your_method``. For clarity, change the name of the method to ``test_printhelloworld``. Now modify the test method.

.. code-block:: python

    def test_printhelloworld(self):
        result = self.getImpl().printhelloworld(self.getContext(), {
            'phrase': "HelloWorld"
        })[0]
        print "TEST file and your phrase is " + result['phrase'] + "\n"

We need to provide one parameter to our function: a word phrase. 

.. note::

    Make sure that you have put your developer token in the ``test_local/test.cfg`` as mentioned in the
    `Initialize the Module <../tutorials/initialize.html>`_

Run ``kb-sdk test`` and, if everything works, you'll see the docker container boot up, the ``printhelloworld`` method will get called, and you will see some printed output.

When running an app, the messages created by the Impl file and the test will show up in the log. The next example includes a report builder that is used by the Narrative User Interface.
