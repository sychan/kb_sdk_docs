Using workspace metadata
==================================

.. important::

    While you can get objects using the workspace client, it is not the recommended way to do it. We suggest using the `file utilities <../howtos/file_utils.html>`_ instead.

Retrieving workspace object metadata
---------------------------------------

If you don't want to retrieve the entire object, and just want information about it, the most efficient way is to use the ``ws.get_object_info`` function. To read more about the available functions, see the `workspace spec file <https://kbase.us/services/ws/docs/Workspace.html>`_.

*Initialize the workspace client and get an object's information by reference*

.. code:: python

    # The config object is passed into the __init__ function for your *Impl.py module
    from Workspace.WorkspaceClient import Workspace as Workspace
    self.ws_url = config["workspace-url"]
    self.token = config["KB_AUTH_TOKEN"]
    self.ws = Workspace(self.ws_url, token=self.token)
    obj_ref = "your/object/reference"
    object_info = self.ws.get_object_info([{"ref": obj_ref}])[0]
    object_type = object_info[2] #this could be KBaseGenomes.Genome-8.3


How to use the Workspace Client in the narrative
-------------------------------------------------

You can access the workspace client from within narrative code cells. See this `example narrative <https://narrative.kbase.us/narrative/ws.30170.obj.1>`_.

.. code:: python

    ws = biokbase.narrative.clients.get('workspace')
    obj = ws.get_objects2({'objects' : [{'ref' : '30170/2/1'}]})



How to add types to the workspace
------------------------------------

* See the guide at https://ci.kbase.us/services/ws/docs/administrationinterface.html#managing-module-ownership-requests
* Use the administration interface in any of the clients above to register, approve registration of, and to upload typespecs

How to get a given typespec / .spec file?
------------------------------------------

If you can't find a typespec in a repo, you can find it doing the following:

.. code-block:: python
    :caption: Example spec file snippet

    from biokbase.workspace.client import Workspace
    ws = Workspace('https://ci.kbase.us/services/ws')
    print ws.get_module_info({'mod': 'YourModuleName'})['spec']

