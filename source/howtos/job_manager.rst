Listing and viewing jobs
-----------------------------

.. code-block:: python

    # View the latest jobs

    from biokbase.narrative.jobs.jobmanager import JobManager
    jm = JobManager()
    jm.list_jobs()


If you do ``run_app()`` without a ``cell_id`` field, it’ll return the job object. So, you could do something like:
    
.. code:: python

    my_job = AppManager().run_app( ... app info ...)
    my_job.job_id


.. todo::
    
    Expand with an explanation of what the jobs are, the purpose of viewing jobs, and in what context you would need this.
