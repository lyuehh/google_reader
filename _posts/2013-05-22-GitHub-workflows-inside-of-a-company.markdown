---
layout: post
title:  "GitHub workflows inside of a company"
date:   2013-05-22 00:00:20
author: Nicholas C. Zakas
categories: program
---

## GitHub workflows inside of a company
### by Nicholas C. Zakas
### at 2013-05-22 00:00:20
### original <http://feedproxy.google.com/~r/nczonline/~3/wl4tTija-Cs/>

<p>Recently I asked engineers to share their experiences working with GitHub at companies. I’ve always used GitHub for open source projects, but I was interested in learning more about using it professionally and how one’s development workflow might change given all of GitHub’s capabilities. I set up a gist<sup>[1]</sup> so people could leave the answers to my questions and got some great responses. The information comes from companies such as Yammer, BBC News, Flickr, ZenDesk, Simple, and more. This is an overview of the responses I received plus some detail from Scott Chacon’s post on Git Flow at GitHub<sup>[2]</sup>.</p>
<h2>Basic setup</h2>
<p>Everyone has at least one GitHub organization under which the official repositories live. Some have more than one organization, each representing a different aspect of the business, however all official repositories are owned by an organization. I suspect this would be the case as it would be horribly awkward to have an important repository owned by a user who may or may not be at the company next year. Also, using an organizational owner for these repositories allows better visibility as to what’s going on with official projects just by looking at the organization.</p>
<p>Several people mentioned that no one is barred from creating their own repositories on GitHub for side projects or other purposes. Creating repositories for company-related work is generally encouraged. If a side project becomes important enough, it can be promoted to an organizational repository.</p>
<h2>Developer setup</h2>
<p>Companies took a couple of different approaches to submitting code:</p>
<ul>
<li>Most indicated that developers clone the organization repository for their product and then work on feature branches within that repository. Changes are pushed to a remote feature branch for review and safe-keeping.</li>
<li>Some indicated that each developer forks the organization repository and does the work there until it’s ready for merging into the organization repository.</li>
<li>A couple indicated that they started out with forks and then switched to feature branches on the organization repository due to better transparency and easier workflow.</li>
</ul>
<p>The general trend is in the direction of feature branches on the organization repository. Since you can send pull requests from one branch to another, you don’t lose the review mechanism.</p>
<h2>Submitting code</h2>
<p>In the open source world, external contributors submit pull requests when they want to contribute while the maintainers of the project commit directly to the repository. In the corporate world, where everyone may logically be a maintainer for the repository, does it makes sense to have developers send pull requests? The responses were:</p>
<ul>
<li><span style="line-height:13px">Some required pull requests for all changes.</span></li>
<li>Some required pull requests only for changes outside of their responsibility area (i.e., making a change to another team’s repo). Other changes can be submitted directly to the organization repository.</li>
<li>Some left this up to the developer’s discretion. The developer should know the amount of risk associated with the change and whether or not another set of eyes is useful. The option to submit directly to the repository is always there.</li>
</ul>
<p>The responsibility for merging in pull requests varied across the responses. Some required the team leads to do the merging, others allowed anyone to do the merging.</p>
<p>Interestingly, some indicated that they start a pull request as soon as a new feature branch is created in order to track work and provide better visibility. That way, there can be a running dialog about the work being done in that branch instead of temporary one at the time of work completion.</p>
<h2>Preparing code for submission</h2>
<p>A secondary part of this process is how the code must be prepared before being merged in. The accepted practice of squashing commits and rebasing still  remains common across the board though the benefits aren’t clear to everyone. Of those who responded:</p>
<ul>
<li><span style="line-height:13px">Some required a squash and rebase before a pull request can be merged in.</span></li>
<li>Some will merge in a pull request regardless of the makeup of commits.</li>
<li>Some care about keeping a strict, non-branching history while others do not.</li>
</ul>
<p>It’s hard to outline any consistent trends in this regard. Whether or not you squash, rebase, or merge is very much a team-specific decision (not an organization-specific one).</p>
<h2>What about git-flow?</h2>
<p>I didn’t ask this question specifically, but it came up enough. Git-flow[3] is a process for managing changes in Git that was created by Vincent Driessen and accompanied by some Git extensions[4] for managing that flow. The general idea behind git-flow is to have several separate branches that always exist, each for a different purpose: master, develop, feature, release, and hotfix. The process of feature or bug development flows from one branch into another before it’s finally released.</p>
<p>Some of the respondents indicated that they use git-flow in general. Some started out with git-flow and moved away from it. The primary reason for moving away is that the git-flow process is hard to deal with in a continuous (or near-continuous) deployment model. The general feeling is that git-flow works well for products in a more traditional release model, where releases are done once every few weeks, but that this process breaks down considerably when you’re releasing once a day or more.</p>
<h2>Conclusion</h2>
<p>A lot of people shared a lot of very interesting anecdotes about using GitHub at their companies. As I expected, there’s no one accepted way that people are using GitHub for this purpose. What’s interesting is the range of ways people have chosen to adapt what is essentially an open source workflow for enterprise use. Since GitHub also has GitHub Enterprise, I’m certain that this trend will continue. It will be interesting to see if the feedback from GitHub Enterprise and corporate needs will end up changing the public-facing GitHub in any way.</p>
<p>I’m interested in doing more research about how Git and GitHub in particular are used inside of companies. I’ve yet to see any good research done on whether or not squashing and rebasing is important in the long run, and I think that would be great to figure out. Please feel free to share your experiences in the comments.</p>
<h2>References</h2>
<ol>
<li><a href="https://gist.github.com/nzakas/5511916">How do you use GitHub at your company?</a> (GitHub Gist)</li>
<li><a href="http://scottchacon.com/2011/08/31/github-flow.html">GitHub flow</a> by Scott Chacon (Scott Chacon’s Blog)</li>
<li><a href="http://nvie.com/posts/a-successful-git-branching-model/">A successful Git branching model</a> by Vincent Driessen (nvie.com)</li>
<li><a href="https://github.com/nvie/gitflow">git-flow Git Extensions</a> (GitHub)</li>
</ol>
<div>
<a href="http://feeds.feedburner.com/~ff/nczonline?a=wl4tTija-Cs:DmB7c1GyKX8:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/nczonline?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=wl4tTija-Cs:DmB7c1GyKX8:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/nczonline?i=wl4tTija-Cs:DmB7c1GyKX8:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=wl4tTija-Cs:DmB7c1GyKX8:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/nczonline?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=wl4tTija-Cs:DmB7c1GyKX8:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/nczonline?i=wl4tTija-Cs:DmB7c1GyKX8:F7zBnMyn0Lo" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/nczonline/~4/wl4tTija-Cs" height="1" width="1">