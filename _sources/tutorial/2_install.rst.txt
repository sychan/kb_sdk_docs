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


.. note::

    While the preceding steps are the recommended approach, it's also possible to  |manual_link|

.. Internal links

.. |manual_link| raw:: html

   <a href="../howtos/manual_build.html">build the SDK from source code. </a>

