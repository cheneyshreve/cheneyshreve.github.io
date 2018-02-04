---
layout: post
title: Bloccit
thumbnail-path: "img/bloccit.png"
short-description: Bloccit is a web app where people can post, vote on, share and save links and comments.

---
{:.center}
![]({{ site.baseurl }}/img/bloccit.png)

## Explanation
Bloccit, if you couldn’t surmise from the name, draws similarities to Reddit. Boccit is a web app where people can post, vote on, share and save links and comments.

## Problem
Bloccit was my first rails app, and it was a great opportunity to get my feet wet with the Model View Controller (MVC) architecture that underpins rails, as well as getting started with good practices using Test-Driven Development (TDD). Supporting the user activities such as creating, editing, deleting and favoriting posts was an institutive introduction to data manipulation or CRUD (create, read, update, delete).

## Solution
The rails architecture makes supporting these basic user activities straightforward. Additionally, the use of a few ruby gems or libraries made activities like user authentication and styling a breeze. The devise gem was used for user authentication and bootstrap-sass for styling.The icing on the cake was turning Bloccit into my first live app in a production environment, Heroku. Another perk of using Heroku was the use of the SendGrid mail add-on to manage emails.


## Results & Conclusions
Bloccit is fast, easy to navigate, and intuitive, which makes it fun to use. The user overview page gives Bloccit a more personalized feel, keeping track of your personal avatar, and summarizing your favorite posts, comments, and other activities.


{:.center}
![]({{ site.baseurl }}/img/bloccit2.png)


Check out a live version of [Bloccit](https://radiant-meadow-79731.herokuapp.com/) on Heroku.
