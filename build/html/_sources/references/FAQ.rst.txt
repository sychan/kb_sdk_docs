Frequently Asked Questions
==========================

Q: How can I tell if someone's already wrapped a tool I'm interested in?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Go to the development `App
Catalog <https://narrative.kbase.us/#appcatalog>`__ and start by using
*Search* to see if there's a released method. If you don't find a method
that way, it may also help to by selecting "Organize by"... "Category"
and selecting what seems to be a likely category for the tool. There are
also methods that have not yet been officially released, so you should
also check the "Beta" and "Dev" categories to see if there's something
in the pipeline by selecting them from the "Version" dropdown.

It may be the case that someone is wrapping a tool, but is doing so in a
way that doesn't serve your needs exactly. Feel free to rewrap the tool
using your approach, or ask the previous tool wrapper to tweak their
implementation to expose the parameters or other functionality you are
looking for.

Q: What's already installed in the base KBase Docker image and how do I call it?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** The base KBase Docker image contains an Ubuntu Linux image with
kbase tools for connecting to KBase services and data installed. Please
see the SDK Module implementation
`examples <https://github.com/kbase/kb_sdk#examples>`__ for
how to access these services and data.

Q: How do I find and access data?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** There are both public reference data sets and individual user
data sets (both public and private). Public reference data can be found
with `KBase Data
Search <https://narrative.kbase.us/#search>`__, and should
be copied into your Narrative to be used. Accessing Data Objects from
your SDK Methods is explained in `Working with KBase Data
Types <https://narrative.kbase.us/#catalog/datatypes>`__ and illustrated
in the SDK Module implementation
`examples <https://github.com/kbase/kb_sdk#examples>`__.

Q: What are system requirements for development?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** You will need to be able to run Docker, which if you're on a Mac
means you must be running Mac OS X 10.8 or later. Other operating
systems, such as the various flavors of Linux, are fine too. Really
anywhere you can run Docker, Java, and your preferred development
language (among Python, Perl, or Java). You will need about 1-2 GB free
to install the
`dependencies </tutorial/dependencies.html>`__
and the `KBase SDK </tutorial/install.html>`__

Q: What are system requirements for execution environment?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** 

- Runs completely on a standard KBase worker node (at least 2 cores and 22GB memory) 
- Operates only on supported KBase data types
- Requires either no or fairly limited amounts of reference data 
- Uses existing data visualization widgets 
- Does not require new uploaders/downloaders 
- Wrapper written in Python, Java, or Perl

Q: What size data will be too big for the system?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Currently we support up to about 10 GB of accessory data for a
tool (meaning reference DBs, etc). Please contact us at
https://kbase.us/contact-us if you need to use something larger.

As for processing, once it's uploaded to the system (which can take
awhile for larger data sets), it depends on how you are using it.
Currently SDK methods are limited in their memory footprint to the 22 GB
of the worker nodes, so your code plus any data you load into memory
must fit within that. As in any situation, we recommend the use of
graceful exception handling and efficient implementations in your coding
style.

Q: Where should my code be?
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Your SDK Module code should be in a single repo. That means you
can develop on your personal workstation as long as you check in all
changes to a public revision control system such as github.com. For the
time being, we ask that you put your code into your own directory rather
than in the kbase github account.

Q: Do you need to use GitHub? What about BitBucket? SourceForge?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** You can use any public open-source revision control system. We
use GitHub. The path to your repo is what you provide to the SDK
Registration method to register your SDK Module.

Q: Do I need to copy the tool I'm using into my github repo?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** You do not if there is a public way to retrieve the code such as
by using a *git clone*, *curl*, or other way of pulling the data down
into the Docker image. This is accomplished by `modifying the
Dockerfile </howtos/edit_your_dockerfile.html>`__
to configure the Docker image build.

Q: Can I develop on Windows?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Sort of. Your best option right now is to install
`VirtualBox <https://www.virtualbox.org>`__ with `Ubuntu
Linux <https://www.ubuntu.com/desktop>`__ and work in the Linux VM. Many
developers use this approach in KBase, and we know it works well.

Q: Can I do all the development on a remote Linux box?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Yes. All graphical-user-interface requiring
steps in the process are accomplished by using a web browser.

Q: How do I add custom data and/or configuration data to the system?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** If it's less than 10 GB, you can add it to the Docker image by
`editing the Dockerfile </howtos/edit_your_dockerfile.html>`__.
Please contact us at https://kbase.us/contact-us if you need to use
something larger.

Q: I need high-performance computing for my application. Is that available?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Not yet. We're working on it!

Q: How do I set my favorite Apps?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** After logging into `KBase <https://kbase.us>`__, go to the `App
Catalog <https://narrative.kbase.us/#appcatalog>`__, and then click on
the stars for your favorite Apps. You must be logged in for it to
associate it with your account.

Q: Is there a cheatsheet?
^^^^^^^^^^^^^^^^^^^^^^^^^

**A:** Yes, there is a document that provides a lot of hints about the
SDK `SDK Cheatsheet <https://github.com/kbase/kb_sdk/blob/master/doc/SDK_AdvancedFeaturesCheatSheet.pdf>`__.

.. |alt text| image:: https://avatars2.githubusercontent.com/u/1263946?v=3&s=84

