ContigFilter Example
========================

The |helloWorld_link| example demonstrated where various components of the SDK are located and how to run a basic test.
This "ContigFilter" example adds more features of the SDK. In practice, apps need to interact with the workspace/narrative, create reports from the app output, and potentially interact with database objects.

For this tutorial, we will use an example module that has a few more specific details so we can explore how to make
changes. The example is taken from |contigFilter_link|  if you get stuck at any point and want to see the original.  For this example module use:

.. |helloWorld_link| raw:: html

   <a href="helloworld.html" target="_blank">Hello World</a>

.. |contigFilter_link| raw:: html

   <a href="https://github.com/msneddon/ContigFilter" target="_blank">https://github.com/msneddon/ContigFilter</a>

.. code-block:: bash

    kb-sdk init --example --language python --user <your_kbase_user_name> <user_name>ContigFilter
    cd <user_name>ContigFilter
    make

Because this example is
used in training and is tried by many users, the ``<user_name>`` is added to make sure that your module has a unique 
name. However, you would not usually put your own username in the module name, and instead name it something 
like ``ContigFilter``.

From here on, the ``<user_name>ContigFilter`` will simply be called ``module_name``.

The setup for the "ContigFilter" module includes:

#. Customizing the description of the module
#. Specify inputs, outputs, and functions of the module
#. Validate the module and its apps
#. Customize the Narrative User Interface

Module Description
-------------------------------------------

Open and edit the ``kbase.yml`` file to include a better description of your module. It is in the root directory of the module. Be sure to describe what your module and its apps actually do. In future revisions you can update the version and the list of authors/owners.

For this tutorial, we will be looking at the app ``filter_contigs``. The app takes as inputs, an ``assembly file`` and applies 
a ``minimum length`` filter to the contigs. **The exercise below creates a second app that filter the contigs 
by a** ``maximum_length``

The Specification File
-------------------------------------------

The specification file is called ``module_name.spec`` and is in the root directory of the module. 
This file is highly structured and follows a KIDL type definition. The steps below show the sections that need
to be modified for the ContigFilter module. 

View the |KIDL_link| 


.. |KIDL_link| raw:: html

   <a href="../references/KIDL_spec.html" target="_blank">KIDL tutorial and reference </a>


Plan the Inputs, Outputs, and Functions
```````````````````````````````````````````

For purposes of this exercise we will consider 3 inputs, 6 outputs, and 2 functions. The specification can contain
additional specifications that don't apply here. For example, not all functions become apps. Functions that have
a defined user interface will become apps.

**Inputs:**

The exercise includes the following inputs from the user:

* A reference to an assembly
* A ``min_length``  (the minimum contig length that the user wants)

In addition, the narrative will send the following input to the apps:

* The name of the workspace (always needed when working with data)

In SDK, input parameters can be in the form of maps (dicts/hashes/objects), lists (arrays), floats, integers, 
strings, or booleans.  Sometimes, input strings will actually be **reference addresses** to files on 
KBase's workspace servers. The scripts include some code to download the files from the workspace.

**Outputs:**

This exercise includes the following outputs:

* Number of starting contigs
* Number of contigs removed
* Number of remaining contigs
* A reference to the new assembly 
* A report name 
* A report reference

The ``report name`` and ``report reference`` are used internally by the user interface for directing output
to the right place on the screen.

In SDK, output can be the following data:

* KBase Typed Data - Assemblies, genomes, annotations, etc.
* HTML Pages - A formatted page representing the output of your app
* Misc. files for download - Your method can use KBaseReports to save results to a file server for the user to download

**Functions:**

* ``filter_contigs`` - Existing app which filters contigs using a minimum contig length
* ``filter_contigs_max`` - New app which filter the contigs using both a minimum and a maximum contig length

Edit the Spec file with KIDL 
`````````````````````````````

The syntax comes from a custom type language called KIDL, which is used as a common interface definition language, allowing different apps to communicate with one another, regardless of programming languages.

