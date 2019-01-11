Design Checklist
====================
This checklist walks though design considerations for the Implementation, Input and Output of SDK apps.

- Does my App depend on any compiled or third-party packages?
    - If so, are they licenced with a KBase compliant licence?
        All dependencies of KBase Apps must be free for any use including commercial use.
    - If so, is there a version that is executable by one of the SDK languages or as a linux binary?
        See |wrap_app_link| for more detail.

- What are the CPU and RAM requirements for my app?
    For more information on compute hardware and resource specification see this |job_exec_link|

- Does my App require reference/precomputed data?
    - If so, is that data licenced with a KBase compliant licence?
        All dependencies of KBase Apps must be free for any use including commercial use.

    - If so, is that data small enough to store in a GitHub repository (<100 MB files)
        If not, you can still used versioned reference data. See the |RefData_link| for more details.

- Does my App operate on KBase datatypes?
    - If so, do I need a FileUtil to convert the Workspace object to a desired file format?
        See our |FileUtil_link| for more detail

- What parameters do I want users to be able to provide?
    See |UIspec_link| for more detail

- Does my app produce any KBase DataTypes?
    - If so, do I need a FileUtil to save the desired file format as a Workspace object?
        See our |FileUtil_link| for more detail

- How do I want to display results to users?
    See the |Reports_link| for options.

.. Internal links

.. |UIspec_link| raw:: html

   <a href="../references/UI_spec.html">Narrative App UI Specification</a>

.. |FileUtil_link| raw:: html

   <a href="../howtos/file_utils.html">FileUtil Guide</a>

.. |Reports_link| raw:: html

   <a href="../howtos/create_a_report.html">Report Guide</a>

.. |RefData_link| raw:: html

   <a href="../howtos/work_with_reference_data.html">Reference Data Guide</a>

.. |wrap_app_link| raw:: html

   <a href="../howtos/run_a_shell_command.html">Running Shell Commands Guide</a>

.. |job_exec_link| raw:: html

   <a href="../references/execution_engine.html">Job Execution Guide</a>


