=================================================================
LSST Dark Energy Science Collaboration : Developing DESC Software
=================================================================

.. raw:: html

   <div id="page">

.. raw:: html

   <div id="main" class="aui-page-panel">

.. raw:: html

   <div id="main-header">

.. raw:: html

   <div id="breadcrumb-section">

#. `LSST Dark Energy Science Collaboration <index.html>`__

.. raw:: html

   </div>

.. rubric::  LSST Dark Energy Science Collaboration : Developing DESC
   Software
   :name: title-heading
   :class: pagetitle

.. raw:: html

   </div>

.. raw:: html

   <div id="content" class="view">

.. raw:: html

   <div class="page-metadata">


.. raw:: html

   </div>

.. raw:: html

   <div id="main-content" class="wiki-content group">

.. raw:: html

   <div class="toc-macro rbtoc1458588141638">

-  `Using Git and GitHub <#DevelopingDESCSoftware-UsingGitandGitHub>`__
-  `Designing a Project: Repository
   Organization <#DevelopingDESCSoftware-DesigningaProject:RepositoryOrganization>`__
-  `Development Tools <#DevelopingDESCSoftware-DevelopmentTools>`__
-  `Coding Style
   Guidelines <#DevelopingDESCSoftware-CodingStyleGuidelines>`__
-  `Code Review <#DevelopingDESCSoftware-CodeReview>`__
-  `Development
   Workflows <#DevelopingDESCSoftware-DevelopmentWorkflows>`__

   -  `Steps for GitHub
      flow: <#DevelopingDESCSoftware-StepsforGitHubflow:>`__

-  `Automated Testing <#DevelopingDESCSoftware-AutomatedTesting>`__

   -  `Unit Tests <#DevelopingDESCSoftware-UnitTests>`__
   -  `Continuous Integration
      Tools <#DevelopingDESCSoftware-ContinuousIntegrationTools>`__

.. raw:: html

   </div>

Welcome! This page contains a growing set of guidelines for people
involved in the development of DESC software, which is to say, all of
you. We are in the accurate cosmology business, which at the very least
means that *the systematics introduced by our code must not dominate our
dark energy parameter error budget.* There's a number of coding
practices that we can adopt from the software engineering industry to
help us achieve that.

We're lucky to have the DM team to learn from: a useful starting point
for DESC development is the \ `DM team's developer
guidelines <http://developer.lsst.io/en/latest/>`__.

A lot of the tips below were introduced and demo'd by Jim Chiang at the
SLAC 2016 collaboration meeting, in the Computing Tutorial session. `You
can view his slides
here <https://confluence.slac.stanford.edu/download/attachments/206768025/CollabCoding_tutorial.pdf?version=1&modificationDate=1457468188000&api=v2>`__,
and the video should be on youtube shortly.

.. rubric:: Using Git and GitHub
   :name: DevelopingDESCSoftware-UsingGitandGitHub

All code should be in \ `DESC
GitHub <https://github.com/DarkEnergyScienceCollaboration>`__ repositories.
This enables the sharing of code throughout the collaboration, enhances
communication, and allows people to work together more easily.

Phil put together a nice FAQ and YouTube tutorial on "`Getting Started
with Git and
GitHub <https://github.com/drphilmarshall/GettingStarted#top>`__," aimed
at people trying to contribute to GitHub projects. The DM team has a
good page on `getting started with git and recommended
practices <http://developer.lsst.io/en/latest/tools/git_setup.html>`__.
 `The Pro Git book <http://git-scm.com/book/en/v2>`__ is good
introduction to git.

.. rubric:: Designing a Project: Repository Organization
   :name: DevelopingDESCSoftware-DesigningaProject:RepositoryOrganization

Since DESC will be working a lot with LSST Stack code, it make sense
to adopt a repository organization that emulates the DM team
conventions. Here is the structure of their \ `standard package
template <https://github.com/lsst/templates>`__:

.. raw:: html

   <div class="code panel pdl" style="border-width: 1px;">

.. raw:: html

   <div class="codeContent panelContent pdl">

.. code:: brush:

    templates
    ├── bin.src
    │   └── SConscript
    ├── CopyrightHeader.cc
    ├── CopyrightHeader.f90
    ├── CopyrightHeader.py
    ├── doc
    │   ├── doxygen.conf.in
    │   └── SConscript
    ├── examples
    │   └── SConscript
    ├── lib
    │   └── SConscript
    ├── python
    │   └── lsst
    │       ├── __init__.py
    │       └── TMPL
    │           ├── __init__.py
    │           ├── SConscript
    │           └── TMPLLib.i
    ├── README
    ├── SConstruct
    ├── tests
    │   └── SConscript
    └── ups
        ├── TMPL.build
        ├── TMPL.cfg
        └── TMPL.table

