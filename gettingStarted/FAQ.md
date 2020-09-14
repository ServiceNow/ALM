Answers to the most common questions we get
===============

Branching Strategy
---------------
Q. "Is there a best practice or recommendation for how to use branches? For instance, for the simple case that we have discussed, where we have released version 1 and we are working on development of version 2 and supporting bug fixes for version 1, should we have a branch for bug fixing and do development of version 2 in the master branch, have a branch for version 2 and develop bug fixes in the master branch, or have two branches, one for bug fixes to version 1 and one for version 2 development (or something else)? Are there general recommendations like “minimize the number of branches” that we should be aware of?"

> A. Yes there absolutely are best practices. Start with GitHub flow. Therefore, for this hypothetical case, checkout two branches from master. Merge whichever branch gets past the build/testing into master first. I don't recommend minimizing branch count (why?), but it's a reflection of how many "pieces of work" are ongoing at a time, so you should be trying to get those pieces of work finished and merged into master ASAP anyway. 

Q. "Are there recommendations for releases? For instance, should we always release to production from the master branch?"

> A. I would recommend starting out with always releasing from master just to make sure your team's process is drilled in. Once you understand why certain feature branches (or railroad track branches) get to parity with master branch after merge, and therefore any apps built from that branch can just be released, then you can start streamlining your workflow. 

Q. "Are there recommendations for merging? For instance, should we merge after every bug fix? This will depend on the answer to #1 and how we handle branching, but is there a general recommendation?"

> A. It's far easier to merge smaller pieces of work that can pass the CI build with minimal changes to tests, than it is to merge a giant piece of work. So merge early and often. 

Q. "Is there a good way to handle code reviews? Right now we review story-by-story using the update sets for the story to determine what to review. With source control, there will not be separate update sets for each story, so that will not be possible. Is it possible to create a branch for each story and code review that? Should code reviews be done before code is committed or after?"

> A. This goes back to customers being confused on how to move from Update Sets to using apps, where they need to understand where the abstractions to branching and app versioning come in. In this case, a branch could be a single story, or several stories. As a dev, I should probably be linking the pull request to the story (or stories) that are tied to it. Or commenting in the pull request which stories are included in it. You don't have to do 1 branch for 1 story, that sounds tedious and might not make sense from a functional feature perspective. Code should be committed to a branch first, and a pull request opened, THEN a code review happens. 

App Versioning Strategy
---------------

Using ATF
---------------

Using the CI/CD Spoke
---------------

How to migrate from Update Sets to using Apps?
---------------

Q. "I’ve had a question recently about how we would support versioning within workflows (and subsequent promotion through the envs) – both from an HR and SecOps perspective. In the example where they want to update an existing HR workflow through an App Repo approach, would this be supported today, or will they need to wait until we support OOTB apps in Quebec? My current understanding is that this use case wouldn’t be available until Quebec but just wanted to confirm with you to make sure I understand this correctly."

> A. If they've customized the HR or SecOps apps (within the scope of those apps), then they will need to wait until Quebec for the Store/OOTB app support for Source Control and App Repo workflows. 
However, if they've been building in their own scoped apps or global (they can add  update sets into global apps, for example), those are available as of Paris. 
Also, one of the major advantages of moving to using Git and App Repo is the ability to abstract from just moving code (promoting update sets) between environments directly, to being able to compare code between Git branches, and then being able to publish/install/rollback apps to load/unload them into an environment. These are powerful features that help customers to better manage how they develop changes (features, fixes, etc.) and eventually put them in front of their end users in prod.
