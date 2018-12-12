Dynamic Services
================

Dynamic services vs. SDK Methods (e.g. apps)
--------------------------------------------

There are two flavors of SDK Module: dynamic services and apps. Apps run as one-off asynchronous jobs. In contrast to apps, Dynamic Services run as always-on services. Dynamic services cannot call app methods but can call other dynamic services.

Dynamic services are typically used as the backend for UI elements (such as Narrative widgets) so those elements can be displayed and updated quickly. The function of a dynamic service is often, for a specific workspace type (such as a KBaseGenome.Genome), to pull the data for an object and process the data into a form that the UI element can understand. The dynamic service will often cache the processed form of the data and return parts of the processed form as the UI needs those parts. This way, the UI doesn’t need to keep the entire dataset in memory.

.. important::

    Remember, dynamic services cannot import other utility modules like DataFileUtil. If your
    service needs these libraries, it can't be dynamic.

Creating a dynamic service
--------------------------

By default, all SDK modules are SDK Methods that run asynchronously. To mark a module as a
dynamic service, add the following to the kbase.yml file:

.. code-block:: yaml

    service-config:
        dynamic-service: true

You can then register your module as usual and start and stop it in a production environment using the |catalog_link| as described below in the :ref:`Manage dynamic services in a KBase environment section <in-kbase>`.
Other services should be installed with a dynamic service flag like this ``kb-sdk install -d <service>``.
Calls to other services should be initialized with the Service Wizard URL rather than the SDK Callback URL. The Service Wizard tracks the services that are deployed in the catalog and routes the request to the most up-to-date released service by default. Additionally, authentication token will need to be passed to the called service. This also means the called services should be initialized within each function that uses then rather than once as a class property. With  SDK apps where all of the processing takes place in a common location authentication is handled automatically. With a dynamic service however, the code is not initialized for a particular user and permissions need to be managed explicitly

.. code-block:: python

    # In the class init
    self.serviceWizardURL = config['srv-wiz-url']

    # *snip*

    # In each method's implementation
    gaa = GenomeAnnotationAPI(self.serviceWizardURL, token=ctx['token'])


Start and stop a dynamic service locally
----------------------------------------
1. Run kb-sdk test. This will set up a workdir in test_local like this:

.. code-block:: bash

    $ tree test_local/workdir/
    test_local/workdir/
    ├── config.properties
    ├── tmp
    └── token

2. Create a run_container.sh file in test_local by copying run_tests.sh to run_container.sh and
making the following edits to the last line:

* Delete `test` at the end of the file
* Delete ``-e SDK_CALLBACK_URL=$1``
* Add ``-d -p <external_port>:5000 --dns 8.8.8.8``

When you’re done it should look similar to this (obviously the module name will be different):

.. code-block:: bash

    #!/bin/bash
    script_dir="$(cd "$(dirname "$(readlink -f "$0")")" && pwd)"
    cd $script_dir/..
    $script_dir/run_docker.sh run --user $(id -u) -v
    $script_dir/workdir:/kb/module/work -d -p 10001:5000 --dns 8.8.8.8
    test/htmlfilesetserv:latest

3. Run the script:

.. code-block:: bash

    $ ./run_container.sh
    c8ea1197f9251323746d9ae42363387381ee79f6c06cd826e6dbfba0a7fd703b

You can now interact with the service at the port you specified (in the example above, 10001).

To view logs, get the container ID with docker ps and run docker logs:

.. code-block:: bash

    $ docker ps
    CONTAINER ID
    CREATED
    NAMES
    c8ea1197f925
    "./scripts/entrypoint" 2 minutes ago Up 2 minutes 0.0.0.0:10001->5000/tcp gigantic_swirles
    $ docker logs c8ea1197f925
    2016-10-14 22:55:27.835:INFO::Logging to StdErrLog::DEBUG=false via
    org.eclipse.jetty.util.log.StdErrLog
    2016-10-14 22:55:27.892:INFO::jetty-7.0.0.v20091005
    *snip*

When you’re done, shut down the docker container:

.. code-block:: bash

    $ docker stop c8ea1197f925
    c8ea1197f925

.. _in-kbase:

Manage dynamic services in a KBase environment
-------------------------------------------------------
The |catalog_link| provides tools to launch, inspect and stop dynamic services in each environment. The top of this page is a list of currently running services. There may be multiple instances of a service running that are based on different git hashes. As described above, the Service Wizard will route requests to the most current version of a released service (falling back to Beta or Dev if the service is not yet released). If this latest version is not running when a request is received, the service wizard will launch a new instance. This behavior improves resilience because if a container crashes, it will be restarted by the next service request. However, it also means that there is no rapid way to revert to an earlier version of the service if a problem is discovered with the service.

.. image:: /images/service-catalog.png
    :alt: Service Catalog

Logs of STDOUT and STDERR for services are also available to catalog administrators and may prove useful for debugging. 
Finally, catalog admins may also stop running services. There is no default timeout for dynamic services, 
so developers should periodically cull old versions of their services that are still running as they release new versions. 
Below the active services are lists of Released, Beta and Dev services that developers may launch but the Service Wizard generally renders this unnecessary.


.. External links
.. |catalog_link| raw:: html

   <a href="https://appdev.kbase.us/#catalog/services" target="_blank">catalog interface</a>


