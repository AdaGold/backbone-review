# Backbone Review

## Intention
Let's think about the types of applications that we have built using JavaScript thus far. Scrabble was a basic functional program, but didn't really have any front-end component that did DOM manipulation or eventing. Trek was a nice SPA that used AJAX to interact with an API and jQuery events to provide much of the functionality. These types of applications were fairly small with not a lot of features or components that were difficult to keep track of.

Let's say that Trek had 5 times as many features that were built with similar interactions with APIs and events. This would be much harder to keep track of utilizing the unstructured process we were using originally. **This is where we will see the greatest use-case for utilizing Backbone as our JS library.**

## Setup
This repo includes a completed Backbone application version of Trek that we will refer to for these activities.

You should **fork and clone this repository to follow along.**

***

## 1) Review Components
![Backbone JS Architecture from http://www.slideshare.net/ronreiter/writing-html5-web-apps-using-backbonejs-and-gae](backbonejs-architecture.jpg)

Below are the Backbone components we have focused on to build our single page applications. Each component has a brief explanation along with a link to it's documentation.

- [Model](#model)
- [Collection](#collection)
- [View](#view)
- [Events](#events)

### Model
Models are the component of our Backbone application which retrieves and populates the data. Models also contain our applications business logic.

### Collection
Collections handle a group of related models. When initially thinking about a collection, they may seem like simply a list, but they are much more powerful. Collections are useful because they can allow you to listen for (and act on) events that occur on any model within the collection.

### View
Views act as the HTML representation of your Backbone Models. Views have much more power than Views in our Rails applications. Views act as the broker between the data in our application and the HTML displayed to the user.

The two main purposes of views are:
- Templating (rendering HTML)
- Eventing (triggering and handling events)

##### Clarifications on Views
**Notes on `el` `$el` and `this.$`:**
- **`el`:** A View's `el` property is it's _DOM element_. Every view has **one** and by default, unless otherwise defined, it is a `div` element.
- **`$el`:** The _jQuery wrapper_ around the View's `el` property. It gives you easy access to jQuery methods like `show()`, `hide()` and `addClass()`.
- **`this.$`:** The jQuery selector function, [_scoped_](http://api.jquery.com/jquery/#selector-context) to only search within the children of the View's DOM element. This allows you to make selector calls and be certain that you're not affecting any parts of the page outside of your View's responsibility.

You should always design your Backbone Views so that they never modify DOM elements that aren't children of `el`. The easiest way to make sure that your Views hold to that design is to **only ever use `this.$` and `$el` to modify the page**.

Using the standard jQuery `$` is acceptable if you need to _read_ parts of the page, when you might need to get the content for a template.


### Events
Events are defined inside of your Views. We can utilize three different types of events to create a truly dynamic site.

**DOM Events**: These are events that are triggered within our `events: {}` or `on` code in the View. These events occur when there is something directly happening in the DOM.

**Custom Events**: These are events which we can create ourselves by extending `Backbone.Events`.

**Backbone Events**: These are _technically_ custom events which Backbone has created for us based on common things that we need to do in a Backbone application. These are things like `add`, `update`, and `remove` on models and collections.

#### Documentation
[Backbone.model Documentation](http://backbonejs.org/#Model)

[Backbone.collection Documentation](http://backbonejs.org/#Collection)

[Backbone.view Documentation](http://backbonejs.org/#View)

[Backbone.events Documentation](http://backbonejs.org/#Events)

[Backbone built-in events](http://backbonejs.org/#Events-catalog)

**Activity:**
Go through the Trek application and list out each individual component. Use the diagram above to help you label each component as Model, View or Collection.

***

## 2) How we have structured our Backbone applications
The point of Backbone is to provide a very intentional structure to our front-end code, so that we know where to find the code that is handling the specific functionality of our application.

With Backbone being a less opinionated library/framework, it also has less rules and conventions on how to organize your application and where you implement specific functionality.  We've structured our App as follows.  


### App.js
App.js is our starting file for execution.  It creates the models & our ApplicationView to start the page.  

### Views
- **ApplicationView** was the most arbitrary. Why is it there?
  - The ApplicationView controls the whole viewport and handles the form for creating new trips.
- **TripListView** 
  - The TripListView manages the list of trips and listens for events on the collection and when individual trips are selected.  When they are it shows the individual trip's details.  
- **TripView**
  - A TripView renders one trip Model using an underscore template.  


### Models

Right now our models & collections have done very little. We have used them to set defaults.

Then can also connect to APIs CRUD operations, provide business logic and let Views know when the data has changed with Events.  

We will utilize them a lot more with Tic-Tac-Toe, and that lesson will come on Monday.  Yippie!

### Collections


### That Other stuff
Because our Backbone application relies on a few other libraries to get it's gear'a'grinding, we have a json file that keeps track of those libraries and will include them into our project when we run `npm install`.

To add more libraries, or to see which ones are already included, open the `package.json` file and look under `"devDependencies": {}`

If this process sounds familiar, it's because it's basically the same as having a gem file in rails and running *'bundle install'*.

- **Webpack**

- **Underscore**

- **JQuery** Anytime you see that money sign, you're working with a Jquery object. That Jquery object is set by referring to a

***

## 3) Review How the Components Interact with Each Other
Each component is responsible for specific functionality of our application. These components rely on each other, which can quickly complicate how data flows through our application.

The below activities practice understanding how the components interact with each other.

- [Rendering Views](#rendering-views)
- [Tracing Events](#tracing-events)
- [Initial Page Load](#initial-page-load)

### Rendering Views

**The structure of our HTML has hierarchy** All HTML elements are nested inside of **one** other element, other than the `<HTML>` tag. The *parent* element acts as a container for it's nested *child* elements.

Backbone views can have the same relationship, and generally mirror the HTML's hierarchy. Some backbone views will be responsible for *child* views and act as a container with hierarchy over the inner-elements that are generated from those *child* views. For example, in the Rolodex project, the rolodex view was responsible for rendering many contact views.

With our Backbone applications, a single page will likely have many different Backbone views rendering at once.


**Activity:** Views Responsibility

With this activity, we will identify what sections of the DOM map to specific views.

Each of the following views should correspond to a color (or other distinct marking, like dashed lines).
**ex:** tripView is yellow, tripListView is blue and appView is red.

On the worksheet, use that color to draw a square around all the code snippets that correspond to each of these views:

- tripView
- tripListView
- appView

Once you have finished, compare your worksheet with a neighbor. Review the code together and come to a consensus.

### Initial Page Load
**Activity:**
Some of our JavaScript is run immediately when the page loads, and some of it doesn't run until the user does something. To help understand which is which and in what order various things happen, let's leave ourselves a trail of breadcrumbs to follow. We'll start by looking at what does and doesn't happen when the page is first loaded.

To do so, we'll add a log line to the top of every function in our TREK app. Each line should be of form

```javascript
console.log(">>> BREADCRUMBS: #");
```

Where `#` is the order in which the log line appears when the app is started. If the function shouldn't be called when the page is first loaded, replace `#` with `does not print`. So the first line to be printed out should look like

```javascript
console.log(">>> BREADCRUMBS: 1");
```

Did things appear in the order you expected? Did you get any `does not print`s? Check in with the person next to you and verify, then revise your numbering if needed.

Now change your numbering to reflect the order that functions are called when various buttons are pressed.


## Tracing Events

**Activity**
It can be difficult to figure out when functions get called when we're listening to events and there's not one line of execution to follow.  Now you and your partner will be diving into the events and the functions listening to those events in this Trek App.  

### DOM Events

Using a whiteboard make a list of the DOM events each of the views are listening to and draw arrows to functions that the events trigger.  

-  When button 'See All Trips' is clicked
-  When button 'Make Reservation' is clicked

![image of a DOM Event linked to a function](images/diagramEvents.png)

Now look at the event handler functions, do they invoke other events or functions?  Diagram those as well.

### Backbone Events

Look at the `listenTo()` calls in the initialize functions.  Are any of the views listening to events on other objects, such as Models, Collections or Views?

Diagram those events as well.

### Check your work

Now cause the events to trigger, for example click on a trip, or add a reservation.  

-  Look at the console log messages you put in earlier for the Page Load activity.
-  Did they print? Were they in the expected order?  Why or why not?
-  How many events did you identify?  Check with a neighbor to see if they have the same.  


## 4) Would this be a backbone app?
For each of the following websites, take a minute or so to look over the page's design, determine whether or not it would make sense to build the site as a Backbone application, and write down your reasoning for that determination:

* [SoundCloud](https://soundcloud.com/)
* [Zombo.com](https://html5zombo.com/)
* [Gmail](https://gmail.com/) (the actual email interface)
* [YouTube](https://youtube.com/)
* [Cachemonet](http://cachemonet.com/)
* [Zoomquilt](http://zoomquilt.org/)
* [APIdock](http://apidock.com/ruby)
