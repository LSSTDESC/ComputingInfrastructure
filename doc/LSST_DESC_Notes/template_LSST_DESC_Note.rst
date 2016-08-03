..
  Template for LSST DESC Notes

  See also:
  * https://github.com/lsst-sqre/sqr-000/blob/master/index.rst for an LSST technote deescribing LSST technotes, on which DESC notes are styled.
  * https://github.com/lsst-dm/dmtn-008/blob/master/index.rst for a nice example LSST technote by Michael Wood-Vasey, which is rendered by the LSST technotes system at http://dmtn-008.lsst.io/en/latest/
  * http://docs.lsst.codes/en/latest/development/docs/rst_styleguide.html for a guide to reStructuredText writing.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  directory containging this note. The reST syntax for adding the image is

  .. figure:: /filename.ext
     :name: fig-label
     :target: http://target.link/url

     Caption text.

  
  Feel free to delete these instructions!



=========================
LSST DESC Note Title 
=========================

*Author A. Name, Author B. Name*

.. |date| date::
This document was generated on: |date|


Introduction
============
This is a template LSST DESC Note, for you to adapt for your own work. The original can be found `on GitHub <https://github.com/DarkEnergyScienceCollaboration/ComputingInfrastructure/blob/master/doc/LSST_DESC_Notes/template_LSST_DESC_Note.rst>`_. 

Sectioning 
==========
This can be divided into subsections, as follows.

A Subsection
------------
You can also have subsubsections, like this:

A Subsubsection
^^^^^^^^^^^^^^^
See?

Another Subsubsection
^^^^^^^^^^^^^^^^^^^^^
Easy.

Another Subsection
------------------
And so on.

Code
====
You can show code in blocks like this:

.. code-block:: python

  print "Hello World"

or this:

.. code-block:: bash

  echo "Hello World"


Figures
=======

To add images, add the image file (png, svg or jpeg preferred) to the ``_static`` subdirectory in this note's folder. Here's an example:

.. figure:: /_static/desc-logo.png
  :name: fig-label
  :target: http://target.link/url

  Caption text.
