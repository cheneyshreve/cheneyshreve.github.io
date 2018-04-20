---
layout: post
title: Blocmetrics
thumbnail-path: "img/blocmetrics_front.png"
short-description: Blocmetrics is an analytics service to track events on websites made with Ruby-on-Rails. Key features include a server-side API, a client-side JavaScript code snippet that allows users to track events in their applications, and a web accessible rails app that displays visualizations.
---

Blocmetrics is an analytics service to track events on websites that I recently completed as a part of the backend foundations training at Bloc. Check out the live version of the app on [Heroku](https://stark-forest-69447.herokuapp.com/) here.

### Key features of Blocmetrics include:
- A server-side API that captures and saves events to a database built with Rails 5.1.5
- A client-side JavaScript snippet that allows users to track events on their website
- A web accessible rails application that displays the captured event data for a user
- Graphs of event data displayed using Chartkick and Groupdate download option

{:.center}
![]({{ site.baseurl }}/img/blocmetrics_graph.png)

This JavaScript code snippet enables users to track events in their applications:

```
var blocmetrics = {};

blocmetrics.report = function(eventName){

  var event = { event: { name: eventName }};
  var request = new XMLHttpRequest();
  request.open("POST", "https://stark-forest-69447.herokuapp.com/api/events", true);
  request.setRequestHeader('Content-Type', 'application/json');
  request.send(JSON.stringify(event));
};
```

Example usage:
```
window.onload = function() {
  blocmetrics.report('eventName');
}
```

### Key challenges and learning outcomes
I'm familiar with using the devise gem to manage user authentication and authorization from earlier projects, however, one of the things that the Bloc curriculum does not cover is deploying to Heroku, or debugging production related errors. I blogged about developing a good workflow for setting up devise in a Rails app using Gmail, SendGrid, and Heroku on [Medium](https://medium.com/@cheney.shreve/using-devise-with-gmail-sendgrid-rails-and-heroku-tips-to-save-time-debugging-626546118358). Some key points were to test that the environment variables were properly set, I'd used the figaro gem to facilitate this, in the rails console and using Heroku CLI before the first deploy. Similarly, I found it useful to deploy to Heroku every time I'd made any noticeable changes to the database in my app, partly because I was using different databases for test/development (SQLite3) and production (Postgres), and partly because the additional Heroku logs allowed me to catch bugs that weren't caught locally. In earlier apps, I'd waited until the later stages of the project to deploy to Heroku, but I'm finding it's better to do this sooner, and more frequently. I've become much more able to debug Heroku error logs now, and avoid many errors all together.
