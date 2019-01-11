Narrative User Interface
========================

Apps on KBase, such as genome assemblers and annotators that run on narrative pages, are created using the KBase SDK.
The user interface (UI) that users see in narratives and in the app catalog are defined in two files: 
``spec.json`` and ``display.yaml``. These two files are in a directory called ``ui/narrative/methods/<app_name>``
where ``<app_name>`` is the name of the function that you want to turn into an app. In the example module,
the directory  ``ui/narrative/methods/run_{uid}ContigFilter`` exists and you need to create a directory called
``ui/narrative/methods/run_{uid}ContigFilter_max``.

.. note::

    For historical reasons, in the user interface, methods and apps are synonymous. 

View |UIspec_link| for more information and options.

Update spec.json
-----------------

Copy the directory named ``ui/narrative/methods/run_{uid}ContigFilter`` and create a directory for the new app.

.. code-block:: bash

    # From your module's root directory:
    $ cd ui/narrative/methods/
    $ cp -r run_{uid}ContigFilter run_{uid}ContigFilter_max


``run_{uid}ContigFilter_max`` matches the ``funcdef`` name we used in the KIDL ``{uid}ContigFilter.spec`` file.

Now open up ``ui/narrative/methods/run_{uid}ContigFilter_max/spec.json``. This file defines a mapping between our 
KIDL ``.spec`` file and how our parameters will show up in the app's user interface.

In the section under ``parameters``, there are two input parameters:

.. code:: json

    {
        "parameters": [
            {
                "id": "assembly_input_ref",
                "optional": false,
                "advanced": false,
                "allow_multiple": false,
                "default_values": [ "" ],
                "field_type": "text",
                "text_options": {
                    "valid_ws_types": [ "KBaseGenomeAnnotations.Assembly", "KBaseGenomes.ContigSet" ]
                }
            },
            {
                "id": "min_length",
                "optional": false,
                "advanced": false,
                "allow_multiple": false,
                "default_values": [ "" ],
                "field_type": "text",
                "text_options": {
                    "validate_as": "int",
                    "min_integer": "0"
                }
            }
        ]
    }

These options will generate UI form elements in the narrative that allow the user to input data into your app. 
We leave out the ``workspace_name`` parameter because it will automatically be provided by the system, 
not the user, so we don't need a form element for it.

Each parameter object has a number of options.

* We want both parameters to be required ("optional": false)
* We want the ``assembly_input_ref`` to be a reference to either an Assembly or ContigSet object (view the |typeCatalog_link| ) to see all KBase types)
* We want the ``min_length`` parameter to be validated as an integer, and we don't want to allow negative numbers (minimum valid integer to be 0).

Edit the file to add the other input parameter ``max_length`` with similar values. The end of the parameters section should have something like this.

.. code:: json

    [
        {
            "id": "min_length",
            "optional": false,
            "advanced": false,
            "allow_multiple": false,
            "default_values": [ "" ],
            "field_type": "text",
            "text_options": {
                "validate_as": "int",
                "min_integer" : 0
            }
        },
        {
            "id": "max_length",
            "optional": false,
            "advanced": false,
            "allow_multiple": false,
            "default_values": [ "99999999" ],
            "field_type": "text",
            "text_options": {
                "validate_as": "int",
                "min_integer" : 0
            }
        }  
    ]

Notice that a comma was added to the end of the ``min_length`` parameter.

Below parameters, in the section under ``behavior``, change ``run_{uid}ContigFilter`` to  ``run_{uid}ContigFilter_max``. Note that ``name`` is the name of the module and doesn't change and ``method`` is the name of the app.

.. code:: json

    {
        "service-mapping": {
            "url": "",
            "name":"ContigFilter",
            "method": "run_{uid}ContigFilter_max"
        }
    }


Also in the ``behavior`` section, you will see ``input_mapping`` options. It contains entries for the input 
parameters.

.. code:: json 

    {
        "input_mapping": [
            {
                "narrative_system_variable": "workspace",
                "target_property": "workspace_name"
            },
            {
                "input_parameter": "assembly_input_ref",
                "target_property": "assembly_input_ref",
                "target_type_transform": "resolved-ref"
            },
            {
                "input_parameter": "min_length",
                "target_property": "min_length"
            }
        ]
    }


