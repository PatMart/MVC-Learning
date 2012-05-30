
# Learning MVC (Model-View-Controller) Architecture In ColdFusion

by Ben Nadel (http://www.bennadel.com)

My application architecture is not great. While I know a great amount about ColdFusion
at a technical level, my understanding of how things fit together in cohesive modules
is not that strong. After talking to Steven Neiland at cf.Objective() 2012, I have a new
enthusiasm for bringing my architecture skills up to par. This project is an exploration
of a better MVC (Model-View-Controller) application architecture in which the controller
is really a thin layer that does little more than moderate requests and responses.

## Yes - Another ToDo Application

ToDo applications are the new "Hello World" apps in the programming world. They are simple
enough to build, yet complex enough to teach some theory. I'll be trying to build a ToDo
app using Model-View-Controller. While the app will only be used by a single person, I'll 
be building in a need to create an account for some added complexity and behavioral concerns.

## No Database

Sometimes, in order to learn how to do something, you have to handicap yourself. To help 
me learn about separation of concerns and modularity and single-responsibility, I am going
into this learning experiment without a database. All data will be cached within Gateway 
components for the life-cycle of the application. By doing this, I won't be tempted (or be
able) to have database queries in places that they shouldn't be; in other words, I won't be
able to have gateways that cut across too many tables. Nor will I have service objects that
can mutate data caches that don't really belong to them. 

## All CFScript In The Controller (C) And Model (M)

For the past 10 years, I've been living in a tag-based world. This has made it very easy 
for me to mix my "controller" and my business logic and my data access. For this exploration,
I'm going to go with nothing but CFScript in my Model and my Controller, leaving tags to only
the View layer of the application. The intent here is to make it much harder to mix the 
different parts of the application; at least, it makes it harder to mix the views and the 
business logic.

### What About Reporting?

I understand that, in reality, there are often different points of contact for a given set
of data. Reporting is a good example of this. I wouldn't necessarily want my primary data
gateways to be used for reporting; rather, I'd probably want a reporting gateway that can
flatten, denormalize, and optimize queries for a particular outcome. In this app, however,
there is no reporting - the point is to learn about tiered application architecture.

## User Cases

The following outlines a number of actions that the user will be able to perform in the 
ToDo application.

* Create a new account.
* Log into an existing account.
* Log out of an existing account session.
* Add a new ToDo item.
* Edit an existing ToDo item.
* Delete an existing ToDo item.
* Add a note to an existing ToDo item.
* Delete a note from an existing ToDo item.

## Architecture Notes And Random Thoughts

As I am going through this, I thought I would write down some random thoughts about the 
various aspects of the applicattion.

### Data Gateways And Exceptions

As I'm working on the data access portion of the application stack (data gateways in this
context), I've been grappling with whether or not to return a collection of entities; or, 
just to return a given entity. I like the idea of returning a collection, even when the 
intent is to find a single entity, as it allows me to always return non-NULL values from
the gateway. If I don't return collections, then I am faced with a decision as to what 
should be done when a given set of data cannot be found? Should I return NULL? Should I throw
an exception? 

In the case of ColdFusion, the language itself doesn't have great support for NULL values.
We now have the isNull() "function"; however, NULL values cannot be passed around. As such,
it seems like an attempt to return a given value should result in a data-access exception
if the given value cannot be found.

But, which functions of the data gateway layer should be considered to only load a single
entity? Getting something based on its unique ID definitely feels like a single-object 
intent. But what about getting something based on a username and password? This might feel
like a single-object intent; but, what if you can have inactive users? Who's to say that 
a given method will only ever return a single value?

Hmmmm....













