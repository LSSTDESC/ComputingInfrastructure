..
  Template for LSST DESC Notes, including guidelines for authors.

  Heather Kelly & Phil Marshall, Summer 2016

  See also:
  * https://github.com/lsst-sqre/sqr-000/blob/master/index.rst for an LSST technote deescribing LSST technotes, on which DESC notes are styled.
  * https://github.com/lsst-dm/dmtn-008/blob/master/index.rst for a nice example LSST technote by Michael Wood-Vasey, which is rendered by the LSST technotes system at http://dmtn-008.lsst.io/en/latest/
  * http://docs.lsst.codes/en/latest/development/docs/rst_styleguide.html for a guide to reStructuredText writing, and https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst for a nice cheatsheet.



===============================================
LSST DESC Notes: Template and Author Guidelines
===============================================

*Heather Kelly, Phil Marshall*

.. |date| date::
This document was generated on: |date|


Introduction
============
This is a template LSST DESC Note, for you to adapt for your own work.

Sectioning 
==========
Your content can easily be divided into subsections, as follows.

A Subsection
------------
You can also have subsubsections, like this:

A Subsubsection
^^^^^^^^^^^^^^^
See? This is a subsubsection.

Another Subsubsection
^^^^^^^^^^^^^^^^^^^^^
And so is this.

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
To add images, add the image file (PNG, SVG or JPG preferred) to the ``_static`` subdirectory in this note's folder. Here's an example:

.. figure:: ./_static/desc-logo.png
  :name: fig-logo
  :target: ./_static/desc-logo.png
  :width: 200px
  :align: center

  This is the figure caption: above we have the LSST DESC logo, in PNG format.

And then the text continues. Note that GitHub ignores the image sizing commands when presenting reST format documents; sphinx might not.


References
==========
You can cite papers (or anything else) by providing hyperlinks. For example, you might have been impressed by the DESC White Paper `(LSST Dark Energy Science Collaboration 2012) <http://arxiv.org/abs/1211.0310>`_.  It should be possible to convert these links to latex citations automatically later. 
