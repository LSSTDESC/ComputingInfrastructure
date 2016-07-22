..
  Guidelines for Writing LSST DESC Notes. 

  See http://docs.lsst.codes/en/latest/development/docs/rst_styleguide.html
  for a guide to reStructuredText writing.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file to the same directory containing your note
  The reST syntax for adding the image is

  .. figure:: /filename.ext
     :name: fig-label
     :target: http://target.link/url

     Caption text.

   Feel free to delete this instructional comment.

======================================
Guidelines for Writing LSST DESC Notes. 
======================================

:Authors: - Heather Kelly
          - Phil Marshall

:Version: 1

Steps 
======

* Locate the appropriate GitHub repository where the new LSST DESC Note should be stored and fork the repository
* Under the doc/LSST_DESC_Notes directory, create a new subdirectory with a 
* Copy the `Computing Infrastructure LSST DESC Note template <https://github.com/DarkEnergyScienceCollaboration/ComputingInfrastructure/blob/master/doc/LSST_DESC_Notes/template_LSST_DESC_Note.rst>`__ into your new directory
* Rename the template to follow the form:  *project_version_filename.ext*

 

* Edit your new file with the contents of your note
* When complete and ready for review, submit a Pull Request
* Project leads will review the note, iterate via comments on the Pull Request, and merge into the repository when the note is accepted

To Do
======

* `Create setup script <https://github.com/DarkEnergyScienceCollaboration/ComputingInfrastructure/issues/28>`__ 
* Iterate on the guidelines above
* Further test the suitability of Pandoc for conversion into PDF, and take another look at Sphinx
* Enable travis-ci to automate creation of PDF and population of `GitHub-pages <http://darkenergysciencecollaboration.github.io/ComputingInfrastructure/>`__







