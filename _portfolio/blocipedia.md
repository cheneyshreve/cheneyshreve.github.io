---
layout: post
title: Blocipedia
thumbnail-path: "img/blocipedia.png"
short-description: Build a production quality SaaS app that allows users to create their own wikis.

---

{:.center}
![]({{ site.baseurl }}/img/blocipedia.png)

## Explanation

Blocipedia is an application that allows users to create public and private Markdown-based wikis. Blocipedia was my second rails app, and first foray into online subscriptions. Users can upgrade from a standard to premium account to gain access to private wikis, and add or remove wiki collaborators. Once a premium user adds a collaborator, that individual has access to the wiki regardless of their subscription status.

## Problem
Time is precious, so this app makes use of some key ruby gems, namely, Devise, to support user authentication and authorization, Stripe, a third-party vendor to handle processing payments and managing subscriptions, and RedCarpet, to enable the use of Markdown language.

As I'd used Devise before with Bloccit, getting that up and running was no problem. Stripe and RedCarpet were similarly straight forward to install and set-up. Setting up privacy controls for wikis and users was a bit more involved, requiring close attention to flow of information throughout the MVC framework. RSpec testing for Test-Driven Development (TDD) was less straightforward, especially with regard to the Devise and Stripe gems, and required setting up mock plans/subscriptions to test against, and user authentication and authorization mock-ups.

## Solution
Writing the RSpecs for this app was more involved compared to my earlier projects. It appears I'm not the only one who finds this challenging, after researching some queries on StackOverflow, I found some ruby gems to help support testing Devise and Stripe, which made the process more straightforward. I found that the gems facilitated the testing process, but I was also able to incorporate my own ideas as well.

The fun part of this project was installing the 'faker' gem to populate the database with fake data. From superheroes to Star Trek quotes, there's plenty of fun to be had with faker! I chose to use faker's Hitch Hiker's Guide to the Galaxy resources.

{:.center}
![]({{ site.baseurl }}/img/blocipedia2.png)

## Results & Conclusions
Checkout my Blocipedia repository on [Github](https://github.com/cheneyshreve/blocipedia), clone the repository it and give it a test-run yourself! Check back soon and I'll post a link to the live version on Heroku!
