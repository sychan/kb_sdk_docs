Troubleshooting Guide
=====================

.. contents::

Trying to run ``make sdkbase`` and seeing errors that include ``TLS-enabled daemon`` and/or ``docker daemon``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you try to run ``make sdkbase`` if you see a message like:

.. code-block:: bash

    docker build -t kbase/kbase:sdkbase2.latest sdkbase
    Post http:///var/run/docker.sock/v1.20/build?cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&memory=0&memswap=0&rm=1&t=kbase%2Fkbase%3Asdkbase.latest&ulimits=null: dial unix /var/run/docker.sock: no such file or directory.
    * Are you trying to connect to a TLS-enabled daemon without TLS?
    * Is your docker daemon up and running?
    make: *** [sdkbase] Error 1


You likely have not started your Docker daemon. On a Mac, that means running in the Docker CLI shell after starting Docker Kitematic and clicking on "Docker CLI" in the lower left corner (See `Install SDK Dependencies - Docker <../tutorial/install.html>`__ for guidance).

Trying to run ``kb-sdk test`` and seeing errors that include "TLS-enabled daemon" and/or "docker daemon"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you try to run ``kb-sdk test``, if you see a message like:

::

    Build Docker image
    Post http:///var/run/docker.sock/v1.20/build?cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&memory=0&memswap=0&rm=1&t=test%2Fkb_vsearch%3Alatest&ulimits=null: dial unix /var/run/docker.sock: no such file or directory.
    * Are you trying to connect to a TLS-enabled daemon without TLS?
    * Is your docker daemon up and running?

You likely have not started your Docker daemon. On a Mac, that means
running in the Docker CLI shell after starting Docker Kitematic and
clicking on "Docker CLI" in the lower left corner (See `Install SDK
Dependencies - Docker <../tutorial/install.html>`__ for guidance).

My code keeps disappearing. What happened to it?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Magic comments are comments that are used internally by the kbase_sdk in order to generate the implementation file when you make changes to the spec file.

Examples of magic comments include:

.. code:: python

    #BEGIN_HEADER
    (This is where your import statements go)
    #END_HEADER

    #BEGIN_CLASS_HEADER
    (This is where your class variables and functions go that you want imported)
    #END_CLASS_HEADER

    #BEGIN_CONSTRUCTOR
    (This is in your init statement for your class goes)
    #END_CONSTRUCTOR

    #BEGIN YourFunctionName1
    (This is were the implementation details of your functions go)
    #END YourFunctionName1


Any code created outside of the Magic Comments will not be included inside the final .impl implementation file.

Having trouble getting Docker working on Mac
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It may be that your Docker installation may be incorrect, out of date,
or the daemon may not have been started. Please see https://docs.docker.com/mac/


Having trouble getting Docker working on Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It may be that your Docker installation may be incorrect, out of date,
or the daemon may not have been started. Please see https://docs.docker.com/linux/


Getting Java-related errors trying to run kb-sdk
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Java may not be installed or the path may not be set properly. Please follow the directions for installation of Java at https://github.com/kbase/kb\_sdk/blob/master/doc/kb\_sdk\_dependencies.md and then set your *JAVA\_HOME* with

::

    # for bash
    export JAVA_HOME=`/usr/libexec/java_home`
    # for tcsh/csh
    setenv JAVA_HOME `/usr/libexec/java_home`


.. |alt text| image:: https://avatars2.githubusercontent.com/u/1263946?v=3&s=84


My report isn't generating output correctly
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See the guide on `Creating a report <../howtos/create_a_report.html>`_.


Getting "ServerError: JSONRPCError"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*Full error*: ``ServerError: JSONRPCError: -32601. Unknown server error (output data wasn't produced)``

This case happens because the python process exits without writing an output file, and then the callback server throws the above error. Make sure your process finishes and writes an output file to avoid this error.

Unable to find valid certification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you get an error on OSX as follows:

.. code-block:: bash

    $ kb-sdk test
    Validating module in (/Users/user/Module/ExpressionUtils)
    Congrats- this module is valid.
    Error while testing module: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
    For more help and usage information, run:
        kb-sdk help
        (ExpressionUtils)


Generate new security cerficates:


.. code-block:: bash

    $ openssl x509 -in <(openssl s_client -connect ci.kbase.us:443 -prexit 2>/dev/null) -out ~/example.crt
    $ sudo keytool -importcert -file ~/example.crt -alias example -keystore $(/usr/libexec/java_home)/jre/lib/security/cacerts -storepass changeit
