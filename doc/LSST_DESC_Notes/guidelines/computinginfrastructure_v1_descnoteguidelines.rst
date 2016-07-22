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

Heather Kelly, Phil Marshall

Version: 1

Steps 
======

* Locate the appropriate GitHub repository where the new LSST DESC Note should be stored and fork the repository
* Under the doc/LSST_DESC_Notes directory, create a new subdirectory with a 
* Copy the CI LSST DESC Note template into your new directory
* Write the Note
* When complete and ready for review, submit a Pull Request
* Project leads will review the note, iterate via comments on the Pull Request, and merge into the repository when the note is accepted

To Do
======








