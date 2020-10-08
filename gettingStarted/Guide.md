# I am not done with this!


**What processes and tools to choose for developing with Source Control and CI/CD on ServiceNow?**

The purpose of this article is to clarify:

- Where do I get started if I don&#39;t know?
- What are the recommended best practices for a DevOps-driven development process?
- What are the tradeoffs when making tooling decisions?

This article assumes an understanding of:

- The purpose of DevOps in driving quality of applications and delivery velocity for the organization.
- Familiarity with common DevOps ecosystem tools, such as Jenkins, Azure DevOps, GitHub, GitLab, etc.
- Familiarity with ServiceNow capabilities and development of apps using Studio.

Summary of Product + Development Process:

![](https://github.com/ServiceNow/devproductivity-docs/blob/master/gettingStarted/product_dev_workflow.png)

What happens in Discovery?

- This is how your team identifies the needs of your users. For example, how do you handle intake of feature requests, bugs, new application requests, etc. Are users successful at using what you built? Is your roadmap heading in the right direction?
- Common patterns for handling intake
  - Talk directly to your users
  - Ticketing system
  - Feature upvote system
  - Community forums/channels
- Tooling choices
  - If your priority is to link intake request tickets to stories on a backlog, you may consider using the same tool, i.e. ServiceNow ITSM with the Agile Development 2.0 product.
  - If your priority is to share the same engineering backlog tool between the company, so all development teams share the same structure for handling projects, linking stories between backlogs for dependencies, you may consider using a common backlog tool (e.g. ServiceNow Agile Development 2.0, Azure Boards, Jira, Trello, etc.)
- But at the end of intake, it&#39;s important to document user stories in your agile backlog of choice, so they are ready to be prioritized and planned for work in sprints. This leads to our next section, Planning.

What happens in Planning?

- This is how the intake of needs turn into features on a prioritized backlog of stories (or a roadmap). This is also where you align stakeholders and users on prioritization of your backlog. You may start breaking down user stories into designs and engineering stories.
- Common patterns for handling planning (alignment on prioritization)
  - Understand your users&#39; needs and objectives
  - Clearly defined goals and metrics (how you&#39;re measuring success towards goals)
  - Roadmap (or backlog) in prioritized rank order in a deck or backlog tool of choice
  - Measure outcomes on what you&#39;re building and how it&#39;s being used to feed back into future prioritization
  - Some organizations also choose to do UI design work at this stage, so you may find working with UI/UX designers on tools like Balsamiq, Sketch, Invision, or Miro easier to show mockups of workflows and views.
- Tooling choices
  - Typically, an agile backlog tool that can show a roadmap-level view of priorities. Alternatively, simple slides reflecting the same.
- At the end of planning, you should have alignment with your stakeholders/users on the prioritization of your roadmap, and you&#39;re ready to break down into epics and stories on an engineering backlog.

What happens in Execution?

- Once you have validated that you&#39;re solving for a real user need and prioritizing building the right things, then it&#39;s time to turn into engineering stories to execute from. Generally, features from roadmaps are translated into epic on engineering backlogs, then broken down into engineering stories to be estimated and prioritized into sprints. Once a story is in a sprint, an engineer can pick up that story and begin working on the feature, which will generally go through the code, build/test, release/deploy workflow.
- Epics and Stories
  - Once prioritization of your roadmap has been aligned with stakeholders, you&#39;re ready to break down the near-term features into epics and stories on your engineering backlog. These stories are intended to capture the work that will be done and should reflect what the feature will end up looking and working like.
  - Common patterns for breaking down into epics and stories
    - Firstly, make sure you have mockups and workflow diagrams clearly documented so the team has something to refer to when turning features into epics and stories.
    -
  - Tooling choices
    - Backlog:
- Code
  - Fairly self-explanatory, the goal here is to manage your code changes with a larger team such that it&#39;s easy for the team to work on separate features while providing code review coverage for each other.
  - Common patterns for coding best practices
    - Source-driven development with branching workflows. We recommend that your team use a Git repo to manage your code, and separate branches representing a developer working on a separate feature or story from your engineering backlog.
    - Stories can then be linked to a branch or a pull request, depending on the model you prefer for managing and seeing changes.
    - General workflow for a developer:
      - Start from master branch, which is the current build of the application in production.
      - On your development environment, create a branch from master, representing changes you are making for a story or feature.
      - Make sure to commit your changes as you are developing, it helps to keep track of the changes you are making. In dev Studio, when using Source Control, committing also pushes your code to the linked remote Git repo.
      - Once you are ready, you can create a pull request in your Git repo product (e.g. GitHub, Azure Repos), which compares the changes between the two branches in preparation for a merge. You should do a code review in your team at this point.
      - You can then set up your Git repo such that on pull request, a CI build is kicked off to determine whether the branch passes all of the unit/integration/functional tests and reports back to the pull request whether the build had a success or failure outcome. This, in conjunction with the code review, should determine whether your team should go ahead with the code merge, or have the developer go back and make changes on their dev branch.
      - Once your pull request has passed code review and the CI build, you can safely merge to master and then determine whether to release the master branch as a build of the application.
  - Tooling choices
    - For developing on ServiceNow, it&#39;s best to use either the [built-in Studio product](https://docs.servicenow.com/bundle/orlando-application-development/page/build/applications/concept/c_ServiceNowStudio.html), or [VS Code](https://code.visualstudio.com/)[with our extension](https://marketplace.visualstudio.com/items?itemName=ServiceNow.now-vscode) for scripting. You may find it difficult to interpret the XML files in the directories for our app if using another IDE.
    - For source control, as long as it&#39;s a Git repo, our Source Control features will work with any of them. For example, GitHub, GitLab, Azure Repos, just to name a few. Our recommendation would be to stick with what the rest of your organization is using, so it&#39;s easier to organize code collaboration workflows when doing code reviews, integrating webhooks into build servers, etc.
- Build/Test
  - Continuous integration is intended to help you validate whether a dev branch should be merged into your master branch as a part of a pull request. This also means that ideally, your build servers should be able to kick off their own set of test environments, specific to that particular CI build each time. That way, multiple developers can each submit their own PRs for their own dev branches, and aren&#39;t blocked on running the CI build while waiting for another developer&#39;s branch to finish testing.
  - Common patterns for Continuous Integration build/test best practices
    - Automated Tests
      - Smoke
        - This is a small suite of tests that lets you know early whether the rest of the CI build is worth running, and helps you to save time if there are critical errors/bugs in the branch being tested.
      - Unit
        - These are tests intended to test whether an individual method or class works by itself. In the ServiceNow world, you may extend this to whether a particular table or field works the way as intended.
      - Integration
        - These are tests where you start combining functionality to make sure they work together. For example, perhaps you have data from one application and workflow being completed in another one, you should have a test that covers the complete end-to-end workflow.
      - Functional
        - Functional tests are sometimes extensions of integration tests, but focus primarily on the click-and-scroll part of the experience. Sometimes also known as UI testing.
      - Acceptance
        - This is where your product manager, product owner, or team tries out the features in a staging environment to make sure it works as intended, and meets the acceptance criteria you defined for the story.
    - Reporting build results
      - Build tools generally have their own UI for showing the results of builds as pass or fail, and which specific tests failed. This is the detailed level view your team will need to rely on to figure out what to fix.
      - You can also report the build result directly into the PR so it&#39;s easier to track the &quot;progression&quot; of a dev branch. This can be accomplished via webhooks (e.g. GitHub to Jenkins), or just via the platform capabilities (e.g. Azure DevOps products).
      - You can also choose to report the build result to another third party tool to aggregate in one place, for example ServiceNow Enterprise DevOps product.
  - Tooling choices
    - API vs Spoke.
      - ServiceNow offers to a two-pronged strategy when it comes to choosing setting your CI/CD build.
      - Firstly, if you&#39;re experienced in DevOps, and already have your favorite set of 3rd party ecosystem tools to bring with you, we provide generic API endpoints to integrate with so you can automate deployment of your Now Platform applications, running ATF test suites, etc.
      - However, if you don&#39;t have any experience using 3rd party tools such as Azure, Jenkins, GitLab, etc., we also want to make it easy for you to get started by providing capabilities to build rudimentary CI/CD workflows with Flow Designer, via subflows published with the CI/CD Spoke on the IntegrationHub store.
      - Lastly, as part of our strategy to make it faster to get started regardless of which bucket your team falls into, we will be providing extensions and plugins published on the most popular marketplaces, so customers can get started with CI/CD builds faster using pre-built templates and &quot;steps&quot;.
    - Build server.
      - Build servers and pipeline automation tooling allow you to define a CI or CD build, when to trigger it, and have it automated to increase your team&#39;s delivery velocity.
      - Common choices include Jenkins, Azure Pipelines, GitLab, to name a few. Your team should choose the tool that the rest of your company is using, so share the knowledge and configurations where possible.
    - Code repository.
    - Application repository.
    - Test environments and setup.
    - Test framework.
      - On ServiceNow, Automated Test Framework is the recommended starting point for setting up your tests and test suites for automated test coverage. You will want to run this in a subprod environment specific to use as a test environment.
      -
    - Source control for tests.
- Release/Deploy
  - Continuous delivery is intended to
  - Common patterns for Continuous Delivery release/deploy best practices
    - Application versioning
    - Production deployment
    - Change management
    - Updating stories
  - Tooling choices
    - API vs Spoke.
    - There&#39;s a number of continuous delivery tools that can help you automatically manage release versioning, different strategies for rolling deployments. This author is not very familiar with this piece, so won&#39;t be too prescriptive.
