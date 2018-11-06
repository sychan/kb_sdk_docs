Installation
================

Now that Docker is installed, pull down the KBase SDK image.

.. code-block:: bash

   $ docker pull kbase/kb-sdk

.. tip::

    If it has been a while since you last developed a KBase module, it is a good idea to run the 
    command above before starting a new module. This will insure that you have the latest version.

Add the ``kb-sdk`` as a global command by linking it in your ``$PATH``. Place the script in a directory like `~/bin`:

.. code-block:: bash

   $ mkdir $HOME/bin/
   # Generate the kb-sdk script and put it in ~/bin/kb-sdk
   $ docker run kbase/kb-sdk genscript > $HOME/bin/kb-sdk
   $ chmod +x $HOME/bin/kb-sdk
   # Add ~/bin to your $PATH if it is not already there
   $ export PATH=$PATH:$HOME/bin/
   # You might want to put the above command in your ~/.bashrc or ~/.zshrc:
   $ echo "export PATH=\$PATH:$HOME/bin/" >> ~/.bashrc


Test the installation by running the kb-sdk help command.

.. code-block:: bash

   $ kb-sdk help


Download the Docker Image
-------------------------------------------

All KBase modules run in Docker containers.  Docker containers are built on top of existing base images.  KBase has 
a public base image that includes a number of installed runtimes, some basic bioinformatics tools, and other KBase-specific tools.
To run this locally, you will need to download the base image.

.. code-block:: bash

   $ kb-sdk sdkbase


The image is fairly large, so it will take some time to download.  This step is required for running tests locally and
should only be reqired during the initial installation.  However, KBsae staff may occasionally require the base image
to be updated in order to match any changes in the base image running in the production KBase platform.

.. note::

    While the preceding steps are the recommended approach, it's also possible to  |manual_link| 

.. External links

.. |manual_link| raw:: html

   <a href="../howtos/manual_build.html" target="_blank">build the SDK from source. </a>

