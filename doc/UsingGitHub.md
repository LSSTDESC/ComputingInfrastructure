# Using GitHub

Phil Marshall and others

This page is for tracking the collaboration's use of GitHub, and any needs arising from it. If you add to it, please include your name at the top.

### Sharing Code

This seems to be working fine. See the [cookie cutter]() project for our attempt to help DESC members start projects with well-organized and easily importable repos 


### Communication via the Issues system

We are still learning how best to do this. Default GitHub behavior is to send **notifications about all issues** and pull requests to each person with "Write" or "Admin" repo access, as well as to everyone else who has opted to "Watch" the repo. This can be over-ridden in your profile settings, but this is not obvious to people. Repos can also be "unwatched" and individual issues can be "unsubscribed" from, but again, this is not readily apparent. The [Getting Started](https://github.com/drphilmarshall/GettingStarted#watching) repo now has an [FAQ about this0(https://github.com/drphilmarshall/GettingStarted#watching), which probably needs advertizing. 


### Logging progress in an "Online Lab-book"

This came up in the Twinkles project, [in issue #186](https://github.com/DarkEnergyScienceCollaboration/Twinkles/issues/186#issuecomment-205454214). 

* Issue threads can be treated as lab-book pages, tracking results and supporting continuous editing - but this is perhaps not obvious. Some example usage of this could be helpful.
* Repo wiki pages are potentially useful as lab-books, but are not widely used in the DESC yet. The [Reprocessing Task Force](https://github.com/DarkEnergyScienceCollaboration/ReprocessingTaskForce/wiki) uses their wiki extensively.
* IPython notebooks are popular, but how best to work with them in GitHub is not clear. It could be hard to keep code repositories light weight if evolving notebooks containing plots are present. Checking in clean notebooks (with all outputs reset) avoids this problem but renders the notebook much less useful (as you can no longer see the plots unless you run the notebook yourself). Notebooks can be checked in to GitHub wikis (which are themselves git repos) but they do not render, and do not appear in the wiki page list.
