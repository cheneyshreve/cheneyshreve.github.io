---
layout: post
title: Custom Alexa Skill
thumbnail-path: "img/spain-logo-large.png"
short-description: Just a little Taste of Spain is a custom skill for Alexa that provides guided cooking instructions and utilizes Amazon's List APIs to send custom lists to the user's home Alexa app.
---

I've been working on developing custom skills for Alexa recently, to familiarize myself with Amazon's cloud computing resources and gain experience programming for voice. One of my skills, "Just a Little Taste of Spain", was just certified and will be freely available on Amazon.com shortly. Some key features of the skill are that it allows users to progress at different speeds through the program depending on their cooking proficiency and familiarity with the recipe, database persistence stores user progress so they can repeat or pause steps, and they can enable permissions within the Alexa app to have the skill send them a customized shopping list.

Check out the live version of the skill on [Amazon](https://www.amazon.com/Cheney-Shreve-Little-Taste-Spain/dp/B07BDCFBXN/ref=sr_1_1?s=digital-skills&ie=UTF8&qid=1521938366&sr=1-1&keywords=just+a+little+taste+of+spain) here.


Writing a custom skill for Alexa was great practice working with APIs, programming in JavaScript, and working with Nodejs. As I blogged about recently on [Medium](https://medium.com/why-i-developed-badass-women-facts-for-alexa/building-a-custom-alexa-skill-that-utilizes-the-list-apis-post-success-3a38f824e83e), gaining familiarity with the Amazon technical documentation, especially guidance using their various APIs, took some time and practice. It requires a bit of a shift in thinking because sessions become more complex, that is, instead of thinking of a session as a user signing into a site, sessions for Alexa pertain to individual intents, or vocal interactions with the user. It was also a great learning experience because of the relative lack of documentation and discourse on Alexa skills on sites such as StackOverflow. The Amazon Developer's Forum is a potentially good resource, but I found I didn't get a response to my questions there and there were more users with similar questions than there were answers. I don't think this is a "ding" against Amazon, it's just the nature of new technology--the more developers we have working on skills, the more the discourse will grow and eventually it will have more discussion on sites like StackOverflow.

Here's a code snippet for the List APIs, which was a recurring question (but unanswered) in the dev forums, with more discussion and context in the Medium blog linked above. Overall, I'm glad to have worked with Alexa. It forced me to be more resourceful in problem-solving, to rely on failures and feedback when submitting the skill to Amazon, which eventually led to a better understanding of the skill.

Within the CreateListIntent, you first need an accessToken, or permission for your skill to use APIs. Next, you need constrain the call to the API only if the user has granted permission and the voice interaction session is complete.

```
'CreateListIntent': function () {

        var filledSlots = delegateSlotCollection.call(this);
        var slotName = "Recipe";
        var slotValue = isSlotValid(this.event.request, slotName);
        var answer = this.event.request.intent.slots.Recipe.value;

        if (slotValue){
          current_response = formatName(answer);
        }

        var accessToken = this.event.context.System.apiAccessToken;

        if(!this.event.session.permissions && !this.event.session.user.permissions) {
           var say = 'You need to enable permissions for accessing lists within the Alexa app before I can send you a list. '
           var speechOutput = say + " What would you like to do next, start cooking, or hear the recipe ingredients? ";
           var reprompt = " What would you like to do next, start cooking, or hear the recipe ingredients? ";
           this.response.speak(speechOutput).listen(reprompt);
           this.emit(':responseReady');
           return;
        }

        if (this.event.request.dialogState === "COMPLETED")  {
         var that = this;

          createList(this.event.session, (callback) => {
              console.log("callback:", callback);

              var say;
              let speechOutput;

              if (callback == 201) {
                say = 'Okay, your list was sent. '
                speechOutput = say + " What would you like to do next, start cooking, or hear the recipe ingredients? ";
                that.response.speak(speechOutput).listen(say);
                that.emit(':responseReady');

              } else if (callback == 409) {

                say = 'You already have a list with that name. You will need to archive the old one within the Alexa app. '+
                'What would you like to do next, you can say start cooking, or hear the recipe ingredients? ';
                var reprompt = " What would you like to do next, start cooking, or hear the recipe ingredients? ";
                that.response.speak(say).listen(reprompt);
                that.emit(':responseReady');

              } else {
                that.emit('Unhandled');
              }
          });
       }
      },

```

You also need to utilize the GetListsMetadata API to retrieve the listId for the specific user. Following that, I've added code that allows the user to POST a custom list, in this case a recipe, to the user's Alexa app.

```
const getListsMetadata = function(session, callback) {
    console.log("starting getListsMetadata");

    if(!session.user.permissions) {
        console.log("permissions are not defined");
        callback(null);
        return;
    }

    var consent_token = session.user.permissions.consentToken;

    var options = {
        host: api_url,
        port: api_port,
        path: '/v2/householdlists/',
        method: 'GET',
        headers: {
            'Authorization': 'Bearer ' + consent_token,
            'Content-Type': 'application/json'
        }
    }

    var req = https.request(options, (res) => {
        console.log('STATUS getListsMetadata: ', res.statusCode);
        console.log('HEADERS getListsMetadata: ', JSON.stringify(res.headers));

        if(res.statusCode === 403) {
            console.log("permissions are not granted");
            callback(null);
            return;
        }

        var body = [];
        res.on('data', function(chunk) {
            body.push(chunk);
        }).on('end', function() {
            body = Buffer.concat(body).toString();
            callback(body);
        });

        res.on('error', (e) => {
            console.log(`Problem with request: ${e.message}`);
        });
    }).end();

    console.log("ending getListsMetadata");
};


function createList(session, callback) {

    var consent_token = session.user.permissions.consentToken;
    let options = {
      host: api_url,
      port: api_port,
      listId: list_id,
      path:  '/v2/householdlists/',
      state : 'active',
      version : 1,
      method: 'POST',
      headers: {
          'Authorization': 'Bearer ' + consent_token,
          'Content-Type': 'application/json'
      }
     };

    let messageObject;

     if (current_response == 'tapas') {
        messageObject = {
          name: "tapas: 1 baguette, 4 oz. goat\'s cheese or brie cheese, 1 Tbsp honey or agave, 1 Tbsp olive oil, 0.5 c. chopped walnuts, salt to taste",
          state: 'active',
          version: 1
      }
     } else if (current_response == 'paella') {
        messageObject = {
          name: "paella: 2 c. rice, 1 qt. vegetable or chicken broth, 4 Tbsp olive oil, 15 threads saffron, 2 medium tomatoes, 0.5 pound shrimp, 1 onion, 1 shallot, salt and pepper to taste ",
          state: 'active',
          version: 1
        }
     } else if (current_response == 'tortilla') {
        messageObject = {
          name: "tortilla espanola: 6 eggs, 4 medium potatoes, 2 tsp salt, 1 onion, 1 shallot, 0.5 c. green beans, pepper to taste",
          state: 'active',
          version: 1
        }
     } else if (current_response == 'gazpacho'){
        messageObject = {
          name: "gazpacho: 4 medium tomatoes or 20-30 cherry tomatoes, 1 cucumber, 2 Tbsp olive oil, 1 onion, 1 lime, 1 bunch cilantro, 1 tsp basalmic vinegar, salt and pepper to taste",
          state: 'active',
          version: 1
        }
     } else {
        callback(null);
        this.emit('Unhandled');
        return;
     }

     let req = https.request(options, (res) => {
        if(res.statusCode === 403) {
            console.log("permissions are not granted");
            callback(null);
            return;
        } else {
            callback(res.statusCode);
            console.log(res.statusCode);
        }

        var body = [];
        res.on('data', function(chunk) {
            body.push(chunk);
        }).on('end', function() {
            body = Buffer.concat(body).toString();
            console.log(JSON.stringify(body));
            callback(body);
        });

        res.on('error', (e) => {
            console.log(`Problem with request: ${e.message}`);
        });
    });

    req.write(JSON.stringify(messageObject));
    req.end();
}

```
