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

Q. "How do I manage multiple developers needing to work on the same app in the same instance, but on separate stories representing different features/fixes with different timelines? Should they all work on the same branch?"

> A. Let's first review the intended workflow:
> - You have an app. You have a backlog of stories to work on.
> - Multiple devs need to work on their own separate stories at the same time, on that same app.
> - Devs should branch off master to create feature branches for their own work, so they can merge/build/deploy independently.
> - Only one feature branch can be active for an app on a single instance, so this provides incentive for the customer to purchase additional dev instances, so multiple devs can work simultaneously on their own branches, rather than waiting for each other to finish on a single dev instance.
> - Customers are not limited the way SURF is when publishing apps to their app repo. Any app author or dev instance can publish, no need to register just one.

> General guidelines:
> 1) If all your work must be in a single app, then definitely make use of multiple branches to track different pieces of work (features, fixes, etc.). Pull requests should drive when code reviews and CI builds happen, and therefore which branch needs to be active on an instance in order to publish an application as a version.
> 2) If you have multiple developers working on the same app, then they should use different branches. Ideally, they would also have their own instances so they don't conflict when simultaneously developing.

> The analogy is performing a maven release. As a dev, assuming my build passed and PR merged, I can switch to the master branch and do a maven release locally. This will produce an artifact I can deploy to prod. Any dev on a team could do this.
> Alternatively, if we have a CD setup, we can also have a dedicated environment in our CD pipeline that does the maven release from master branch once a build has passed and branch is merged. That pipeline could also auto-upload the artifact to Artifactory, and deploy to prod. This would be like having a publishing instance. The advantage here over each dev handling release locally is less work for the dev, and having a standardized release pipeline for the whole team.

> The feature request I am hearing here is "As a customer, each developer on my team should be able to work on their own separate branches simultaneously on the same app in the same dev instance." The solution that was proposed has been Individual Developer Instances - that is, if you need more branches, then get more instances so you can work simultaneously on multiple branches for the same app. I do recognize the cost barrier to purchasing more instances, and time spent cloning down to many dev instances. Our strategy going forward will be figuring out how to 1) reduce the amount of time it takes to prepare a dev instance from moment of request, and 2) decrease the cost of an instance by either decreasing the disk usage or by decreasing the time the instance needs to live. I also understand that customers are coming from Update Sets, where multiple devs in the same instance can each be in their own Update Set and not conflict with each other while working on their own separate stories. Customers will need to be educated on the benefits of moving from a model where code just lives in an environment and changes are packed and moved as Update Sets, to one where code is packed into apps and versioned accordingly, and changes are managed via Git branches and pull requests are used to review and check for conflicts on merge.


Questions to Ask Your Customer
---------------

1. Have they adopted Source Control yet? Are they familiar with using Git branches or doing pull requests to merge into master? For example, a branching strategy like [GitHub flow](https://guides.github.com/introduction/flow/) as a starting point. 
2. Have they had a chance to check out our [Paris release features](https://developer.servicenow.com/blog.do?p=/post/paris-source-control/) yet? Specifically, supporting global apps with Source Control, and delta-loading. 
3. How familiar are they with build automation tools like Azure Pipelines or Jenkins? Do they already have a preference? Alternatively, they can also get started on-platform with Flow Designer and subflows using the [CI/CD Spoke](https://docs.servicenow.com/bundle/orlando-servicenow-platform/page/administer/integrationhub-store-spokes/concept/cicd-spoke.html) to get a quick and simple pipeline running. 
4. Are they using [ATF for automating testing](https://docs.servicenow.com/bundle/paris-application-development/page/administer/auto-test-framework/concept/automated-test-framework.html) as a part of today's workflows? It's one of the bigger benefits of adopting CI. 
