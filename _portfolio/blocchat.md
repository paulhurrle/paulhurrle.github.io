---
layout: post
title: BlocChat
thumbnail-path: "/img/blocchat.png"
short-description: BlocChat is an app that leverages AngularJS and Firebase to create a personalized chat room experience.

---

{:.center}
![]({{ site.baseurl }}/img/blocchat.png)

## Explanation

This was my second web development project using AngularJS and the first using Firebase to manage information in a database. Moreover, it was my first solo project which would surely test my ability to apply AngularJS to solve real-world problems and flex my design muscles.

## Problem

Starting with not much more than a few wiremaps and a set of project requirements, I needed to build everything from the ground up. This included creating a Firebase database and learning Firebase JavaScript and AngularFire APIs. I would also need to create all of the script files - controllers and services - needed to enable seamless communication between the view, model, and database. Following that, I would need to complete several user stories related to retrieving and creating chat rooms or messages, setting a username using a cookie, and integrating user and other details dynamically into the view. And last but not least, I would have to style the app to reflect UI/UX best practices and, of course, look visually stunning!

## Solution

After choosing a rough design concept for the project and initializing a Firebase database, I created a factory to store all AngularFire APIs needed to query the database to display a room or to add one, and a controller to manage requests to and from the view. A main html template was then created to organize the content to inject into the view when requested. I associated the main controller with the main html template using a state provider which ensured all properties and methods within the controller's scope were available to that state:

{% highlight javascript %}
$stateProvider
    .state('home', {
        url: '/',
        controller: 'HomeCtrl as home',
        templateUrl: '/templates/home.html'
        });
{% endhighlight %}

I added a modal using the UI Bootstrap $uimodal service to allow users to add new rooms, which involved creating an additional template and controller for adding new room objects to the Firebase database.

{% highlight javascript %}
function RoomModalCtrl(Room, $uibModalInstance) {
        this.input = '';
        this.cancel = function () {
            $uibModalInstance.close(false);
        };
        this.submit = function () {
            Room.add(this.input);
            $uibModalInstance.close(this.input);
        };
    }
    angular
        .module('blocChat')
        .controller('RoomModalCtrl', ['Room', '$uibModalInstance', RoomModalCtrl]);
}
{% endhighlight %}

Next I added logic to identify the active room and created message objects in the Firebase database to include this property. I added a message factory and controller in a similar fashion to those created for displaying and creating rooms. Next I included the Angular cookies module to enable tracking usernames, another property included within each message object. A run block was added to prompt new users to provide this information using similar modal logic as before. Then methods were added to execute when a user creates a new message, associating specific user and time properties with each message.

{% highlight javascript %}
this.sendMessage = function () {
    if (this.newMessage === '') {
        return;
    }
    var messageObj = {
        username: $cookies.get('blocChatCurrentUser'),
        content: this.newMessage,
        sentAt: new Date().getTime(),
        roomId: this.activeRoom
    };
    Message.send(messageObj);
    this.newMessage = '';
}
{% endhighlight %}

The final step was styling and incorporating a few UI features including the option to submit messages on click or keypress and integrating user thumbnails.

{:.center}
![]({{ site.baseurl }}/img/blocchat.png)

## Results

Regular testing of the app was conducted through the use of browser developer tools. With the help of an experienced front-end developer, I assessed limitations of the app and discovered several ways to enhance usability.

## Conclusion

This was an exciting project that not only challenged my existing knowledge of AngularJS, but pushed me to learn so much more. I experienced many pitfalls along the way, often when encountering new Angular requirements I had not experienced before. As, an example, integrating modal logic required extensive research including Angular UI Bootstrap documentation and searching for similar user stories on Stack Overflow. I also had a number of breakthroughs, my favorite being the implementation of a  simplified process each time I began working on a new feature. Quite often this involved defining the transactions that needed happen between the view, controller, services, and the database in isolated sequences, and finally assembling them all together. This project illuminated the power of AngularJS and its amazing possibilities when combined with other web tools like Firebase and external APIs like Angular UI. I also learned a lot about myself during this project, in particular my ability to rebound from failure and my potential to reach what seem like impossible goals at times.
