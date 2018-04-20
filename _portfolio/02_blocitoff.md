---
layout: post
title: Blocitoff
thumbnail-path: "img/blocitoff.png"
short-description: Blocitoff is a web app that allows users to create self-destructing to-do lists made with Ruby-on-Rails.
---

### Key features of Blocitoff:
- Users can create a free account where they can easily add, visualize and manage to-do's
- Lists automatically delete after 7 days and display days remaining before deletion
- Profile page displays a summary of user activities

Visit the live version on [Heroku](https://morning-thicket-71470.herokuapp.com).


{:.center}
![]({{ site.baseurl }}/img/blocitoff.png)

### Technical resources:
- Ruby and Ruby-on-Rails
- Devise for user authentication
- AJAX request for deleting to-do items without reloading page
- Custom rake task to delete items older than 7 days

### Example Outcomes
Learning about the Ajax requests was important because it provides a much better user experience, without it, the user would have to reload the page to be able to see that the to-item had been removed. This was accomplished using simple respond_to helper in the controller to enable the use of HTML and JavaScript formats. Additionally, jQuery was used for selecting relevant elements in the views with Ruby to accomplish the fade out effect of deleted to-do items.

Code snippet in the to-do items controller:

```
def destroy
  @item = current_user.items.find(params[:id])

  if @item.destroy
    flash[:notice] = "To-do Complete!"
  else
    flash[:error] = "Error removing item, please try again."
  end

  respond_to do |format|
    format.html
    format.js
  end

end
```

Code snippet from a to-do item view:

```
<% if @item.destroyed? %>
 $('#item-' +<%= @item.id %>).prepend("<div class='alert alert-notice'><%= flash[:notice] %></div>");
  $("#item-<%= @item.id %>").fadeOut(1300);
<% else %>
  $('.flash').prepend("<div class='alert alert-danger'><button type='button' class='close' data-dismiss='alert'>&times;</button><%= flash.now[:alert] %></div>");
<% end %>

```
Another aspect of this project that was a great learning outcome was insight into custom rake tasks. While I decided not to implementing the custom rake task in my production environment due to cost, it was a valuable learning experience to implement this function locally, and to learn the requirements and resources for automating tasks in Heroku.

The code snippet below is for a simple rake task to update the database and remove to-do items older than 7-days:


```
namespace :todo do
  desc "Delete to-do items older than 7 days"
    task delete_items: :environment do
      Item.where("created_at <= ?", Time.now - 7.days).destroy_all
    end
end
```
