Terminology
===========

App
   A catalog of existing apps is found at https://narrative.kbase.us/#catalog/apps

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

   The pros for grouping multiple apps in module include:

   - Apps that are part of a logical grouping are all found together
   - Common functions that are shared by the apps are defined once
   - The specification file is only defined once 

   The pros for creating one app per module include:

   - The apps are revised independently and version numbers are specific to the app, not the group of apps
   - The implementation file for some apps can get really long. Combining them into one module can get difficult
     to manage.
