---
layout: post
title:  "Data loss: and now what?"
smmry: I immediately performed a deploy of what should be the stable version and it starts working again but there is no content to be displayed. I had lost everything.
img: /assets/img/data_loss/this_is_fine.jpg
date:   2020-06-04 02:00:00 -0300
comments: true
disqus_identifier: comments_for_data_loss_and_now_what
category: [serverless, data, dynamodb, infrastructure, postmortem]
---

<a target="_blank" href="https://quarantinenotes.com">
  <img style="width:500px" src="{{site.url}}/assets/img/data_loss/this_is_fine.jpg"/>
</a>

On March 29th I wrote about [Quarantine Notes](https://quarantinenotes.com) on a [post](https://www.grott.me/programming/python/flask/serverless/dynamodb/vuejs/quarantine/covid19/2020/03/29/quarantine-notes.html). I was really excited about the project, it had been really fun to work on and I learned a lot on the process.

A friend of mine kept pushing me to share it on the web so people could see and give their opinions. Besides the joy of bringing this project to done, I never had high expectations about it but I decided to give it a shot and [shared it on Twitter](https://twitter.com/lelogrott/status/1244405642904907782?s=20), [Hacker News](https://news.ycombinator.com/item?id=22728460) and [Reddit](https://www.reddit.com/r/programming/comments/frfws8/wanted_to_learn_vuejs_built_a_100_serverless_app/), the results were, well, unexpected.

Quarantine Notes got roughly 400 access per day, and in a few weeks we had thousands of notes there for people to read and [reply](https://twitter.com/lelogrott/status/1246902333470097408?s=20), if they wanted to. It was... satisfying.

<div align="center">
  <img style="width:300px" src="{{site.url}}/assets/img/data_loss/brent_rambo.gif"/>
  <br/><small>Hm... Yep, all good!</small><br/><br/>
</div>

Everything was done, people were adding notes, interacting through replies and so on.  There’s still a [TO DO list](https://github.com/lelogrott/quarantinenotes/blob/master/README.md) of small enhancements to be added, but things were fine and I started working on another project.


This new project of mine will also rely on AWS services using Lambda and DynamoDB through Serverless. So I got my notes from the previous project and started the new repository replicating the same steps I had done once already. I had deployed my new Serverless Flask API and it worked, awesome! Next morning, Quarantine Notes was broken, it couldn't reach my API due to CORS policy, weird. I immediately performed a deploy of what should be the stable version and it starts working again but there aren’t notes to be displayed. I had lost everything.

<div align="center">
  <img style="width:300px" src="{{site.url}}/assets/img/data_loss/ron.gif"/>
  <br/><small>What I wanted to do.</small><br/><br/>
</div>

After trying to understand what happened and trying to retrieve the lost data, I figured that it might be worth to talk/write about this incident in something like a postmortem: identifying the cause of the issue, how can we fix it and what actions can we take to prevent this from happening in the future either on this project or any other one.

### What Actually Happened

The Serverless framework does an amazing job making it really easy and simple for you to deploy your project to AWS but one should understand how it works in order to avoid this kind of situation. [Serverless uses AWS CloudFormation](https://www.serverless.com/framework/docs/providers/aws/guide/deploying#how-it-works) to automate the provisioning of resources on the AWS environment, all requirements and details are specified through a template that is created from the serverless.yml file.

So basically, the serverless.yml file we create defines a [Service](https://www.serverless.com/framework/docs/providers/aws/guide/intro/#services) that needs to have its resources created/updated on AWS, and to do so it uses CloudFormation.

Long story short, when working on my new project I followed the same steps I did before when creating [Quarantine Notes](https://quarantinenotes.com) and ended up deploying a service with the same name but not the same stack. This caused the already existing project to be UPDATED and have its previous Resources (tables and Lambda functions) deleted.

Once I noticed it I immediately deployed the correct project again, what created deleted Resources once again. Data couldn’t be restored.

<div align="center">
  <img style="width:300px" src="{{site.url}}/assets/img/data_loss/thanks.gif"/>
  <br/><small>AAAAAAAA</small><br/><br/>
</div>

### Action

One word action: <b>BACKUP</b>.

Back in 2017, Amazon announced [Dynamo DB Backups](https://aws.amazon.com/blogs/aws/new-for-amazon-dynamodb-global-tables-and-on-demand-backup/) and later on ways to [schedule them](https://aws.amazon.com/blogs/database/a-serverless-solution-to-schedule-your-amazon-dynamodb-on-demand-backup/). Didn’t take long for Serverless to provide us with a pre-build solution for it.

Apparently you can [Automate your DynamoDB Backups with Serverless in less than 5 minutes](https://www.serverless.com/blog/automatic-dynamodb-backups-serverless/), I have tried and if you leave the deploy time out of the equation you can automate your backups in seconds.

Their approach on how to automate this process is really safe, it is a separate service on AWS that requires a different role with dynamodb:ListTables and dynamodb:CreatBackup actions.
You can set up a custom retention period for your tables and adjust backup rate as needed. This service can handle multiple tables, so if you create a new DynamoDB table in any other project, just be sure to add the table’s name to your backup service and you should be good to go.

_So, all done. Right?_ __- Wrong.__

After I realized I had lost all data, I went after logs on CloudWatch and I couldn’t be more frustrated.
The logs didn’t contain ANY useful information, the data from POST requests weren’t there because they have to be explicitly printed to STDOUT and, of course, I wasn’t doing that either. That leads me to

### Action Round 2

<div align="center">
  <img style="width:300px" src="{{site.url}}/assets/img/data_loss/jim.gif"/>
  <br/><small>Last mile sprint. GOGOGO</small><br/><br/>
</div>

It’s better to be safe than sorry. I’ve decided to print every JSON formatted entry right after inserting it into the table, that way if for some reason backup tables don’t work I’ll be able to do some extra work and retrieve data from logs.

That's it, I know this was a long post but I feel that it was needed. Answering the question in the title: now we analyze it, understand what happened and why it happened. Only doing that we'll be able to work on prevention and data recovering methods.

Stay safe!

