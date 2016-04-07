# Using GitHub

*Phil Marshall and others*

This page is for tracking the collaboration's use of GitHub, and any needs arising from it. If you add to it, please include your name at the top.

### Sharing Code

This seems to be working fine. See the [DESC cookie cutter](https://github.com/DarkEnergyScienceCollaboration/desc_package_template) project for the start of an attempt to help DESC members begin projects with well-organized and easily importable repos. 


### Communication via the Issues system

We are still learning how best to do this. Default GitHub behavior is to send **notifications about all issues** and pull requests to each person with "Write" or "Admin" repo access, as well as to everyone else who has opted to "Watch" the repo. This can be over-ridden in your profile settings, but this is not obvious to people. Repos can also be "unwatched" and individual issues can be "unsubscribed" from, but again, this is not readily apparent. The [Getting Started](https://github.com/drphilmarshall/GettingStarted#watching) repo now has an [FAQ about this](https://github.com/drphilmarshall/GettingStarted#watching), which probably needs advertizing. 


### Managing projects with Issues, Milestones, Labels and ZenHub

Bryce Kalmbach (@jbkalmbach) and Phil Marshall (@drphilmarshall) are attempting to use GitHub and ZenHub to manage the [Monitor](https://github.com/DarkEnergyScienceCollaboration/Monitor) and [Twinkles](https://github.com/DarkEnergyScienceCollaboration/Twinkles) projects, respectively.

* "Epics" are now supported by ZenHub, although our [homegrown labeling solution](https://github.com/DarkEnergyScienceCollaboration/Twinkles/issues?q=is%3Aopen+is%3Aissue+label%3AEpic) seems to get almost all of that functionality.
* "Burndown charts" are provided by ZenHub, and hopefully will help us manage our time better. They rely on the assignment of "complexity" to each issue.
 
Eventually, a short page on how to do all this in the DESC context will be in order. 

### Logging progress in an "Online Lab-book"

This came up in the Twinkles project, [in issue #186](https://github.com/DarkEnergyScienceCollaboration/Twinkles/issues/186#issuecomment-205454214). 

* Issue threads can be treated as lab-book pages, tracking results and supporting continuous editing - but this is perhaps not obvious. Some example usage of this could be helpful.
* Repo wiki pages are potentially useful as lab-books, but are not widely used in the DESC yet. The [Reprocessing Task Force](https://github.com/DarkEnergyScienceCollaboration/ReprocessingTaskForce/wiki) uses their wiki extensively.
* IPython notebooks are popular, but how best to work with them in GitHub is not clear. It could be hard to keep code repositories light weight if evolving notebooks containing plots are present. Checking in clean notebooks (with all outputs reset) avoids this problem but renders the notebook much less useful (as you can no longer see the plots unless you run the notebook yourself). Notebooks can be checked in to GitHub wikis (which are themselves git repos) but they [do not render](https://github.com/DarkEnergyScienceCollaboration/Twinkles/wiki/Test.ipynb), and also [do not appear in the wiki page list](https://github.com/DarkEnergyScienceCollaboration/Twinkles/wiki).