Notice that we added a ``target_type_transform`` option with the value ``resolved-ref`` for the 
``assembly_ref`` input. This indicates to the narrative that this parameter needs to be a valid reference 
to an object in the workspace.

Add the ``max_length to the ``input_mapping``. The lines will look something like:

.. code:: json 

    [
        {
            "input_parameter": "min_length",
            "target_property": "min_length"
        },
        {
            "input_parameter": "max_length",
            "target_property": "max_length"
        }
    ]

Make sure you include the commas after the min_length parameters to maintain valid JSON syntax. We don't need to change the output section.

When you make changes to UI files, you can validate the syntax of your changes by running:

.. code-block:: bash

    $ kb-sdk validate

When you run ``kb-sdk validate``, you will get an error about your ``display.yaml``, which we will update next.

Update display.yaml
-------------------

The YAML file found in ``ui/narrative/methods/run_{uid}ContigFilter/display.yaml`` holds text content for your app. The text written here will show up in the narrative and in the  |Catalog_link| 
for each form element. You only need to set this text for parameters that actually display in the form.

.. note::

    Compare these screenshots of the narrative and App Catalog images of the app "View flux network" with
    the specifications in its |displyYAML_link| . If screenshots are included, they appear between the ``tooltip`` and the ``description``.

.. figure:: ../images/View_flux_network_narr.png
    :align: center
    :figclass: align-center

    View Flux Network App in a narrative.

.. figure:: ../images/ViewFluxNetwork_cat.png
    :align: center
    :width: 90%
    :figclass: align-center

    App Catalog for View Flux Network.


Open the ``display.yaml`` and update its ``name`` and ``tooltip`` to say something related to filtering assembly files 
based on contig length with both a min and a max filter.

You can leave the "screenshots", "icon" and "suggestions" fields to their default values.

.. tip::

    The icon is completely optional but will come in handy when you get to the "Publish and Update" step. It will help you find your app in a sea of others that have the same name. The |UIspec_link| has more information on icons.

Moving down to the "parameters" section, the parameter entries for "assembly_ref" and "min_length" are filled in. 

.. code-block:: yaml

    parameters:
        assembly_ref:
            ui-name: Assembly to filter
            short-hint: |
                Genome assembly with contiguous fragments
            long-hint: |
                Genome assembly where we want to filter out fragments that are below a minimum
        min_length:
            ui-name: |
                Min contig length
            short-hint: |
                Minimum required length of every contig in the assembly
            long-hint: |
                All contigs will be filtered out of the assembly that are shorter than the given length

Edit the file and add the ``max_length`` parameter. The new lines might look like:

.. code-block:: yaml

        ...
            max_length:
            ui-name: |
                Maximum contig length
            short-hint: |
                Maximum required length of every contig in the assembly
            long-hint: |
                All contigs will be filtered out of the assembly that are longer than the given length
        ...


Finally, run ``kb-sdk validate`` again and it should pass! Now we can start to actually work on the functionality of the module and its apps.

.. note::

    For a more exhaustive overview of the ``spec.json`` and ``display.yaml`` files, take a look at
    the |UIspec_link|  You can also experiment with UI generation
    with the |AppSpec_link| 

.. External links

.. |AppSpec_link| raw:: html

   <a href="https://narrative.kbase.us/narrative/ws.30118.obj.1" target="_blank">App Spec Editor Narrative</a>

.. |typeCatalog_link| raw:: html

   <a href="https://narrative.kbase.us/#catalog/datatypes" target="_blank">type catalog</a>

.. |Catalog_link| raw:: html

   <a href="https://narrative.kbase.us/#appcatalog" target="_blank">App Catalog</a>

.. |displyYAML_link| raw:: html

   <a href="https://github.com/kbaseapps/fba_tools/blob/master/ui/narrative/methods/view_flux_network/display.yaml" target="_blank">display.yaml file</a>

.. Internal links

.. |UIspec_link| raw:: html

   <a href="../references/UI_spec.html">Narrative App UI Specification</a>

