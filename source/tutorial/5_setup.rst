ContigFilter Example
========================

The |helloWorld_link| example demonstrated where various components of the SDK are located and how to run a basic test.
This "ContigFilter" example adds more features of the SDK. In practice, apps need to interact with the workspace/narrative, create reports from the app output, and potentially interact with database objects.

For this tutorial, we will use an example module that has a few more specific details so we can explore how to make
changes. The example is taken from |contigFilter_link|  if you get stuck at any point and want to see the original.

The setup for the "ContigFilter" module includes:

#. Running though the SDK App Design checklist
#. Customizing the description of the module
#. Specify inputs, outputs, and functions of the module

For this tutorial, we will be looking at the app ``run_{uid}FilterContigs``. The app takes as inputs, an ``assembly file`` and applies
a ``minimum length`` filter to the contigs. **The exercise below creates a second app that filter the contigs by a ``maximum_length``**

App Design Checklist
-------------------------------------------
When planing a new KBase app we recommend reviewing the |checklist_link| to help clarify the the inputs, outputs, and dependencies of the app.
An example for our second filter contig app is below:

- Does my App depend on any compiled or third-party packages?
    Yes, this app will use |biopython| in addition to new python code

    - If so, are they licenced with a KBase compliant licence?
        Yes the "Biopython License Agreement" is a compliant licence which allows commercial use.

    - If so, is there a version that is executable by one of the SDK languages or as a linux binary?
        Yes, in fact a version of biopython is available in the default SDK image

- What are the CPU and RAM requirements for my app?
    The CPU requirements will be minimal. The RAM consumption will be a similar order of magnitude to the length of the assembly file in our naive implementation.

- Does my App require reference/precomputed data?
    No, it will not.

- Does my App operate on KBase datatypes?
    - If so, do I need a FileUtil to convert the Workspace object to a desired file format?
        Yes, we will use AssemblyUtils.assembly_to_fasta to produce FASTA file for Biopython

- What parameters do I want users to be able to provide?
    * ``min_length`` - An integer describing minimum length for contigs in the assembly
    * ``max_length`` - A integer describing maximum length for contigs in the assembly

- Does my app produce any KBase DataTypes?
    - If so, do I need a FileUtil to save the desired file format as a Workspace object?
        Yes, we will use AssemblyUtils.fasta_to_assembly to save the FASTA file produced Biopython

- How do I want to display results to users?
    We will display a Report with a text message describing how many contigs are filtered out and how many remain as well as a link to the new assembly object.


Module Description
-------------------------------------------
Now that we have a clear idea of what our apps will do, we can begin to build it. Because this example is used in training and is built by many users, the ``{uid}`` is added to make sure that your module has a unique
name. However, you would not usually put your own username in the module name, and instead name it something like ``ContigFilter``.
 To get started, please run:

.. code-block:: bash

    kb-sdk init --example --language python --user <your_kbase_user_name> {uid}ContigFilter
    cd {uid}ContigFilter
    make

You can now edit the ``kbase.yml`` file to include a better description of your module. It is in the root directory of the module.
Be sure to describe what your module and its apps actually do. In future revisions you can update the version and the list of authors/owners.

The Specification File
-------------------------------------------

In the root directory of the module, there is a specification file is called ``{uid}ContigFilter.spec``.
This file uses a proprietary language called KIDL to specify which functions the module that will be accessible to other SDK services as well as the input and output types.
The specification is used to generate client code in any KBase supported language, therefore, FileUtils and modules that contain functions that are primarily used by other modules will have a detailed KIDL spec.
However, for modules that are focused on defining user-facing apps, a very basic specification is all we need.

File structure
`````````````````````````````

If you open ``{uid}ContigFilter.spec``, you should see two types of specifications:

**Input/Output Structures**

.. code-block:: none

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

.. code-block:: none

    funcdef run_{uid}ContigFilter(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;


The next section defines the functions that may be called by other SDK modules or app cells. In this case we find a single function called
``run_{uid}ContigFilter`` which filters contigs using a minimum contig length. As input it receives a mapping called ``params`` (also a dictionary in Python)
that is defined by an apps UI specification and produces the ``ReportResults`` structure described above.

Edit the Spec file
`````````````````````````````
Our new app will also receive parameters from the UI and create a report, so all we need to do copy the funcdef line and give the function a unique name.
We also should add a little description about our new function in a comment that precedes it. Once we are done, our ``{uid}ContigFilter.spec`` file should look like the following:

.. code-block:: none

    {uid}module ContigFilter {
        typedef structure {
            string report_name;
            string report_ref;
        } ReportResults;

        /*
            Example app which filters contigs in an assembly using both a minimum contig length
        */
        funcdef run{uid}ContigFilter(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;

        /*
            New app which filters contigs in an assembly using both a minimum and a maximum contig length
        */
        funcdef run{uid}ContigFilter_max(mapping<string,UnspecifiedObject> params) returns (ReportResults output) authentication required;

    };

Now return to your module's root directory and run ``make``. With that complete, it's time to specify the user interface for the new app.

.. important::

    You must rerun *make* after each change to the KIDL specification to regenerate implementation and server code used in the codebase.

.. External links

.. |contigFilter_link| raw:: html

   <a href="https://github.com/kbaseapps/ContigFilter" target="_blank">https://github.com/kbaseapps/ContigFilter</a>

.. |biopython| raw:: html

   <a href="https://biopython.org" target="_blank">Biopython</a>

.. Internal links

.. |helloWorld_link| raw:: html

   <a href="helloworld.html">Hello World</a>

.. |KIDL_link| raw:: html

   <a href="../references/KIDL_spec.html">KIDL tutorial and reference </a>

.. |UIspec_link| raw:: html

   <a href="../references/UI_spec.html">Narrative App UI Specification</a>

.. |checklist_link| raw:: html

   <a href="../references/design_checklist.html">Design Checklist</a>


