Implement Code
====================

.. important::
    If you are new to Python, here are a few tips:

    - Tabs are not used. Anything that is indented is filled with spaces.
    - Indentation is **not** optional. Python uses indentation to designate blocks of code that go together.
    - Indentation must be consistent. If you start with 4, 8 and 12 spaces being your indentation levels, stick with those throughout.
    - The pound sign (#) indicates comments to the end of the line.

The actual code for your app will live in the python package under ``lib/module_name``. The entry point, where your code is initially called, lives in the file: ``lib/module_name/module_nameImpl.py``. It is sometimes called the "Implementation" file or simply the "Impl" file.  This is the file where you edit your own Python code.

This "Implementation" file defines the python methods available in the module. Two of the methods correspond to our two apps and are named ``filter_contigs`` and ``filter_contigs_max`` and they are part of the class inside ``method_nameImpl.py``.

.. note ::

    In a real-world app, you may want to split up your code into several python modules and packages. You can place extra modules and folders inside ``lib/MyOtherModule`` and import them in ``lib/module_name/module_nameImpl.py``.

Much of the Implementation file is auto-generated based on the KIDL .spec file. The ``make`` command updates the Implementation file. To separate auto-generated code from developer code, developer code belongs between ``#BEGIN`` and ``#END`` comments. For example:

.. code-block:: python

        #BEGIN_HEADER
        #END_HEADER

        #BEGIN_CLASS_HEADER
        #END_CLASS_HEADER

        #BEGIN_CONSTRUCTOR
        #END_CONSTRUCTOR

        #BEGIN filter_contigs
        #END filter_contigs

The ``make`` command preserves everything between the ``#BEGIN`` and ``#END`` comments and replaces everything else.

.. warning::

    Don't put any spaces between the '#' and 'BEGIN' or 'END'. It has bad consequences.

The method for ``filter_contigs`` is a working app and has a lot of code between the ``#BEGIN`` and ``#END``.
The new app ``filter_contigs_max`` has nothing between the ``#BEGIN`` and ``#END``.

.. note::

    At this point, you could

    - take a short-cut and copy all the code from the ``filter_contigs`` method and paste it into the ``filter_contigs_max`` method.
    - make a few minor edits to add the filter for contigs exceeding the maximum length.
    - run ``kb-sdk test`` and see if everything works.

    The rest of this page is for those who want to understand how the code works and how to create tests for the
    code. It goes through the process of building up the code section, step-by-step.

Receive parameters
---------------------------

Our first goal is to receive and print the method's parameters. Open ``method_nameImpl.py`` and find the ``filter_contigs_max`` method, which should have some auto-generated boilerplate code and docstrings.

You want to edit code between the comments ``#BEGIN filter_contigs_max`` and ``#END filter_contigs_max``. These are special SDK-generated annotations that we have to keep in the code to get everything to compile correctly. If you run ``make`` again in the future, it will update the code outside these comments, but will not change the code you put between the ``#BEGIN`` and ``#END`` comments.

Between the comments, add a simple print statement, such as: ``print(params['min_length'], params['max_length'], params['assembly_ref'])``. This let us see what is getting passed into our method.


.. code-block:: python

    def filter_contigs_max(self, ctx, params):
        """
        :param workspace_name: instance of String
        :param params: instance of type "ContigFilterParams" (Input
           parameters) -> structure: parameter "min_length" of Long,
           parameter "assembly_ref" of String
        :returns: instance of type "ContigFilterResults" (Output results) ->
           structure:
        """
        # ctx is the context object
        # return variables are: returnVal
        #BEGIN filter_contigs_max
        print(params['min_length'], params['max_length'], params['assembly_ref'])
        output = {}
        #END filter_contigs_max
        return [output]

Don't try to change the docstring, or anything else outside the ``BEGIN filter_contigs_max`` and ``END filter_contigs_max`` comments, as your change will get overwritten by the ``make`` command.

Initialize a test
------------------

.. note:

    Tests are an important part of KBase modules and are a requirement for release of apps. The module's root 
    directory has a directory called ``test``. All tests should be added to this directory. A template for 
    initial tests should be named after the module and in the ``test`` directory. When you enter ``kb-sdk test`` 
    at the command line, it will run the tests in the test directory. 



Your ``module_nameImpl.py`` file is tested using ``test/module_nameImpl_server_test.py``. This file also has a variety of auto-generated boilerplate code and tests for the first app.  Python will automatically run all methods that start with the name ``test``. There are three tests for the old app. As a temporary measure, we will rename them so they don't run until we are done working on the new app.

- Change ``def test_filter_contigs_ok(self)``` to ``def my_test_filter_contigs_ok(self)``
- Change ``def test_filter_contigs_err1(self)`` to ``def my_test_filter_contigs_err1(self)``
- Change ``def test_filter_contigs_err2(self)`` to ``def my_test_filter_contigs_err2(self)``


Now add your own test for the new app method at the bottom of the test class and call it the ``test_filter_contigs_max(self)``.


.. code-block:: python

    def test_filter_contigs_max(self):
        ref = "79/16/1"
        result = self.getImpl().filter_contigs_max(self.getContext(), {
            'workspace_name': self.getWsName(),
            'assembly_ref': ref,
            'min_length': 100,
            'max_length': 1000000
        })
        print result
        # TODO -- assert some things (later)

We need to provide three parameters to our function: a workspace name, an assembly reference string, and a min length integer. For the reference string, we can use this sample reference to a *Shewanella oneidensis* assembly on AppDev: ``79/16/1``. You can always get a workspace name from the test class by using ``self.getWsName()``.

.. note::

    Make sure that you have put your developer token in the ``test_local/test.cfg`` as mentioned in the
     |initialize_link|


Run ``kb-sdk test`` and, if everything works, you'll see the docker container boot up, the ``filter_contigs_max`` method will get called, and you will see some printed output.

Set the callback URL and scratch path
-----------------------------------------

.. note::
	In this "ContigFilter" module, the steps in this section have already been done. They are included here so you can see why they were added to the basic module template.

The callback URL points to a server that is used to spin up other SDK apps that we will need to use in our own app. In our case, we want to use |Assembly_link| to validate and download genome data. When we use that app, our app makes a request to the callback server, which spins up a separate docker container that runs AssemblyUtil.

The other parameter we need is the path to the **scratch** directory. Scratch is a special directory that we can use to store files used to run the app. It is a shared directory that is also accessible by other apps, such as AssemblyUtil. You cannot use directories like ``/tmp`` when working with AssemblyUtil, because other apps won't have access to it.

.. note::

    The module_nameImpl.py code always uses the scratch directory to store files in your app.


.. important::
    
    Scratch is a temporary directory and only lasts as long as your app runs. When your app stops running, scratch files are gone. To generate persistent data, we can use Reports, which are described in more detail later on.

To enable callbacks and the scratch directory, this code was added into your ``__init__`` method in your ``module_nameImpl.py``, between the ``#BEGIN_CONSTRUCTOR`` and ``#END_CONSTRUCTOR`` comments:

.. code-block:: python

   ...
   # Inside your __init__ function:
   #BEGIN_CONSTRUCTOR
   self.callback_url = os.environ['SDK_CALLBACK_URL']
   self.shared_folder = config['scratch']
   #END_CONSTRUCTOR
   ...


Also added was an ``import os`` in the header of your ``module_nameImpl.py`` file, between the ``#BEGIN_HEADER`` and ``#END_HEADER`` comments.

We need to convert the reference to bacterial genome data, passed as an input parameter, into an actual FASTA file that our app can access. For that, we can use the |Assembly_link| app.

The app was installed from your repository's root directory with:

.. code-block:: bash

    $ kb-sdk install AssemblyUtil


That added an entry for ``AssemblyUtil`` to your ``dependencies.json`` file. It also added a python package under ``lib/installed_clients``. Other dependencies can be added the same way.

.. important::

    Don't forget to ``git add`` these new dependencies to your source control when you run kb-sdk install.

At the top of your ``module_nameImpl.py`` file, the module is imported with: 

.. code-block:: python

    from installed_clients.AssemblyUtilClient import AssemblyUtil

If you made any changes to this code, run the ``kb-sdk test`` command again to make sure you have no errors.

Add some basic validations
------------------------------------

It's good practice to make some run-time checks of the parameters passed into your ``module_nameImpl#filter_contigs_max`` method. While params will get checked in the Narrative UI, if your app ever gets called from another codebase, it will bypass any UI typechecks.

Make sure your user passes in a workspace, an assembly reference, a minimum length greater than zero, and a maximum length greater than zero:

.. code-block:: python

  ...
  # Inside filter_contigs_max(), after #BEGIN filter_contigs_max, before any other code
  # Check that the parameters are valid
  for name in ['min_length', 'max_length', 'assembly_ref', 'workspace_name']:
      if name not in params:
          raise ValueError('Parameter "' + name + '" is required but missing')
  if not isinstance(params['min_length'], int) or (params['min_length'] < 0):
      raise ValueError('Min length must be a non-negative integer')
  if not isinstance(params['max_length'], int) or (params['max_length'] < 0):
      raise ValueError('Max length must be a non-negative integer')
  if not isinstance(params['assembly_ref'], basestring) or not len(params['assembly_ref']):
      raise ValueError('Pass in a valid assembly reference string')
  ...

Feel free to add another test for the ``max_length`` being greater than the ``min_length``.

Re-run ``kb-sdk test`` to make sure everything still works.

Back to defining tests (``test/module_nameImpl_server_test.py``).
We can add some additional tests to make sure we raise ValueErrors for invalid parameters:

.. code-block:: python

    ...
    # Inside test/module_nameImpl_server_test.py
    # At the end of the test class
    def test_invalid_params(self):
        impl = self.getImpl()
        ctx = self.getContext()
        ws = self.getWsName()
        # Missing assembly ref
        with self.assertRaises(ValueError):
            impl.filter_contigs_max(ctx, {'workspace_name': ws,
                'min_length': 100, 'max_length': 1000000})
        # Missing min length
        with self.assertRaises(ValueError):
            impl.filter_contigs_max(ctx, {'workspace_name': ws, 'assembly_ref': 'x',
                'max_length': 1000000})
        # Min length is negative
        with self.assertRaises(ValueError):
            impl.filter_contigs_max(ctx, {'workspace_name': ws, 'assembly_ref': 'x',
                'min_length': -1, 'max_length': 1000000})
        # Min length is wrong type
        with self.assertRaises(ValueError):
            impl.filter_contigs_max(ctx, {'workspace_name': ws, 'assembly_ref': 'x',
                'min_length': 'x', 'max_length': 1000000})
        # Assembly ref is wrong type
        with self.assertRaises(ValueError):
            impl.filter_contigs_max(ctx, {'workspace_name': ws, 'assembly_ref': 1,
                'min_length': 1, 'max_length': 1000000})
    ...

Testing for invalid max_length is left as an exercise for the student.

Download the FASTA file
----------------------------

Back to the  ``method_nameImpl.py`` file.

Inside your ``filter_contigs_max`` method, initialize the utility and use it to download the ``assembly_ref``:

.. code-block:: python

    ...
    # Inside filter_contigs_max()
    assembly_util = AssemblyUtil(self.callback_url)
    fasta_file = assembly_util.get_assembly_as_fasta({'ref': params['assembly_ref']})
    print(fasta_file)
    ...


* We have to initialize AssemblyUtil by passing ``self.callback_url``
* The ``get_assembly_as_fasta`` method downloads a file from a workspace ref

Run ``kb-sdk test`` again and you should see the file download along with its path in the container.

Filter out contigs based on length
---------------------------------------

Now we can finally start to implement the real functionality of the app!

The biopython package (|biopython_link| ), included in the SDK build, has a module called SeqIO ( |SeqIO_link| ) that can help us read and filter genome sequence data.

This module should already be included in the module's ``module_nameImpl.py`` between the header comments like so:

.. code-block:: python

    ... # other imports
    from Bio import SeqIO
    ...


Now, inside ``filter_contigs_max``, enter code to filter out contigs less than the given min_length: or greater than the max_length.

.. code-block:: python

    ...
    # Inside module_nameImpl#filter_contigs_max, after you have fetched the fasta file:
    # Parse the downloaded file in FASTA format
    parsed_assembly = SeqIO.parse(fasta_file['path'], 'fasta')
    min_length = params['min_length']
    max_length = params['max_length']

    # Keep a list of contigs greater than min_length
    good_contigs = []
    # total contigs regardless of length
    n_total = 0
    # total contigs over the min_length
    n_remaining = 0
    for record in parsed_assembly:
        n_total += 1
        if len(record.seq) >= min_length and len(record.seq) <= max_length:
            good_contigs.append(record)
            n_remaining += 1
    output = {
        'n_total': n_total,
        'n_remaining': n_remaining
    }
    ...


Run ``kb-sdk test`` again and check the output.

Add real tests
---------------------

Return to ``test/module_nameImpl_server_test.py`` and add tests for the functionality we just added above.

Set ``min_length`` to a value that filters out some contigs but not others. In our case, our FASTA only has 2 sequences of lengths 4,969,811 and 161,613. An in-between minimum could be 200,000. To test the upper end, a minimum could be 100,000 and a maximum could be 400,000

We would expect to keep 1 contig and filter out the other.

.. code-block:: python

    ...
    # Inside module_nameImpl_server_test:
    def test_filter_contigs_test_min(self):
        ref = "79/16/1"
        params = {
            'workspace_name': self.getWsName(),
            'assembly_ref': ref,
            'min_length': 200000,
            'min_length': 6000000
        }
        result = self.getImpl().filter_contigs_max(self.getContext(), self.getWsName(), params)
        self.assertEqual(result[0]['n_total'], 2)
        self.assertEqual(result[0]['n_remaining'], 1)

    def test_filter_contigs_test_max(self):
        ref = "79/16/1"
        params = {
            'workspace_name': self.getWsName(),
            'assembly_ref': ref,
            'min_length': 100000,
            'min_length': 4000000
        }
        result = self.getImpl().filter_contigs_max(self.getContext(), self.getWsName(), params)
        self.assertEqual(result[0]['n_total'], 2)
        self.assertEqual(result[0]['n_remaining'], 1)
    ...


Run ``kb-sdk test`` again to make sure it all passes.

Output the filtered assembly
---------------------------------

Next, we want to save and upload a new version of our genome assembly data with the contigs filtered out.

Beneath the code that we wrote to filter the assembly, add this file saving and uploading code.

.. code-block:: python

    ...
    # Underneath your loop that filters contigs:
    # Create a file to hold the filtered data
    workspace_name = params['workspace_name']
    filtered_path = os.path.join(self.shared_folder, 'filtered.fasta')
    SeqIO.write(good_contigs, filtered_path, 'fasta')
    # Upload the filtered data to the workspace
    new_ref = assembly_util.save_assembly_from_fasta({
        'file': {'path': filtered_path},
        'workspace_name': workspace_name,
        'assembly_name': fasta_file['assembly_name']
    })
    output = {
        'n_total': n_total,
        'n_remaining': n_remaining,
        'filtered_assembly_ref': new_ref
    }
    #END filter_contigs_max
    ...


Add a simple assertion into your ``test_filter_contigs_max`` method to check for the ``filtered_assembly_ref``. Something like:

.. code-block:: python

    self.assertTrue(len(result[0]['filtered_assembly_ref']))


Run ``kb-sdk test`` again to make sure you have no errors

Build a report object
-------------------------

In order to output data into the UI inside a narrative, your app needs to build and return a KBaseReport ( |KBaseReport_link| ).

The following  KBaseReport app should be installed already:

.. code-block:: bash

    $ kb-sdk install KBaseReport


Import the report module should be between the ``#BEGIN_HEADER`` and ``#END_HEADER`` section of your ``module_nameImpl.py`` file:

.. code-block:: python

    from KBaseReport.KBaseReportClient import KBaseReport


The KBaseReport takes a series of dictionary objects that can have text messages, object references, and more. Add the report initialization code inside your ``filter_contigs_max`` method:

.. code-block:: python

    # Inside the filter_contigs_max method, below where we uploaded the new file:
    # Create an output summary message for the report
    text_message = "".join([
        'Filtered assembly to ',
        str(n_remaining),
        ' contigs out of ',
        str(n_total)
    ])
    # Data for creating the report, referencing the assembly we uploaded
    report_data = {
        'objects_created': [
            {'ref': new_ref, 'description': 'Filtered contigs'}
        ],
        'text_message': text_message
    }
    # Initialize the report
    kbase_report = KBaseReport(self.callback_url)
    report = kbase_report.create({
        'report': report_data,
        'workspace_name': workspace_name
    })
    # Return the report reference and name in our results
    output = {
        'report_ref': report['ref'],
        'report_name': report['name'],
        'n_total': n_total,
        'n_remaining': n_remaining,
        'filtered_assembly_ref': new_ref
    }
    #END filter_contigs_max


Add a couple assertions in our ``test_filter_contigs_max`` method inside ``test/module_nameImpl_server_test.py`` to check for the report name and ref:

.. code-block:: python

    ...
    self.assertTrue(len(result[0]['report_name']))
    self.assertTrue(len(result[0]['report_ref']))
    ...


Run ``kb-sdk test`` again to make sure it all works.

Configure your app's output data
-----------------------------------

We nearly have a complete app. The last step has already been added to our "ContigFilter" module. If starting from a blank template, you would take all the result data we defined in ``module_nameImpl#filter_contigs_max`` and add entries for them in our ``module_name.spec`` KIDL type file as well as our ``spec.json`` UI config file.

If not there already, add a type entry for our result data in our KIDL file:

.. code-block:: cpp

    /* Output results */
    typedef structure {
        string report_name;
        string report_ref;
        string filtered_assembly_ref;
        int n_total;
        int n_remaining;
    } ContigFilterResults;


Run ``make`` and ``kb-sdk test`` again to make sure everything works.

In your ``ui/narrative/methods/filter_contigs_max/spec.json`` file, if not there already, add entries for this output data:

.. code::

    ...
    "output_mapping": [
        {
            "service_method_output_path": [0,"report_name"],
            "target_property": "report_name"
        },
        {
            "service_method_output_path": [0,"report_ref"],
            "target_property": "report_ref"
        },
        {
            "narrative_system_variable": "workspace",
            "target_property": "workspace_name"
        }
    ]
    ...


Now we have some output entries that point to our report and workspace, which will show up when the job finishes in the narrative.

Finally, under ``widgets/output`` in the spec.json (near the top around line 10), set ``output`` to ``no-display``. This prevents our app from creating a separate output cell. It may already be set to ``no-display`` because that is the default.

.. code::

    ...
    "widgets": {
        "input": null,
        "output": "no-display"
    },
    ...

We've added an entry for everything we put in the ``output`` dictionary field that gets returned from ``module_nameImpl#filter_contigs_max``.

Run ``kb-sdk test`` a final time to make sure everything runs smoothly. If so, we have a working app!

Now that you are done with the new app, remember the three tests for the old app that we commented out? Time to uncomment them. In the test file ``test/module_nameImpl_server_test.py``:

- Change ``def my_test_filter_contigs_ok(self)``` to ``def test_filter_contigs_ok(self)``
- Change ``def my_test_filter_contigs_err1(self)`` to ``def test_filter_contigs_err1(self)``
- Change ``def my_test_filter_contigs_err2(self)`` to ``def test_filter_contigs_err2(self)``



.. External links

.. |Assembly_link| raw:: html

   <a href="https://github.com/kbaseapps/AssemblyUtil" target="_blank">AssemblyUtil </a>

.. |biopython_link| raw:: html

   <a href="http://biopython.org/" target="_blank">http://biopython.org/</a>

.. |SeqIO_link| raw:: html

   <a href="http://biopython.org/wiki/SeqIO" target="_blank">http://biopython.org/wiki/SeqIO</a>

.. |KBaseReport_link| raw:: html

   <a href="https://github.com/kbaseapps/KBaseReport" target="_blank">https://github.com/kbaseapps/KBaseReport</a>

.. Internal links

.. |initialize_link| raw:: html

   <a href="initialize.html">Initialize the Module</a>


