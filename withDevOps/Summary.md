**What is DevOps, and what is Dev@Scale (Source Control, CI/CD)?**

&quot;ServiceNow DevOps&quot; automates change management and progress dashboards for apps built anywhere.

&quot;Now Platform CI/CD&quot; helps customers building apps on SN to automate testing and deployment.

![](RackMultipart20200909-4-1p4fojs_html_5d6988addeb7085f.png)

**Qualifying Questions**

When a customer says, &quot;I want DevOps&quot;, or &quot;I have Jenkins&quot;, we need to help them drill down on their specific user needs, because those are very generic buckets. What are they thinking of solving with &quot;DevOps&quot;? What are they using Jenkins to do?

1. Clarify what applications they are building and where.
  1. If it&#39;s off-Platform (e.g. Java app, mobile app, built by other teams in the company), Now Platform CI/CD does not apply. They might want to use ServiceNow DevOps for change management and viewing progress in one place.
  2. If it&#39;s on-Platform, then we need to keep asking questions, but they could very well need both CI/CD and DevOps.
2. Clarify the need in relation to external tools.
  1. If it&#39;s to track and monitor build progress in one place, then they want ServiceNow DevOps for its Insights Dashboard.
  2. If it&#39;s to add approval steps (change management) in an automated fashion, then direct to ServiceNow DevOps.
  3. If it&#39;s to use external tools as the code repository or build automation server to automate testing and deploying of apps being built on the Now Platform, then point to our CI/CD features.
3. Clarify what type of automation they are seeking
  1. Automating change management requests --\&gt; ServiceNow DevOps
  2. Automate mechanics of deploying code into environments (via App Repo) --\&gt; CI/CD

**The Sweet Spot (1+1=3)**

Now Platform CI/CD --\&gt; External Build Tools --\&gt; ServiceNow DevOps

Use Jenkins as the build automation tool for automating testing and deployment of apps on the Now Platform.

Then feed the pipeline data from Jenkins (for any applications, not just Now Platform apps) back to ServiceNow DevOps to automate generation of change request stories and see progress in one Insights Dashboard.