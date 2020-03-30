---
layout: post
title:  "Quarantine Notes"
smmry: Weird times, I know. People all around the world are in quarantine (myself included) due to the outbreak of the pandemic desease COVID-19. This post is about what I did in the course of the last week in my spare time.
img: /assets/img/quarantine_notes/site_header.png
date:   2020-03-29 13:30:00 -0300
comments: true
disqus_identifier: comments_for_quaratine_notes
category: [programming, python, flask, serverless, dynamodb, vuejs, quarantine, covid19]
---

<a target="_blank" href="https://quarantinenotes.com">
  <img style="width:500px" src="{{site.url}}/assets/img/quarantine_notes/site_header.png"/>
</a>

Weird times, I know. People all around the world are in quarantine (myself included) due to the outbreak of the pandemic desease COVID-19. This post is about what I did in the course of the last week in my spare time.

As I may have made clear in previous posts, coding and actually developing/building something is for me, the most effective way of learning something new. I've been wanting to learn Vue.js for a while now, but the ideas that I had written down on my personal projects's list seemed too complex to start with. That's when I noticed that due to the pandemic situation we're living right now, people are talking only about one thing: their quarantined life.

Since that's what I see in all my social networks, why not to give people a 100% anonymous wall to write anything they want about their quarantine? Yes, I agree. They could do the same in any other existent website. But hey, let's not forget the point here, this is not about the goal as it is about the process to get there. :)

<div align="center">
  <img style="width:300px" src="{{site.url}}/assets/img/quarantine_notes/learning.gif"/>
  <br/><small>Learn, learn, learn!</small><br/><br/>
</div>

So, at this point I was convinced that a wall with some posts and a simple form was a great UI for me to start learning Vue, but then what about the backend? I didn't want to bring up a server to run this simple API that my app would be reaching out to, so that's when things got a lot more exciting.

Let's build up a REST API with Python using Flask, Serverless framework to deploy and execute it on AWS Lambda and of course, having DynamoDB as our NoSQL serverless database. That may sound too complicated, but believe me, all people involved in those projects made it easy for us to do something like that by writing incredibly complete documentation.

Breaking it down to what I've used on this project, we have:

  - [VueMail – Dark – VueJS Theme](https://www.themesine.com/downloads/vuemail-dark-vuejs-theme/) by ThemeSINE
  - [Vuetify](https://vuetifyjs.com/)
  - [Official Vue website](https://vuejs.org/)
  - [Build a Python REST API with Serverless, Lambda, and DynamoDB](https://serverless.com/blog/flask-python-rest-api-serverless-lambda-dynamodb/)
  - [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/)
  - [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

With those tools I was able to build this simple app completely serverless named [Quarantine Notes](https://quarantinenotes.com). Access it, read other people's notes and leave yours :)

You can access the public repository and take a look at the code by clicking the card below:

<a target="_blank" href="https://github.com/lelogrott/quarantinenotes">
  <img style="width:500px" src="{{site.url}}/assets/img/quarantine_notes/repo_card.svg"/>
</a>
