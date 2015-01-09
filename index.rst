.. ACCEPT documentation master file, created by
   sphinx-quickstart on Fri Jul 19 10:10:39 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. Unfortunately toctree does not seem to play nice with only: http://stackoverflow.com/questions/15001888/conditional-toctree-in-sphinx


ACCEPT Portal Documentation
===========================

.. warning:: This reference documentation set was created during the ACCEPT project with a view to guide users of the ACCEPT Portal technology (both end-users, administrative users and power users). Depending on how you plan to use some of the ACCEPT Portal technology, you may decide to only show specific parts of this documentation set.

What is the ACCEPT Portal?
--------------------------

The ACCEPT Portal is a Web application that was developed within the ACCEPT Project: |project-url| The default application is both an experimentation and demonstration platform that can be used to perform multiple data management tasks, including (but not limited to):

  * Setting up data collection containers or projects
  * Collecting usage information associated with text checks (e.g. spelling, grammar and style)
  * Collecting edits of (machine-)translated content as well as metadata about the editing process
  * Collecting user feedback (e.g. translation ratings)

The core functionality of the Portal can be accessed via its Web interface or its API. Additional functionality is provided by dedicated components, known as the *plug-ins*. These plug-ins, or client applications, allow for the actual interactive checking or editing of textual content (text) thanks to their interaction with the ACCEPT Portal API.


.. note:: During the ACCEPT project, an online, public instance of the Portal was available at: |project-portal-url|. This instance was managed by members of the ACCEPT Consortium. Once the ACCEPT project is over, new public or private instances may be deployed since the Portal has been made available as a standalone system.

.. warning:: While the overall vision of the ACCEPT project is to enable textual content owners (especially community content owners) to make their content available in a number of languages using a number of language technologies, the Portal does not (currently) support:

  * setting up Machine Translation engine provider connections to automate the translation process of source content
  * complex end-to-end content authoring, translation and publishing workflows
  * publishing pre- or post-edited content

Documentation Structure
-----------------------

This documentation set serves as reference documentation so if this is your first time interacting with the Portal or one of the ACCEPT plug-ins, you may want to start with our :ref:`quickStart`.

The Portal comprises three modules (|pre|, |post| and Evaluation), each of which being made of multiple components (e.g. API, plug-in, portal section).

.. toctree::
   :maxdepth: 3

   quickstart
   portal
   pre-edit/index
   post-edit/index
   evaluation/index