`View the |KIDL_link| 

We'll start with the ``module_name.spec`` file. **Comments** in KIDL start with a line with ``/*`` and end with a 
line with ``*/``. 
For example, the following has two comments and an empty specification for the module called MyContigFilter.

.. code-block:: cpp

    /*
		A KBase module: ContigFilter
    */
    module MyContigFilter {
        /*
            Insert your typespec information here.
        */
    };

The ContigFilter ``.spec`` file has a lot of comments that may seem distracting at first glance.  For inputs, we need 
a ``min_length`` parameter (an integer), an ``assembly_input_ref`` parameter (a string reference to an assembly 
file in the workspace), and a ``workspace_name``.  Here are the needed statements to define the inputs
(comments removed):

.. code-block:: cpp

     module ContigFilter {
        typedef string assembly_ref;

        typedef structure {
            assembly_ref assembly_input_ref;
            string workspace_name;
            int min_length;
        } FilterContigsParams;
    };

There are two typedefs which define two parameters, an ``assembly_ref`` which is a string and 
a set of input parameters that when combined into a ``structure`` are called ``FilterContgsParams``. 
The format of a ``typedef`` is the template or type followed by the name of the parameter.
As mentioned above, the three input parameters of ``FilterContgsParams`` are a 
``min_length`` of type ``int``, a ``workspace_name`` of type string, and an ``assembly_input_ref`` which is 
a reference to an assembly. Because a reference to an assembly (``assembly_ref``) is not a defined type, 
one was defined. 

Edit your KIDL ``.spec`` file to add the lines needed for a new app that filters using both a minimum and a
maximum length. Your new ``.spec`` file might look something like this:

.. code-block:: cpp

     module ContigFilter {
        typedef string assembly_ref;

        typedef structure {
            assembly_ref assembly_input_ref;
            string workspace_name;
            int min_length;
        } FilterContigsParams;

        typedef structure {
            assembly_ref assembly_input_ref;
            string workspace_name;
            int min_length;
            int max_length;
        } FilterContigsMaxParams;
    };

Now let's look at the outputs. In the ContigFilter module, the following ``typedef`` lines define the outputs:

.. code-block:: cpp

    typedef structure {
        string report_name;
        string report_ref;
        assembly_ref assembly_output;
        int n_initial_contigs;
        int n_contigs_removed;
        int n_contigs_remaining;
    } ContigFilterResults;

This has added a ``structure`` are called ``ContigFilterResults`` as the parameters for output. 
The ``report_name`` and ``report_ref`` are placeholders for the output results, which we will return to later. 
The assembly_output can use the same type as used above and there are three outputs of type ``int``. 
The new app can use the same output parameters and doesn't need a new ``structure``.

Now let us look at the function type for our app, which we can call ``filter_contigs``. 
Refer to the |KIDL_link| for details about function types.

.. code-block:: cpp

    funcdef filter_contigs(FilterContigParams params)
        returns (FilterContigsResults) authentication required;

This function definition (``funcdef``) defines a function called ``filter_contigs`` with input parameters of
``FilterContgsParams`` and returns output parameters of ``ContigFilterResults``.
The function is set as ``authentication required`` because all SDK apps that run in the 
Narrative will require the authentication to interact with a user's workspace.

Edit your KIDL ``.spec`` file to add the lines needed for a new app that filters using both a minimum and a
maximum length. Your new ``.spec`` file might look something like this:

.. code-block:: cpp

     module ContigFilter {
        typedef string assembly_ref;

        typedef structure {
            assembly_ref assembly_input_ref;
            string workspace_name;
            int min_length;
        } FilterContigsParams;

        typedef structure {
            assembly_ref assembly_input_ref;
            string workspace_name;
            int min_length;
            int max_length;
        } FilterContigsMaxParams;

    typedef structure {
        string report_name;
        string report_ref;
        assembly_ref assembly_output;
        int n_initial_contigs;
        int n_contigs_removed;
        int n_contigs_remaining;
    } FilterContigsResults;

    funcdef filter_contigs(FilterContigsParams params)
        returns (FilterContigsResults output) authentication required;

    funcdef filter_contigs_max(FilterContigsMaxParams params)
        returns (FilterContigsResults output) authentication required;

    };


Now return to your module's root directory and run ``make``. 

.. important::

    You must rerun *make* after each change to the KIDL specification to regenerate client and server code used in the codebase. 


Validate your module
---------------------

When you make changes to your KIDL ``.spec`` file, validate the syntax of your changes by running:

.. code-block:: bash

    $ kb-sdk validate


For now, you will get an error that looks something like this:

.. code:: bash

    **ERROR** - unknown method "your_method" defined within path [behavior/service-mapping/method] in spec.json

That's because we need to set up some things for the User Interface in the ``ui/narrative/methods`` directory 
in the module.
