---
layout: post
title: Blocmarks
thumbnail-path: "img/blocmarks_topics.png"
short-description: Blocmarks is a social bookmarking app that allows users to create and manage bookmarks, see other user's bookmarks, and like and unlike bookmarks that they or other users have created.
---

### Key features of Blocmarks:
- Users can create bookmarks through the web app or by sending an email to the application
- Bookmarks are public and visible to other users, but only the creator of the bookmark can edit or delete the bookmark
- Users can like and unlike their own bookmarks, or those of other users
- Thumbnail images of each bookmark replace traditional bookmark folders, providing a better user experience

Visit the live version on [Heroku](https://fierce-peak-50187.herokuapp.com), or checkout my Blocmarks Github repository to download and run the app locally.

### Technical resources:
- Ruby and Ruby-on-Rails
- Mailgun for email confirmation
- Mailgun API to enable users to populate bookmarks via email
- Pundit for authorization
- BitPixel for generating thumbnails of urls

{:.center}
![]({{ site.baseurl }}/img/blocmarks_homepage.png)

### Challenges and Solutions
Writing the models was straightforward for this, mainly requiring a bit of thinking at the start to make sure I planned out what classes were necessary, and what relationships between classes were needed. I ended up with a class for users, topics, bookmarks, and likes. Dividing up classes this way made it easier to compartmentalize different features and tasks.

Writing the Rspecs to utilize TDD for this was very straightforward until the stage where I added pundit for authorization. I'm still investigating the best way to test using pundit. There are several different gems and other resources out there, but there still seems to be a lot of confusion and disagreement on best practices in this area on the web. This is something I will return to and revisit with my mentor here at Bloc because I know how important it is to have solid TDD in your project, not just for your own sanity, but for the overall reliability and scalability of a project moving forward.

Setting up Mailgun and using the Mailgun API was very straightforward and enabled me to add functionality to populate new users and bookmarks by emailing the app, however, I did not implement this in production because I would have needed to purchase a custom domain. In itself, that's very easy, however, the costs start to add up for student projects when you have to keep signing up for various products and services. Nonetheless, learning the process and utilizing the Mailgun "sandbox" for testing was really valuable. Similarly, both BitPixel and the Enably gem provide high quality image thumbnails for URLs, however, Enably requires yet another subscription. BitPixel is very straight forward to use, and free!
