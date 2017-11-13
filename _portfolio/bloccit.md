---
layout: post
title: Bloccit
thumbnail-path: "/img/bloccit_home.png"
short-description: Bloccit is a messaging application where users create, vote on, and favorite posts.

---

{:.center}
<img style="width: 600px;" src="/img/bloccit_home.png" alt='Bloccit'>

## Explanation

In my first endeavor into backend development, I sought to apply principles of MVC architecture and test-driven development to create a CRUD application called Bloccit.  

## Problem

Like the popular Reddit application, Bloccit's purpose is to provide users the ability to post, vote on, share, and save links and comments. I would need to use the Rails framework and associated MVC architecture to assign the responsibilities for these features into the respective models, views, and controllers. Additionally, the RSpec testing framework would be required to ensure data integrity, and users would need to be authenticated and authorized with role-specific permissions. Finally, the application would need to be designed using the Bootstrap CSS framework.

## Solution

To begin, I created the development and production databases and installed the Ruby gems I would be using throughout the project. I then generated the initial controller and corresponding views and routes dynamically using the `rails generate` command, a process that I would later replicate each time a new controller was needed.

The next step was to prepare my application for testing using the canonical framework RSpec. I installed the RSpec gem, created spec files for my controllers, and wrote several preliminary tests to validate that HTTP requests behaved as expected. Additionally, the database was seeded with random data to enable efficient testing and simulation of user behavior in the development environment.

{:.center}
<img style="width: 600px;" src="/img/bloccit_spec.png" alt='Bloccit RSpec Test'>

I then switched gears to focus on styling, opting to install the Bootstrap CSS framework to take advantage of pre-built layouts, forms, buttons, icons, and other features that would be ideal for my application.

Moving back to building the application logic, I created `Post` and `Comment` models - once again using the rails generator to dynamically create models along with the specified attributes in addition to the spec files for TDD. Since comments and posts are associated, corresponding logic was added to create the relation within the database models.

To enable the create, read, update, and delete (CRUD) actions, controllers were generated and resources added to the `config/routes.rb` file, which specified the routes for each controller action. Then TDD was used to build the additional logic within the controllers to support the CRUD actions. Finally, the associated views were edited accordingly.

{:.center}
<img style="width: 600px;" src="/img/bloccit_CRUD.png" alt='Bloccit CRUD'>

User authentication required creating a User model and integrating Active Record callbacks to validate that attributes such as name, email, and password meet specified parameters. The next step was to create a `User` controller with corresponding CRUD methods, and update the view with user sign-in and sign-up interfaces. User sessions were established using a `Sessions` controller, and this combined with adding a role attribute to the User model and callbacks enabled user authorization for specified CRUD actions.

Two of the next features of the Bloccit application were voting and favorites. These required creating `Vote` and `Favorite` models, associating them with the `Post` and `User` models, and testing and implementing the CRUD actions within the models. Further updates to the view and routes were needed to create the desired UI and UX behaviors.

{:.center}
<img style="width: 600px;" src="/img/bloccit_favorite.png" alt='Bloccit Favorite'>

Adding notifications when a user comments on a favorited post was an interesting feature that required incorporating the SendGrid add-on and corresponding mailer files and methods to generate templated emails.

A final touch to the application was to add personalized user images via the popular Gravatar service. Adding scoping to the models controlled the content that each user sees from the UI.

{:.center}
<img style="width: 600px;" src="/img/bloccit_user_profile.png" alt='Bloccit User Profile'>

## Results

As previously stated, TDD was deployed throughout this project, along with the Red-Green-Refactor methodology to produce valid, streamlined code.

## Conclusion

This project was a successful entry into using the Rails framework, which not only consisted of applying MVC principles to organize the way different app parts communicate with each other and how data persists to the database, but also integrating Ruby gems to enhance the capabilities of the app. Perhaps most importantly, I learned how using TDD allows developers to concentrate on writing code for a specific purpose, and to ensure results are valid before moving to the next stage.
