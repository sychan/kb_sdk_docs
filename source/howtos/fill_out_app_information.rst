Fully documenting your app
================================

Complete Module Info
~~~~~~~~~~~~~~~~~~~~

Icons, Publications, Original tool authors, Institutional Affiliations,
Contact Information, and most importantly, Method Documentation must be
added to your module before it can be deployed. This information will
show up in the |appcatalog_link| 

Please be aware that your module implementation and information must
conform to our
|Policies_link| 
before it will be accepted for public deployment. Fortunately, most of
these are common sense (for example, sufficient content on the App Info
page for a user to run your app and understand what it's doing, proper
arguments and explanation of their use on the input widget, etc.), but
please take the time to familiarize yourself with these requirements
before requesting public deployment.

Adding an Icon
^^^^^^^^^^^^^^

You can make a custom icon for each app in your module, or use an
existing one that corresponds to the tool that you have wrapped. Feel
free to repurpose the icons from existing KBase apps, or make your own.
Your icon can be PNG, GIF, or JPEG (the KBase ones are PNG) and should
fit in a square 200x200 pixels. To match our existing icons, use these guidelines:

* 200x200px image with 40px rounded edges
* Font: Futura Condensed Medium, 72pt, white
* 72dpi PNG

PDF vector and PNG bitmap versions that we used for our icons are available at
|appicons_link| in case you would like to use them
as a starting point.

Your icons should be added to your KBase SDK module GitHub repo in the
**img** image folder for each app at:

::

    https://github.com/<MyUserName>/<MyModule>/ui/narrative/methods/<MyMethod>/img/


Then edit the **display.yaml** file found at:

::

    https://github.com/<MyUserName>/<MyModule>/ui/narrative/methods/<MyMethod>/display.yaml


and add an **icon:** configuration (NOTE: just add the name of the image
file and do not include the path, so *not* "img/foo.png"). For example,
in the file:
|trimmomatic_link| 

the **icon:** is configured by the line: icon: trimmomatic-orange.png

Naming and Categorizing
^^^^^^^^^^^^^^^^^^^^^^^

Each app should have a unique display name. The display name is
configured in the **display.yaml** file at:

::

    https://github.com/<MyUserName>/<MyModule>/ui/narrative/methods/<MyMethod>/display.yaml

and add a **name:** configuration. If you are wrapping an existing tool
with a known name, please include that name to make it easier for people
to find it, such as

.. code:: yaml

    name: Trimmomatic - Read Trimming

You should tag your app with one (or multiple) "categories". (If you
don't, it will appear in the "Uncategorized" section and users will be
less likely to find it.)

The categories are set in the **spec.json** file at:

::

    https://github.com/<MyUserName>/<MyModule>/ui/narrative/methods/<MyMethod>/spec.json

Currently, the following categories are recognized. The tag before the :
on each line is the internal label for the category, used in the
spec.json. The phrase after the : is the display name for the category
that is used in the App Catalog.

::

    annotation: Genome Annotation
    assembly: Genome Assembly
    communities: Microbial Communities
    comparative_genomics: Comparative Genomics
    expression: Expression
    metabolic_modeling: Metabolic Modeling
    reads: Read Processing
    sequence: Sequence Analysis
    util: Utilities


(Please contact us via |contactUS_link| if you have
suggestions for more... we expect to add more categories and possibly
subcategories in the near future.)

An example of a category configuration line is:

::

    "categories": ["active","assembly","util"],

Please leave the category "active" at the beginning of the list of
categories, as this is a special category that indicates whether your
app should be shown at all in the App Catalog. The rest of the
categories are treated as normal tags.

Writing your App Info page
^^^^^^^^^^^^^^^^^^^^^^^^^^

Information for the App Info page is configured in the **display.yaml**
file at:

::

    https://github.com/<MyUserName>/<MyModule>/ui/narrative/methods/<MyMethod>/display.yaml

The fields that should be configured are

-  name
-  icon
-  tooltip
-  description
-  publications
-  screenshots

Name, Icon, Tooltip, Description
''''''''''''''''''''''''''''''''

**name:** and **icon:** are explained above. You must also add a
**tooltip:** as a secondary short description (usually one sentence
summarizing the purpose of the app).

The **description:** field is for a more detailed description, which can
include several recommended pieces of information. For example, the URL
of an exemplar Narrative that demonstrates how to use the app should be
included in the description. If you are wrapping an existing tool,
please add links to the open-source repo for that tool in both the
**description:** field and the **publications:** field. If there is a
home page for the tool, please also add a link to that.

