Troubleshooting Guide
=====================

Trying to run ``make sdkbase`` and seeing errors that include ``TLS-enabled daemon`` and/or ``docker daemon``

When you try to run ``make sdkbase`` if you see a message like:

.. code:: bash

    docker build -t kbase/kbase:sdkbase2.latest sdkbase
    Post http:///var/run/docker.sock/v1.20/build?cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&memory=0&memswap=0&rm=1&t=kbase%2Fkbase%3Asdkbase.latest&ulimits=null: dial unix /var/run/docker.sock: no such file or directory.
    * Are you trying to connect to a TLS-enabled daemon without TLS?
    * Is your docker daemon up and running?
    make: *** [sdkbase] Error 1


You likely have not started your Docker daemon. On a Mac, that means running in the Docker CLI shell after starting Docker Kitematic and clicking on "Docker CLI" in the lower left corner (See `Install SDK Dependencies - Docker </tutorial/install.html>`__ for guidance).

Trying to run *kb-sdk test* and seeing errors that include *TLS-enabled daemon* and/or *docker daemon*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you try to run ``kb-sdk test``, if you see a message like:

::

    Build Docker image
    Post http:///var/run/docker.sock/v1.20/build?cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&memory=0&memswap=0&rm=1&t=test%2Fkb_vsearch%3Alatest&ulimits=null: dial unix /var/run/docker.sock: no such file or directory.
    * Are you trying to connect to a TLS-enabled daemon without TLS?
    * Is your docker daemon up and running?

You likely have not started your Docker daemon. On a Mac, that means
running in the Docker CLI shell after starting Docker Kitematic and
clicking on "Docker CLI" in the lower left corner (See `Install SDK
Dependencies - Docker </tutorial/install.html>`__ for guidance).

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

