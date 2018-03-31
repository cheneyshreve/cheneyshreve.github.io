---
layout: post
title: Blocipedia
thumbnail-path: "img/about_wikis.png"
short-description: Blocipedia is a production quality SaaS app that allows users to create their own wikis.

---
## Explanation
Blocipedia is an application that allows users to create public and private Markdown-based wikis. Blocipedia was my second rails app, and first foray into online subscriptions. Users can upgrade from a standard to premium account to gain access to private wikis, and add or remove wiki collaborators. Once a premium user adds a collaborator, that individual has access to the wiki regardless of their subscription status.

## Problem
Time is precious, so this app makes use of some key ruby gems, namely, Devise, to support user authentication and authorization, Stripe, a third-party vendor to handle processing payments and managing subscriptions, and RedCarpet, to enable the use of Markdown language.

Here's a snapshot of the wikis page, a user can see private wikis if they have subscribed, or if they are added to the wiki by a premium subscriber:

{:.center}
![]({{ site.baseurl }}/img/topics.png)

## Solution
This app was 'model heavy, controller light'. It was relatively straight forward to define the ```has_many``` and ```belongs_to``` relationships between the User and Wiki's models, respectively. Defining the policy scope was a bit more involved, as I first needed to define user roles using ```enum``` in the user model, then set the scope in the wiki class:

```
class User < ApplicationRecord
  has_many :wikis, dependent: :destroy
  has_many :collaborates, dependent: :destroy

  enum role: [:standard, :premium, :admin]
  after_initialize :init

  def init
    self.role ||= :standard
  end
  ...
end
```

```
class Wiki < ApplicationRecord
  belongs_to :user
  has_many :collaborates, dependent: :destroy
  has_many :users, through: :collaborates

  scope :visible_to, -> (user) { user.role == 'admin' || user.role == 'premium' ? all : where(private: false) }

end

```



## Results & Conclusions
Checkout my Blocipedia repository on [Github](https://github.com/cheneyshreve/blocipedia), clone the repository it and give it a test-run yourself!

Or checkout the live version of the app on [Heroku](https://immense-ocean-13499.herokuapp.com)
