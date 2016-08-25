---
layout: post
published: true
date: '2016-08-25 21:38 +0200'
modified: '2016-08-25 21:38 +0200'
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: true
title: 'Gotcha maven jgitflow:release-finish with checkout conflict'
tags:
  - maven
---
## Gotcha maven jgitflow:release-finish with checkout conflict
Every once in a while you encounter an error message that makes no sense and puts you on the wrong track to find a solution. This time it occurred in a Jenkins Maven job that released my project before deploying to production. 

Like with all other projects, the Jenkins job executed a simple maven command: 

```
mvn jgitflow:release-start jgitflow:release-finish
```

Unfortunately we got this error: 

```
Error finishing release: org.eclipse.jgit.api.errors.CheckoutConflictException: Checkout conflict with files:
[ERROR] pom.xml
```

Since we are not the only ones using the maven jgitflow plugin, I tried to find the answer on Google. We were not the only one with this problem, but no one with a solution. So I needed to dig deeper. After checking all differences between the maven pom.xml's and de Jenkins jobs, it suddenly hit me. Could it be that the the branch it is merging does not exist? And yes, that was it. Somehow this project did not have a master branch (anymore), which caused the merge from master to develop to fail. 

Sometimes a solution can be very simple. I created a master branch from develop and ran the job again with success this time. Problem solved :)
