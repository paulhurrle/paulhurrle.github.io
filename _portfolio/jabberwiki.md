---
layout: post
title: Jabberwiki
thumbnail-path: "/img/jabberwiki_home.png"
short-description: Jabberwiki is a CRUD application built using Ruby on Rails that allows users to create public and private Markdown-based wikis.

---

{:.center}
![](/img/jabberwiki_home.png)

## Explanation

This Ruby on Rails application provides a hub for community-sourced wikis. This project provides all of the basic CRUD actions,
with features including user authorization with Pundit, authentication powered by Devise, e-payments using the Stripe API, and parsing
Markdown with Redcarpet.

## Problem

This application enables users to create wikis for any purpose, share and collaborate on other wikis, as well as upgrade
or downgrade their account depending on whether they would like additional features such as keeping wikis private. Ensuring only
authorized users may access certain wikis and providing a secure mechanism for processing credit card payments are of particular
importance in this app.

## Solution

After initializing the rails application, the first step was to integrate the Devise gem and related views for user authentication.
With the gem installed, Devise was then used to dynamically generate the User model `rails g devise user` and accompanying views and
routes. Updating Application Controller with a `before_action :authenticate_user!` callback ensures users must be authenticated before
accessing any other content.

Requiring new users to confirm via email before completing the registration process was achieved by calling Devise's `confirmable` method
from the User model and configuring Action Mailer to generate an email to the address provided by the new user at sign-up. A few lines of
embedded html content was added to the navigation bar atop `application.html.erb` to prompt an unauthenticated user to sign in, or to
provide custom links for a user who has signed in.

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_signedin2.png" alt='Signed In HTML'>

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_signedin1.png" alt='Signed In View'>

Setting up the CRUD actions associated with new and existing wikis required the addition of a wiki model with appropriate attributes:
title, body, and private (which defaults to false). Since wikis belong to users, the appropriate relationship was specified in the wikis
model using a `user:references` attribute. Finally, a corresponding Wikis Controller was added with methods for handling each CRUD action.

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_wiki_model.png" alt='Wiki Model'>

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_wiki_controller.png" alt='Wikis Controller'>

The Pundit gem was installed to allow user authorization into one of three roles: standard (default), premium (paid upgrade), and admin.
Since users must be authorized prior to editing a public wiki, a method was added to `application_policy.rb` to check whether a user is
present (user.present?).

The Faker gem was chosen for this application to seed the database in development with users and wikis.

One of the more challenging parts of this application was creating the user flow for upgrading one's account from standard to premium.
This process would involve changing the user's role and charging the user's credit card the "premium membership fee". The Stripe API is
a convenient and safe way to verify a card is valid and route the money, with only a few configurations needed in the application:

1. Sign up for a Stripe account where you will be assigned unique API keys.
2. Configure Stripe in a separate initialization file with the aforementioned keys.
3. Create a Charges Controller with methods for creating a charge (which also upgrades the user's role if successful) and downgrading users.
4. Create the corresponding views and routes for charges.

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_stripe.png" alt='Stripe'>

Premium and admin users need the ability to assign new wikis as private, or change existing wikis to private. Similarly, only authorized
users should be able to see private wikis on `index.html.erb`. This functionality required the use of form partials that rendered this content only if a user of the specified role is verified.

Giving users the ability to edit the way wikis appear by incorporating Markdown syntax was executed through the (Redcarpet)[https://github.com/vmg/redcarpet] gem.

One of the unique features of community-sourced content is the ability to collaborate on each other's work. Adding collaborators in
Jabberwiki started with creating a Collaborator model with the appropriate relationships to users and wikis, then using policy scoping
to render content based on the current user's role and collaborator status, and information about the wiki itself.

{:.center}
<img style="width: 600px;" src="/img/jabberwiki_wiki_policy.png" alt='Wiki Policy'>

## Conclusion

This project expands upon the basics of Rails CRUD applications by integrating features such as authentication and authorization, secure payment processing, parsing Markdown. The inclusion of gems like Pundit, Devise, Stripe, and Redcarpet streamline much of these processes, allowing you as the developer to focus more on routing user requests and policy scoping to control viewable content.
