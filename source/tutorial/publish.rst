Publish and Update
====================

|appdev_link| is a separate KBase narrative server that is useful for app developers like yourself to publish in-progress versions of their apps and test and share them using real data.

Publish all git commits
-------------------------

Make sure you have run ``git add`` and ``git commit`` on all the changes in your app's codebase, then ``git push`` to your public github URL

Register with KBase
-----------------------

Go to |Register_link|.  Enter your public git repo URL:

.. figure:: ../images/register.png
    :align: center
    :figclass: align-center

    Register your module.


    
and click on ``Register``. Wait for the registration to complete (it may take a while on the first registration as it has to build the Docker image that is specific to your module from scratch).

Note that you must be an approved developer to register a new module. You can apply for a 
KBase developer account by going to |accounts_link|. If you are a US citizen, your account 
can be created within a few days. For foreign nationals, it may take several weeks (and, 
in a few cases, you may not be able to get a developer account). Non-US citizens will be 
asked for additional information in order to process their application. Once your account 
is approved, contact us with your username and ask to be added to the developer list.

Once your app has been registered, it will be available in the AppDev environment in KBase. 
Go |appdev_link| and start a new Narrative. Click on the 'R' in the Apps Panel  until it 
switches to 'D' ("develop") to show apps that are still in development.  Find your new app 
by searching for your module and run it!

Your app will now also be visible in the App Catalog when displaying Apps in development:

* Appdev: |appdevBrowse_link| 
* Production: |appcatBrowse_link| 

Updating
-----------
    
From your module page ( |MODULE_link| ) you'll be able to register any update and manage release of your module to the production KBase environment for anyone to use.

To update an app, open `Module Admin Tools` and then click the **REGISTER** button. You can specify a particular commit hash your repository (useful if you want to test a feature branch prior to merging into master) or leave this field blank to build from the latest commit on the master branch.

As you make changes to your Module, **you will need to re-commit those changes to the git repo, and then re-register**. The KBase SDK Catalog service will automatically pull the most recent version. If for some reason you wish to revert to an older version, you can add the hash key of that older commit, which you can find by viewing the ``git log``.

The ver (version) in the ``spec.json`` file becomes important once an app has been released to prod. The version needs to be updated for each subsequent release. The list of authors may need to be updated as well if additional people contribute to revisions.  


.. important::

    Please bear in mind that for public release, your module must meet all the requirements laid out in the  |Policies_link|. We reserve the right to delay public release of SDK Modules until all requirements are met. Please take the time to familiarize yourself with these policies to avoid delay in releasing your Module.

.. External links

.. |appdev_link| raw:: html

   <a href="https://appdev.kbase.us" target="_blank">Appdev (https://appdev.kbase.us)</a>

.. |accounts_link| raw:: html

   <a href=" https://accounts.kbase.us/index.php?tpl=request_identity.tpl" target="_blank">https://accounts.kbase.us/index.php?tpl=request_identity.tpl</a>

.. |Register_link| raw:: html

   <a href="https://appdev.kbase.us/#appcatalog/register" target="_blank">https://appdev.kbase.us/#appcatalog/register</a>

.. |appdevBrowse_link| raw:: html

   <a href="https://appdev.kbase.us/#appcatalog/browse/dev" target="_blank">https://appdev.kbase.us/#appcatalog/browse/dev</a>

.. |appcatBrowse_link| raw:: html

   <a href="https://narrative.kbase.us/#appcatalog/browse/dev" target="_blank">https://narrative.kbase.us/#appcatalog/browse/dev</a>


.. |MODULE_link| raw:: html

   <a href="https://narrative.kbase.us/#appcatalog/module/MODULE_NAME" target="_blank">https://narrative.kbase.us/#appcatalog/module/MODULE_NAME</a>

.. Internal links

.. |Policies_link| raw:: html

   <a href="../references/dev_guidelines.html">KBase SDK Policies</a>



