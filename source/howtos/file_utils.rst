Working with the file utils
===========================

Need to work with a type of file? There's a built-in utility for that.
Remember that utils run in separate containers that have a link to
scratch, so keep your files in scratch and upload them from there.

Where are the utils?
~~~~~~~~~~~~~~~~~~~~

Search for "Util" in `the catalog`_.

DataFileUtil, GenomeFileUtil, and AssemblyUtil are very commonly used.
Read the examples below for a quick idea on how to use them.

.. note::

    Upload local genome files for all of your tests. If you use an
    existing workspace reference in your tests, it won't work in the
    AppDev/CI environments.

DataFileUtil
------------

`DataFileUtil <https://github.com/kbaseapps/DataFileUtil>`__ is the
preferred general utility to fetch and save objects. Use GenomeFileUtil
or AssemblyUtil if you are specifically working with FASTA or GFF files.

*Initialize DataFileUtil client and get an object by reference.*

.. code-block:: py

    self.callback_url = os.environ['SDK_CALLBACK_URL']
    self.dfu = DataFileUtil(self.callback_url)
    genome_ref = "your/object_reference"
    genome_data = dfu.get_objects({'object_refs': [genome_ref]})['data'][0]
    genome_obj = genome_data['data']
    genome_meta = genome_data['info'][10]

*Upload a file or directory to shock*

.. code-block:: py

    # Upload a directory and zip it
    file = self.dfu.file_to_shock({"file_path": scratch_path, "pack": "zip"})
    file['shock_id']  # has the shock id

    # Upload a single file to shock
    file = self.dfu.file_to_shock({"file_path": scratch_path})["shock_id"]
    file['shock_id']  # has the shock id

*Save an object to the workspace and get an object reference*

.. code-block:: py

    save_object_params = {
        'id': workspace_id,
        'objects': [{
            'type': 'KBaseRNASeq.RNASeqSampleSet',
            'data': sample_set_data,
            'name': sample_set_object_name
        }]
    }

    dfu_oi = dfu.save_objects(save_object_params)[0]
    # Construct the workspace reference: 'workspace_id/object_id/version'
    object_reference = str(dfu_oi[6]) + '/' + str(dfu_oi[0]) + '/' + str(dfu_oi[4])

*Download an object from shock to a filepath*

.. code-block:: py

    self.dfu.shock_to_file({
        'shock_id': shock_id,
        'file_path': scratch_directory,
        'unpack': 'unpack'
    })

Examples
^^^^^^^^
- https://github.com/kbaseapps/ProkkaAnnotation/blob/master/lib/ProkkaAnnotation/Util/ProkkaUtils.py
- https://github.com/kbaseapps/kb_stringtie/blob/master/lib/kb_stringtie/Utils/StringTieUtil.py
- https://github.com/kbaseapps/kb_trimmomatic/blob/master/lib/kb_trimmomatic/kb_trimmomaticImpl.py
- https://github.com/kbaseapps/kb_MaSuRCA/blob/master/lib/MaSuRCA/core/masurca_assembler.py

GenomeFileUtil
--------------

*Download:*

.. code-block:: py

    file = gfu.genome_to_gff({'genome_ref': genome_ref})
    file['path']  # -> '/path/to/your/gff_file'

*Upload:*

.. code-block:: py

    gfu = GenomeFileUtil(os.environ['SDK_CALLBACK_URL'], token=self.getContext()['token'])
    gfu.genbank_to_genome({
        'file': {'path': scratch_path},
        'workspace_name': workspace_name,
        'genome_name': genome_obj
    })

Example
^^^^^^^^

- https://github.com/kbaseapps/kb_MaSuRCA/blob/master/lib/MaSuRCA/core/masurca_assembler.py
- https://github.com/kbaseapps/ProkkaAnnotation/blob/master/lib/ProkkaAnnotation/Util/ProkkaUtils.py

AssemblyUtil
------------

*Download:*

.. code-block:: py

    assembly_util = AssemblyUtil(self.callback_url)
    file = assembly_util.get_assembly_as_fasta({
        'ref': assembly_workspace_reference
    })
    file['path']  # -> 'path/to/your/fasta/file.fna'

*Upload:*

.. code-block:: py

    assembly_util = AssemblyUtil(self.callback_url)
    return assembly_util.save_assembly_from_fasta({
        'file': {'path': scratch_file_path},
        'workspace_name': workspace_name,
        'assembly_name': 'my_uploaded_assembly'
    }

Example
^^^^^^^^

- https://github.com/kbaseapps/kb_SPAdes/blob/master/lib/kb_SPAdes/kb_SPAdesImpl.py
- https://github.com/kbaseapps/ARAST_SDK/blob/master/lib/AssemblyRAST/AssemblyRASTImpl.py
- https://github.com/kbaseapps/kb_Velvet/blob/master/lib/Velvet/VelvetImpl.py

ReadsUtils
----------

Example
^^^^^^^

- https://github.com/kbaseapps/kb_trimmomatic/blob/master/lib/kb_trimmomatic/kb_trimmomaticImpl.py
- https://github.com/kbaseapps/kb_SPAdes/blob/master/lib/kb_SPAdes/kb_SPAdesImpl.py  
- https://github.com/kbaseapps/kb_Velvet/blob/master/lib/Velvet/VelvetImpl.py
- https://github.com/kbaseapps/kb_MaSuRCA/blob/master/lib/MaSuRCA/core/masurca_assembler.py

GenomeSearchUtil
-----------------

Example
^^^^^^^

- https://github.com/kbaseapps/DifferentialExpressionUtils/blob/master/lib/DifferentialExpressionUtils/core/diffExprMatrix_utils.py




.. External links
.. _the catalog: https://ci.kbase.us/#catalog/modules