Publications and Links
''''''''''''''''''''''

It is important to accurately cite all associated publications and software when preparing an app for KBase deployment. This is especially true when you wrap an existing tool.

* All publication listings belong in the ‘publications’ field at the bottom of the **display.yaml** file.

* Tool home pages and open source repos should also be included as publications.

* Publications and software information should not be duplicated elsewhere in the file. 

* Links to source codes or websites should be prefaced with some information. Do not list the hyperlink by itself (see example below).

* The minimum fields to provide are the **display-text:** and a **link:**. Optionally, you can provide a **pmid:** with a PubMed ID.

* All publication information should be in the PLOS style format (see example below). 

.. tip::

   PLOS Format:

   Author Surname Author Initial(s), Author Surname Author Initial(s), et al. Title of Article. Title of Journal. Publication Year;Volume: Page Range. doi:  

The format for the minimal publication is:

.. code::

    publications : 
        -
            display-text: |
                Citation in PLOS style format
            link: Link associated with publication 

An example:

::

    publications :
        -
            pmid: 27071849
            display-text : |
                Menzel P, Ng KL, Krogh A. Fast and sensitive taxonomic classification for metagenomics with Kaiju. Nat Commun. 2016;7: 11257. doi:10.1038/ncomms11257
            link: http://www.ncbi.nlm.nih.gov/pubmed/27071849

        -
            pmid: 21961884
            display-text : |
                Ondov BD, Bergman NH, Phillippy AM. Interactive metagenomic visualization in a Web browser. BMC Bioinformatics. 2011;12: 385. doi:10.1186/1471-2105-12-385
            link: http://www.ncbi.nlm.nih.gov/pubmed/21961884

        -
            display-text: |
                Kaiju Homepage:
            link: http://kaiju.binf.ku.dk/

        -
            display-text: |
                Kaiju DBs from:
            link: http://kaiju.binf.ku.dk/server

        -
            display-text: |
                Github for Kaiju:
            link: https://github.com/bioinformatics-centre/kaiju

        -
            display-text: |
                Krona homepage:
            link: https://github.com/marbl/Krona/wiki

        -
            display-text: |
                Github for Krona:
            link: https://github.com/marbl/Krona


Screenshots
'''''''''''

You can add screenshots (or other relevant images) to the "img/" folder
in the same fashion as the icon image. These screenshots should be
configured in the **display.yaml** file as a list with one filename on
each line, preceded by a hyphen, e.g.,

::

    screenshots:
        - screenshot_1.png
        - screenshot_2.png

If you do not want to have any screenshots, leave the **screenshots:**
list blank.

::

    screenshots: []

Example
'''''''

For an example of a complete App Info page that would be acceptable for
public deployment, please see examples in the Trimmomatic app:

-  |appdevTrim_link| 
-  |psdehalTrim_link| 

.. important:: 

    Please bear in mind that for public release, your module **MUST** meet
    all the requirements laid out in the `KBase SDK
    Policies <../references/dev_guidelines.html>`__.
    We reserve the right to delay public release of SDK modules until all
    requirements are met. Please take the time to familiarize yourself with
    these policies to avoid delay in releasing your module.

.. External links

.. |appcatalog_link| raw:: html

   <a href="https://appdev.kbase.us/#appcatalog/" target="_blank">App catalog (https://appdev.kbase.us/#appcatalog/)</a>

.. |Policies_link| raw:: html

   <a href="../references/dev_guidelines.html" target="_blank">Policies</a>

.. |appicons_link| raw:: html

   <a href="https://github.com/kbase/kb_sdk_docs/tree/master/source/images/app-icons" target="_blank">https://github.com/kbase/kb_sdk_docs/tree/master/source/images/app-icons</a>

.. |trimmomatic_link| raw:: html

   <a href="https://github.com/psdehal/kb_trimmomatic/blob/master/ui/narrative/methods/run_trimmomatic/display.yaml" target="_blank">https://github.com/psdehal/kb_trimmomatic/blob/master/ui/narrative/methods/run_trimmomatic/display.yaml</a>

.. |contactUS_link| raw:: html

   <a href="http://kbase.us/contact-us" target="_blank">ContactUs (http://kbase.us/contact-us)</a>

.. |appdevTrim_link| raw:: html

   <a href="https://appdev.kbase.us/#appcatalog/app/kb_trimmomatic/run_trimmomatic/dev" target="_blank">https://appdev.kbase.us/#appcatalog/app/kb_trimmomatic/run_trimmomatic/dev</a>

.. |psdehalTrim_link| raw:: html

   <a href="https://github.com/psdehal/kb_trimmomatic/blob/master/ui/narrative/methods/run_trimmomatic/display.yaml" target="_blank">https://github.com/psdehal/kb_trimmomatic/blob/master/ui/narrative/methods/run_trimmomatic/display.yaml</a>