.. raw:: html

   </div>

.. raw:: html

   </div>

This repository structure has several notable features:

-  The \ ``bin.src`` folder is where command line executable scripts
   should live.  In the setup for this package, this folder would be
   added to the user's PATH environment variable.
-  The \ ``python``\  folder has an \ ``lsst``\  subfolder, which serves
   as a namespace for LSST Stack code, and would be added to the user's
   PYTHONPATH.  For DESC, we would likely have a \ ``desc``\  folder
   instead, so that importing code from this package would look like

.. raw:: html

   <div class="code panel pdl" style="border-width: 1px;">

.. raw:: html

   <div class="codeContent panelContent pdl">

.. code:: brush:

    >>> import desc.TMPL

.. raw:: html

   </div>

.. raw:: html

   </div>

-  There are various \ ``SCons`` build files in some of the folders.
    These would be used in building executables, documentation,
   libraries, `swig <http://www.swig.org/>`__-exposed modules, as well
   as execute test code when the \ ``scons`` build command is issued.
-  The \ ``ups`` folder contains files with configuration for
   the \ ```eups`` package management
   system <https://github.com/RobertLuptonTheGood/eups/>`__ that the DM
   team uses.

.. rubric:: Development Tools
   :name: DevelopingDESCSoftware-DevelopmentTools

-  `Linters <https://en.wikipedia.org/wiki/Lint_(software)>`__ and other
   `static code analysis
   tools <https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis>`__:
    Tools such as `Pylint <https://www.pylint.org/>`__ and `Clang Static
   Analyzer <http://clang-analyzer.llvm.org/>`__  will check coding
   style conventions,but will also check for things like duplicated
   code, whether interfaces are implemented completely and used
   consistently, use of deprecated language features, etc..
-  Editor plugins: The DM team has example configurations for
   `emacs <http://developer.lsst.io/en/latest/tools/emacs.html>`__ and
   `vim <http://developer.lsst.io/en/latest/tools/vim.html>`__ that
   connect those editors to code-checking tools like Pylint or
   auto-completion modules like
   `jedi <https://pypi.python.org/pypi/jedi/>`__.

-  IDEs: Tools like `PyCharm <https://www.jetbrains.com/pycharm/>`__ can
   be useful as they enable debugging and refactoring and have
   interfaces to version control and build systems.

-  DESC will provide template packages (e.g., using
   `cookiecutter <http://cookiecutter.readthedocs.org/en/latest/>`__)
   for creating new software projects.

.. rubric:: Coding Style Guidelines
   :name: DevelopingDESCSoftware-CodingStyleGuidelines

These are specific guidelines for how code should be written.
It includes such things as naming conventions for classes, functions,
and variables (e.g., when to use CamelCase, etc.), line
formatting (indentation and other white-space usage), preferred logic
and coding idioms (using the with statement), etc..

For Python, the `DM style
guidelines <http://developer.lsst.io/en/latest/coding/python_style_guide.html>`__
are essentially the \ `PEP8 Python Style
Guide <https://www.python.org/dev/peps/pep-0008/>`__; while\ `for
C++ <http://developer.lsst.io/en/latest/coding/cpp_style_guide.html>`__,
they are based on a few industry standard conventions
(`CARMA <https://www.mmarray.org/workinggroups/computing/cppstyle.html>`__,
`Geosoft <http://geosoft.no/development/cppstyle.html>`__,
`ALMA <https://science.nrao.edu/facilities/alma/aboutALMA/Technology/ALMA_Computing_Memo_Series/0009/2001-06-06.pdf>`__).

Some benefits of having a standard coding style:

-  Use of common idioms make the code easier to understand.
-  Syntactic consistency makes it easier to spot bugs.
-  New developers have definitive guidance on how to contribute, since
   adopting a software group's established coding style is a social
   norm.

.. rubric:: Code Review
   :name: DevelopingDESCSoftware-CodeReview

-  The aim of code reviews is to have reliable, efficient, maintainable,
   and well-documented code.

-  Using linters and static analysis tools to find and fix syntax
   and other low-level errors before the review frees the code reviewer
   to concentrate on more substantive aspects like \ `algorithms and
   design <http://astromemes.tumblr.com/post/51741245212>`__\ .
-  It is more effective to have code reviews occur *throughout
   the development process* to ensure that a given package proceeds in
   a useful direction from the start.

-  The DM team typically does reviews for code associated with
   a particular development task such as a bug-fix, a feature request,
   or an improvement of a specific aspect of the code.

-  Code review will be essential for "core" code, but it will also
   be useful for non-core code, especially if that code eventually
   gets used for a DESC publication or is integrated into the
   production system.

