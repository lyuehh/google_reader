---
layout: post
title:  "Deployment History in Engine Yard AppCloud"
date:   2010-12-23 10:00:57
author: Martin Emde
categories: program
---

## Deployment History in Engine Yard AppCloud
### by Martin Emde
### at 2010-12-23 10:00:57
### original <http://www.engineyard.com/blog/2010/deployment-history-in-engine-yard-appcloud/>

<h3>Upgrade To The CLI Deployment System</h3>
Deployment history is only available on the web using the CLI-based deployment method. If you haven't already converted your environment to CLI Deploys, now is the time.

Switching your environments to use the CLI deploy system as the strategy for web deployments only takes a few seconds. There have been no reported issues for this well tested deploy system, and about half of all Engine Yard AppCloud environments are using it.
<h3>Benefits of CLI Deploy</h3>
<ul>
	<li>Proven Bundler support</li>
	<li>Consistent behavior between the command line and web deployments</li>
	<li>Much faster deployments (compared to updating instances every deploy, which is required by the old web deploy system)</li>
	<li>Recorded deployment history</li>
</ul>
To enable CLI Deploy and deployment history on your environments, visit the dashboard and click <strong>More Options</strong>, then <strong>Edit Environment</strong>. Show <em>advanced options</em> and switch the pull-down from "Legacy Web Deploy" to "CLI Deploy". Save the environment and you're ready to go.


Once enabled, you'll see a slightly different user interface. The button on the top shows "Update Instances" and only runs instance configuration without deploying your apps. Application deployment has moved to the Applications tab (makes sense, right?).

<img src="http://www.engineyard.com/blog/?getfile=5564" alt="Deploy from the Applications tab">

To deploy applications from the dashboard, click the Applications tab on your environment. You will see a form with "Migrate?" and "Ref" inputs. The migrate check box and input control the migration command to run (or nothing if unchecked) and the ref will autocomplete with your repository's branch and tag names. Once you click Deploy, you can follow the deployment status by watching the area below the form. This deploy process is exactly the same as using the engineyard CLI gem in your terminal.

<img src="http://www.engineyard.com/blog/?getfile=5563" alt="Deployments work the same as command line deployments">

Most deploys take less than a couple minutes and the "View Log" link will show you the deployment output.

You may notice that each deployment tracks your name. If you're not in our beta program for collaboration, this may seem a bit confusing. All deploys are done by you right? If you work with a team of developers or deal with multiple Engine Yard accounts on a daily basis, <a title="Sign up for collaboration beta access" href="http://docs.engineyard.com/beta/home">signing up for the collaboration beta program</a> will make you very happy.

<del>Deployment tracking of command line deploys from the engineyard gem is coming soon.</del>

EDIT (23 December 2010): Deployment history from CLI deploys is now enabled as of engineyard gem version 1.3.8.<p><a href="http://www.engineyard.com/blog"><img height="98" width="61" title="logo-engineyard" alt="" src="http://www.engineyard.com/blog/?getfile=4050"></a></p>