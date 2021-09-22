Answers to the most common questions we get
===============

- [Branching Strategy](#branching-strategy)
- [App Versioning Strategy](#app-versioning-strategy)
- [Using ATF](#using-atf)
- [Using the CI/CD Spoke](#using-the-cicd-spoke)
- [Deployment Model Differences](#deployment-model-differences)
- [Questions to help guide next steps](#questions-to-help-guide-next-steps)


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

Q. Would you recommend that we stay on-platform and use the CI/CD Spoke with Flow Designer, or instead start off-platform by learning how to use <popular CI/CD tool> that our engineering teams use?

> A. If the customer is just starting out with CI/CD conceptually, and has no experience using tools like GitLab, Jenkins, Azure Pipelines, GitHub Actions, etc., using the spoke with Flow Designer may give them some practice with understanding how to set up an automated testing and deployment pipeline. However, from the perspective of spending the time and effort to build a pipeline and workflow that will last the next 5 or 10 years, we highly recommend that they choose a dedicated CI/CD tool from the ecosystem. 

> A short list of reasons why Flow Designer + CI/CD Spoke isn't a full replacement for a mature CI/CD build automation tool:
> - There isn't a flow trigger for either triggering on pushes to branches or from pull requests in the Git repo an app is linked to Source Control with. 
> - Most Git repo products provide the option to automatically merge if the build passes for a pull request, but Flow Designer isn't easily set up for that. You could play around with the GitHub Spoke to get something working with the webhooks, but it's extra work. 
> - There are no reusable environment variables in Flow Designer.
> - There isn't a scripted pipeline option for Flow Designer. 
> - It's not easy to see a history of pipeline jobs for specific branches in Flow Designer. 
> - Hard to define flows based on building from feature branches for testing vs master branch for release. 
> - Dedicated tools like Jenkins, GitLab, GitHub Actions, and Azure Pipelines have huge engineering teams behind them driving improvements specifically to address user/customer needs for DevOps workflows. Flow Designer is a generic automation tool for Now Platform. 
> - Many products incorporate both a Git repo and pipeline tool together (e.g. Azure DevOps with Repos and Pipelines, GitLab), and have tons of features to make them play well together, given customers plenty of configuration options for their specific needs and workflows. You'll be struggling just to hack something together with Flow Designer and the CI/CD Spoke. 
> - At the end of the day, it's hard to create a true CI/CD pipeline that interfaces with branches and pull requests for merging in a standard DevOps model. 

> Please just use tools like GitLab, Jenkins, Azure Pipelines, GitHub Actions, etc. We even provide [nice extensions and template pipeline scripts](https://github.com/ServiceNow/devproductivity-guides/tree/master/pluginsAndIntegrations) for you to get started faster! 

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

Deployment Model Differences
---------------

The behavior or state of applications as being "deployed" to instances differs between Update Sets, App Repo installs, versus Source Control. App Repo is just the artifact store that handles the app zips as they get published/installed between instances. 

Some helpful documentation topics that we published recently to clarify these behaviors: 
- https://docs.servicenow.com/bundle/rome-application-development/page/build/applications/concept/tips-production-deployment.html
- https://docs.servicenow.com/bundle/rome-application-development/page/build/applications/concept/tips-customer-updates.html
- https://docs.servicenow.com/bundle/rome-application-development/page/build/applications/reference/generation-skip-records-app-installs.html
- https://community.servicenow.com/community?id=community_blog&sys_id=ef21c1d21bf04c507a5933f2cd4bcb34

In short, applications that are "in development" live in the `sys_app` table, and this is where both Update Sets and Source Control expect apps to be.
Applications that are being deployed to another instance from the App Repo will live in the `sys_store_app` table. 
Transitioning from one model to the other on an instance for a given app does require conversion, which we're working towards providing features for (rather than relying on the support org workflow). 

Q: Incompatible with apps previously deployed via Update Set
> - With App Repo based deployment, applications have two types of instances that interface with it - app-author instances where development is done, versus app-client instances where the app is installed from the App Repo. In the former case, the app lives in the `sys_app` table, whereas in the latter, the `sys_store_app` table. This is to distinguish between the two types of instances. 
> - We are providing a feature in San Diego to convert applications from `sys_app` to `sys_store_app` table, therefore enabling customers to self-serve and choose whether a particular instance is meant to be an app-author or app-client instance for a given application. 

Q: Inability to force deploy over local instance artifacts instead of skipping
> - This is by design, because otherwise local changes would get overwritten, perhaps unintentionally, on application installs from App Repo. We would not recommend continuing to do development work on app-client instances where applications are being installed to. If there are local hotfixes the customer wishes to blow away during app installs, deleting from the sys_update_xml table, then installing the app, should overwrite it. 

Q: Lock out of app after PROD clone to DEV
> - If the `sys_app` record exists on the target (DEV) instance for a clone operation, we have logic that handles conversion of the app from the `sys_store_app` table to the `sys_app` table properly so development can continue. 
> - If the `sys_app` record doesn't exist on the target (e.g. another DEV2 instance) instance for a clone operation, the app will continue existing on `sys_store_app` table, where it lives in Prod. For app development on that DEV2 instance to be enabled, either the app will need to be pulled from Source Control or will need to have a one-time conversion completed via the San Diego feature. 

Q: Unable to restore GIT repo version over cloned `sys_store_app` copies
> - This is also by design, because app-client instances aren't intended to be used for development for that given app. 
> - We'll take a feature request to provide self-serve options for customers to transition an app from `sys_store_app` to `sys_app` (opposite direction from SD feature), so that they can set up an instance more flexibly. 

Q: No easy way for us to self-convert app from `sys_store_app` to `sys_app` for additional development after clone
> - See 3b and 4b. 
Q: Cap on max number of versions stored in App Repo
> - Correct, the current maximum number of versions for a given app in App Repo is 20, and cannot be configured by customers. We are looking into how to provide more configurability by separating the App Repo functionality from the Store. 

Q: Cap on max number of artifacts allowed in an application.  A few of our apps are too big to live in the Repo as a single app
> - Similar to 6b, there are constraints with the existing App Repo that we need to open up configuration for by decoupling App Repo (customer/partner development use cases) from the Store (partner and SN publishing use cases). 

Q: Undesirable sys_metadata_delete removal of destination instance artifacts after local instance recreation of those artifacts
> - See above articles on author elective delete properties. We're taking note that these configurations are not intuitive for customers to understand how to set up to achieve their desired deletion behaviors. 
> - This also applies to the sys_choice issues mentioned in the support cases. 
> - Separately, I noted there's another case on Dictionary Entry issues. Dictionary Entry is a column. Therefore, if there's data in it (e.g. in Prod), we won't delete it. The workflow would be to delete the data in that column in Prod, then attempt an install from the app. The rationale for this is so that customers don't accidentally delete data unintentionally. 

Q: Inability to convert our release operations to follow one standard best practice because they will need to support Update Set deployments simultaneously with App Repo deployments
> - For a given application, customers should choose one model for deployment onto an instance, rather than attempt to mix Update Sets, Source Control, and App Repo installs. 
> - However, different applications can have a mix of models. For example, if the HR or CSM customizing development squad isn't ready to adopt app-driven workflows yet (e.g. on Paris rather than Q or R), their customizations can continue to be migrated via Update Sets. Meanwhile, other scoped app or global app teams can adopt app-based development. 
> - Most customers aim for a vision of onboarding all teams and apps to the same, unified, development/pipeline model at some point. This is a multi-stage journey. 

Questions to help guide next steps
---------------

1. Have they adopted Source Control yet? Are they familiar with using Git branches or doing pull requests to merge into master? For example, a branching strategy like [GitHub flow](https://guides.github.com/introduction/flow/) as a starting point. 
2. Have they had a chance to check out our [Paris release features](https://developer.servicenow.com/blog.do?p=/post/paris-source-control/) yet? Specifically, supporting global apps with Source Control, and delta-loading. 
3. How familiar are they with build automation tools like Azure Pipelines or Jenkins? Do they already have a preference? Alternatively, they can also get started on-platform with Flow Designer and subflows using the [CI/CD Spoke](https://docs.servicenow.com/bundle/orlando-servicenow-platform/page/administer/integrationhub-store-spokes/concept/cicd-spoke.html) to get a quick and simple pipeline running. 
4. Are they using [ATF for automating testing](https://docs.servicenow.com/bundle/paris-application-development/page/administer/auto-test-framework/concept/automated-test-framework.html) as a part of today's workflows? It's one of the bigger benefits of adopting CI. 