-  Who does the reviewing? Your co-developers! Code review is a key part
   of `collaborative coding <https://github.com/features#code-review>`__
   - your teammates are best placed to check your code, to catch new
   bugs and help foresee future ones. 
-  The DM team has `a helpful description of their code review
   process <http://developer.lsst.io/en/latest/processes/workflow.html#review-preparation>`__.

.. rubric:: Development Workflows
   :name: DevelopingDESCSoftware-DevelopmentWorkflows

A standard workflow such as \ `GitHub
flow <https://guides.github.com/introduction/flow/>`__ enables multiple
developers to work on the same package while minimizing conflicts that
can arise from concurrent development. Having a standard workflow also
gives clear guidance to new developers on how to contribute.

.. rubric:: Steps for GitHub flow:
   :name: DevelopingDESCSoftware-StepsforGitHubflow:

-  Create a branch off of master. Master should always be deployable
   (i.e., not broken), so development should occur only on branches.

-  Add commits to keep track of work done on the branch. Commits should
   be fairly atomic (that is, small and indivisible), and commit
   messages should summarize the content of the change. Code checking
   tools should be used before making a commit.

-  Push to GitHub, in a remote branch (of the same name, for sanity).
   This could be in your fork, or the base repo if you have push access.
-  Open a Pull Request. This initiates discussion about changes and can
   be made at any stage, e.g., to discuss how the development should
   proceed, or when the code is ready to be reviewed. The PR may also
   trigger the CI tools to do a build and run the tests. 

-  Discuss and Review the code, using the PR thread. Make any changes in
   response to the review, and commit and push to the branch as before.

-  Once all the tests pass and the reviewer is satisfied, merge
   into master.

.. rubric:: Automated Testing
   :name: DevelopingDESCSoftware-AutomatedTesting

Continuous testing is a key means of maintaining software
quality. Running tests regularly can significantly reduce development
time, as they can catch bugs as soon as they are introduced. In
addition, comprehensive tests allow for aggressive
`refactoring <https://en.wikipedia.org/wiki/Code_refactoring>`__, which
is an important part of agile development for producing high quality
code.

.. rubric:: Unit Tests
   :name: DevelopingDESCSoftware-UnitTests

Testing can occur at several levels: system testing,
integration testing, and unit testing. Unit tests are the most granular
and operate at the function and class level:

-  Unit tests should ideally be comprehensive, but if not, they should
   at least *cover the parts of the code where the cost of failure is
   highest*.

-  They should *run quickly*. It should be as painless as possible
   to run the unit tests for a package so that they are run
   often throughout the course of development.

-  *If any tests are broken, they should be fixed before any
   other development proceeds.*

-  For any new development, it is recommended to *write the test
   that exercises that development first*, before touching production
   code. (For bugs, this means writing the test that illustrates
   and localizes that bug first, and keeping it as part of the test
   suite thereafter.)  This is a key feature of `test-driven
   development <https://en.wikipedia.org/wiki/Test-driven_development>`__
   (TDD).  Following TDD practices helps ensure that all of the code is
   tested.

The \ `Dive Into Python <http://www.diveintopython.net/>`__ book
has \ `a good description of unit
testing <http://www.diveintopython.net/unit_testing/index.html>`__, and
the \ `DM team's unit test
policy <http://developer.lsst.io/en/latest/coding/unit_test_policy.html>`__ is
also worth looking at.

.. rubric:: Continuous Integration Tools
   :name: DevelopingDESCSoftware-ContinuousIntegrationTools

Several tools are available for implementing automated testing.  Jenkins
and Travis CI are available for use by DESC:

-  `Jenkins <https://jenkins-ci.org/>`__ is a Java-based CI service. We
   have an \ `instance running
   at SLAC <http://srs.slac.stanford.edu/hudson/>`__\ , so we can use
   SLAC resources (disk space, LSST Stack code installations) for
   building and testing DESC code. This is great for projects that
   depend on the LSST DM stack and Sims tools.
-  `Travis CI <https://travis-ci.org/>`__ is a remotely hosted service
   that can be easily connected to and configured for a GitHub repo.
   It's a free service for public repositories, and is great for
   projects that don't have a lot of dependencies.

 

.. raw:: html

   </div>

.. raw:: html

   </div>

.. raw:: html

   </div>

.. raw:: html

   <div id="footer" role="contentinfo">

.. raw:: html

   <div class="section footer-body">

Document generated by Confluence on Mar 21, 2016 15:22

.. raw:: html

   <div id="footer-logo">

`Atlassian <http://www.atlassian.com/>`__

.. raw:: html

   </div>

.. raw:: html

   </div>

.. raw:: html

   </div>

.. raw:: html

   </div>
