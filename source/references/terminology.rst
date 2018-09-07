Terminology
===========

App 
   A tool in the KBase narrative interface. In a narrative, users select the app, it opens a new cell, users 
   specify inputs, outputs, and optional parameters, and it may or may not create a separate output cell.
   In SDK, apps are part of a module and have a defined function, user interface, and code for implementing the app.

Catalog
   For general users, the Catalog is a listing of available apps and modules along with the documentation
   that developers included. Users can select whether the want to see released, beta, or dev apps and 
   they can be sort the list. 

   .. figure:: ../images/app_catalog.png
       :align: center
       :figclass: align-center

       User App Catalog

   For registered KBase developers, there additional catalog options.

   .. figure:: ../images/app_catalog_dev.png
       :align: center
       :figclass: align-center

       Developer App Catalog

   NEW
 
   - A direct link for registering a module
   - An index link that has several useful resources
   - A catalog status

Data Type Catalog
   A `listing <https://narrative.kbase.us/#catalog/datatypes>`_ of the current data objects with links to 
   their specification documentation.

Function Catalog
   A `listing <https://narrative.kbase.us/#catalog/functions`_ of the current functions

Github repository
   Also called 'github repo' or 'git repo'. 
   A repository stored at https://github.com or in an organization's github. It can be in your personal 
   GitHub account or part of an organization's or project's account. For use in KBase, it must be public. 

Method
   When defining the user interface, a method is another name for an app. This is defined in a directory called
   ui/narrative/methods/<app name>. 

   Code written in python will have python methods. A subset of the python
   methods correspond to apps. Those that do are named for the apps.

Module
   A module includes all of the components for one or more apps. The apps are mananged as a group. All the 
   components are in the same directory on your local machine or in one github repository. The directory and
   the github repository are typically the same name as the module. A catalog of existing
   modules is found at https://narrative.kbase.us/#catalog/modules. 

   The pros for grouping multiple apps in one module include:

   - Apps that are part of a logical grouping are all found together
   - Common functions that are shared by the apps are defined once
   - The specification file is only defined once 

   The pros for creating one app per module include:

   - The apps are revised independently and version numbers are specific to the app, not the group of apps
   - The implementation file for some apps can get really long. Combining them into one module can get difficult
     to manage.

Workspace Service
    The Workspace Service (WSS) is primarily a language independent remote storage
    and retrieval system for KBase typed objects (TO) defined with the KBase
    Interface Description Language (KIDL). It has the following primary features:

    - Immutable storage of TOs with
    - user defined metadata
    - data provenance
    - Versioning of TOs
    - Referencing from TO to TO
    - Typechecking of all saved objects against a KIDL specification
    - Collecting typed objects into a workspace
    - Sharing workspaces with specific KBase users or the world
    - Freezing and publishing workspaces
   

Wrap an App
    The SDK can be used to create a wrapper around a 3rd-part software tool and make it accessible through 
    KBase. The guide for `Edititng your app's Dockerfile <../howtos/edit_your_dockerfile.html>`_ has useful
    information for wrapping a tool. Examples of wrapping a tool can be found at `kb_fasttree <https://github.com/kbaseapps/kb_fasttree>`_ and `kb_ballgown <https://github.com/kbaseapps/kb_ballgown>`_. The developer has the option of implementing some or all of the functionality of the original tool into a KBase app.


