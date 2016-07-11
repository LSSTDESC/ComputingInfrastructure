# Using git and GitHub

In this tutorial we will walk through making a first contribution to a
project hosted at GitHub, covering the following topics:

* What `git` repositories are for, and what the GitHub web service provides and enables
* How to communicate with others on GitHub, via the "issues" system
* How to make remote "fork" and a local "clone" of a project repo
* How to use `git` to save a new version of the project files, containing your edits
* How to contribute your work back to the project, by "pull request"
* How to use GitHub to manage your projects using milestones and labels

We'll use the "[GettingStarted](https://github.com/drphilmarshall/GettingStarted)"
repository hosted at github.com for the hands-on demo. It contains a single
markdown-format FAQ file
about `git` and GitHub, which hopefully you will find useful to refer to later.
In the tutorial, you will contribute to the GettingStarted project by fixing
various bugs in the markdown "code" that it contains.


-----

### Task: Improve Phil's "Getting Started with `git` and GitHub" FAQ

* The FAQ in the [test repository](https://github.com/drphilmarshall/GettingStarted)
is being developed collaboratively as an open source project.

* Can you see something that needs improving? If so, [post an issue](https://github.com/drphilmarshall/GettingStarted/issues). You'll want to
search the existing issues first to see if the problem is already known about.

* Can you fix the bug yourself, even partially? If so, then:

  * Comment in the issue thread that you can work on it.
  * *Fork* the repository, and
  * *Clone* it to your local machine (setting up your ssh keys if you need to).
  * Edit the file(s) and *commit* your changes to make a new version,
  * Then *push* those changes back to your fork - and then:
  * Ask for those improvements to be merged into the base repository by submitting a *pull request*.
  * You might even need to *add* a new file.

* As the FAQ evolves, you'll want to keep up with the latest version.

  * Add a new *remote*, a link to the *base repository* and then
  * *Pull* the latest changes down from that remote, and
  * Fix any *conflicts* that occur.

* This project seems interesting. Where is it going, longer term?

  * We'll look at its milestones and epics, to see what Phil has in mind.
  * We'll then take a look at the [Twinkles project on GitHub](https://github.com/DarkEnergyScienceCollaboration/Twinkles), for a
  more complex LSST DESC example that shows the content of this tutorial in action.
  For example, check out:
    * [the current Twinkles epics](https://github.com/DarkEnergyScienceCollaboration/Twinkles/issues?q=is%3Aissue+is%3Aopen+label%3AEpic)
    * [the current Twinkles milestones](https://github.com/DarkEnergyScienceCollaboration/Twinkles/milestones)
-----

### Resources

* A video of much of the material in this tutorial can be found [on YouTube ](https://www.youtube.com/watch?v=2g9lsbJBPEs).

* There are more links on the "[GettingStarted](https://github.com/drphilmarshall/GettingStarted)"
FAQ page to more documentation about `git` and GitHub. Usually you can find
what you need just as quickly by typing `git` commands or GitHub features into
Google, though.
