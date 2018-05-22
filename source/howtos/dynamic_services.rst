Dynamic Services
================

Dynamic services vs. SDK Methods (e.g. apps)
--------------------------------------------
There are two flavors of SDK Module: dynamic services and SDK Methods, known to users of the Narrative interface as Applications or Apps.
SDK Methods are run asynchronously in a queue and are expected to have longer running times.
SDK Methods can also call other SDK Methods. SDK Methods are typically used for wrapping
functionality from 3rd party code, uploaders and downloaders, and other long-running tasks. Almost
all narrative applications are implemented as SDK Methods. Dynamic services (DS) are SDK Modules
designed to respond quickly to requests. As such, the module runs as an always-on service. DSs
cannot call SDK Methods, but can call other services, dynamic as normal.

DSs are typically used as the backend for UI elements (such as Narrative widgets) so those elements
can be displayed and updated quickly. The function of a DS is often, for a specific workspace type
(such as a KBaseGenome.Genome), to pull the data for an object and process the data into a form
that the UI element can understand. The DS will often cache the processed form of the data and
return parts of the processed form as the UI needs those parts. Thus the UI doesn’t need to keep
the entire data, processed or otherwise, in memory.

Creating a dynamic service
--------------------------

.. important::

    Remember, dynamic services cannot import other utility modules like DataFileUtil. If your
    service needs these libraries, it can't be dynamic.

By default, all SDK modules are SDK Methods that run asynchronously. To mark a module as a
DS, add the following to the kbase.yml file (​example​):

.. code-block:: yaml

    service-config:
        dynamic-service: true

You can then register your module as usual and start and stop it using the catalog interface
(see Resources​ above). Otherwise development of a dynamic service module is identical to a Method module.

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