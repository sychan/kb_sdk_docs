Running shell commands
==============================

Download and compile using your Dockerfile
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Many KBase apps wrap third-party tools with commandline interfaces. You
can compile these programs for your app by entering the build commands
in your Dockerfile. For example, |FastANI_link| compiles and installs
with this series of commands:

.. code-block:: bash

    # Fetch and compile FastANI
    RUN git clone https://github.com/ParBLiSS/FastANI.git /opt/FastANI \
        && cd /opt/FastANI \
        && git checkout tags/v1.0 -b v1.0 \
        && ./bootstrap.sh \
        && ./configure \
        && make \
        # Place fastANI in the PATH for this user
        && ln -s $(readlink -f ./fastANI) /usr/local/bin/fastANI

Some things to note:

-  Be sure to check out a specific version tag or commit so that your
   app doesn't receive unexpected, possibly broken updates
-  Put this code high up in your Dockerfile, above where your app gets
   initialized, so that it gets cached and doesn't have to re-run every
   time you change your app
-  You can place commands in the directory ``/kb/deployment/bin/``, and
   it will automatically get added to your container's ``$PATH``

.. note::
    Also see: |editApp_link| 

Invoking the command in Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can use the  |subprocess_link| package to invoke commands from within python.

The following example from the |kb_quast_link| repository
demonstrates how to do (and document) this well with Python.

.. code:: python

    def run_quast_exec(self, outdir, filepaths, labels, skip_glimmer=False):
        threads = psutil.cpu_count() * self.THREADS_PER_CORE
        # DO NOT use genemark instead of glimmer, not open source
        # DO NOT use metaQUAST, uses SILVA DB which is not open source
        cmd = ['quast.py', '--threads', str(threads), '-o', outdir, '--labels', ','.join(labels),
               '--glimmer', '--contig-thresholds', '0,1000,10000,100000,1000000'] + filepaths

        if skip_glimmer:
            self.log('skipping glimmer due to large input file(s)')
            cmd.remove('--glimmer')

        self.log('running QUAST with command line ' + str(cmd))
        retcode = _subprocess.call(cmd)
        self.log('QUAST return code: ' + str(retcode))
        if retcode:
            # can't actually figure out how to test this. Give quast garbage it skips the file.
            # Give quast a file with a missing sequence it acts completely normally.
            raise ValueError('QUAST reported an error, return code was ' + str(retcode))
        # quast will ignore bad files and keep going, which is a disaster for
        # reproducibility and accuracy if you're not watching the logs like a hawk.
        # for now use this hack to check that all files were processed. Maybe there's a better way.
        files_proc = len(open(_os.path.join(outdir, 'report.tsv'), 'r').readline().split('\t')) - 1
        files_exp = len(filepaths)
        if files_proc != files_exp:
            err = ('QUAST skipped some files - {} expected, {} processed.'
                   .format(files_exp, files_proc))
            self.log(err)

.. External links


.. |editApp_link| raw:: html

   <a href="../howtos/edit_your_dockerfile.html" target="_blank">How to edit your app’s Dockerfile  </a>

.. |FastANI_link| raw:: html

   <a href="https://github.com/kbaseapps/FastANI" target="_blank">FastANI  </a>

.. |subprocess_link| raw:: html

   <a href="https://docs.python.org/2/library/subprocess.html" target="_blank">subprocess  </a>

.. |kb_quast_link| raw:: html

   <a href="https://github.com/kbaseapps/kb_quast" target="_blank">kb_quast </a>

