ContigFilter Example
========================

The |helloWorld_link| example demonstrated where various components of the SDK are located and how to run a basic test.
This "ContigFilter" example adds more features of the SDK. In practice, apps need to interact with the workspace/narrative, create reports from the app output, and potentially interact with database objects.

For this tutorial, we will use an example module that has a few more specific details so we can explore how to make
changes. The example is taken from |contigFilter_link|  if you get stuck at any point and want to see the original.  For this example module use:

.. code-block:: bash

    kb-sdk init --example --language python --user <your_kbase_user_name> {uid}ContigFilter
    cd <user_name>ContigFilter
    make

Because this example is used in training and is built by many users, the ``{uid}`` is added to make sure that your module has a unique
name. However, you would not usually put your own username in the module name, and instead name it something like ``ContigFilter``.

From here on, the ``{uid}ContigFilter`` will simply be called ``{uid}ContigFilter``.

The setup for the "ContigFilter" module includes:

#. Customizing the description of the module
#. Specify inputs, outputs, and functions of the module
#. Validate the module and its apps
#. Customize the Narrative User Interface

Module Description
-------------------------------------------

Open and edit the ``kbase.yml`` file to include a better description of your module. It is in the root directory of the module. Be sure to describe what your module and its apps actually do. In future revisions you can update the version and the list of authors/owners.

For this tutorial, we will be looking at the app ``run_{uid}FilterContigs``. The app takes as inputs, an ``assembly file`` and applies
a ``minimum length`` filter to the contigs. **The exercise below creates a second app that filter the contigs 
by a** ``maximum_length``

The Specification File
-------------------------------------------

In the root directory of the module, there is a specification file is called ``{uid}ContigFilter.spec``.
This file uses a proprietary language called KIDL to specify functions the module that will be accessible to other SDK services as well as the input and output types.
The specification is used to generate clients code in any KBase supported language and so fileutils and modules that contain functions that are primarily used by other modules will have a detailed KIDL spec.
However, for module that are focused on defining user-facing apps, a very basic specification is all we need.

File structure
`````````````````````````````

If you open ``{uid}ContigFilter.spec``, you should see two types of specifications:

**Input/Output Structures**

.. : {uid}module ContigFilter {
    typedef structure {
        string report_name;
        string report_ref;
    } ReportResults;


The first section is actually describing the output from the example function. It describes an structure (in Python, a dictionary)
containing two keys ``report_name`` and ``report_ref`` that are used internally by the user interface to indicate which report object should be displayed in the app cell as the result.

As we'll later see, these reports can contain the following data:

* KBase Typed Data - Assemblies, genomes, annotations, etc.
* HTML Pages - A formatted page representing the output of your app
* Misc. files for download - Your method can use KBaseReports to save results to a file server for the user to download

**Functions:**

.. : funcdef run_{uid}ContigFilter_max(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;


The next section defines the functions that may be called by other SDK modules or app cells. In this case we find a single function called
``run_{uid}ContigFilter`` which filters contigs using a minimum contig length. As input it receives a mapping called ``params`` (also a dictionary in Python)
that is defined by an apps UI specification and produces the ``ReportResults`` structure described above.

Edit the Spec file
`````````````````````````````
Our new app will also receive parameters from the UI and create a report, so all we need to do copy the funcdef line and give the function a unique name.
We also should add a little description about our new function in a comment that precedes it. Once we are done, our ``{uid}ContigFilter.spec`` file should look like the following:

.. : {uid}module ContigFilter {
    typedef structure {
        string report_name;
        string report_ref;
    } ReportResults;

    /*
        Example app which filters contigs in an assembly using both a minimum contig length
    */
    funcdef run_{uid}ContigFilter(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;

    /*
        New app which filters contigs in an assembly using both a minimum and a maximum contig length
    */
    funcdef run_{uid}ContigFilter_max(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;

    };

Now return to your module's root directory and run ``make``. With that complete, it's time to specify the user interface for the new app.

.. important::

    You must rerun *make* after each change to the KIDL specification to regenerate implementation and server code used in the codebase.

.. External links

.. |helloWorld_link| raw:: html

   <a href="helloworld.html" target="_blank">Hello World</a>

.. |contigFilter_link| raw:: html

   <a href="https://github.com/kbaseapps/ContigFilter" target="_blank">https://github.com/kbaseapps/ContigFilter</a>

.. |KIDL_link| raw:: html

   <a href="../references/KIDL_spec.html" target="_blank">KIDL tutorial and reference </a>


