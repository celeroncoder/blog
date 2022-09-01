+++
ShowToc = true
date = 2022-08-31T18:30:00Z
description = "A guide to SOA and should you use it or not?"
draft = true
tags = ["guide", "soa", "architecture"]
title = "Should I adopt Service Oriented Architecture (SOA)?"
[cover]
alt = ""
image = ""

+++
The main principle of Service-Oriented Architecture is to **segregate the concerns to move the business logic and the vulnerable data layer off to the API routes directly. In this, we isolate different parts of the logic in such a way that makes sense.**

In this three layer or tier system we usually divide the logic of a route or a function of the application into Controller, Service, and Data Access Layer.

![](/blog/uploads/soa_architecture.png)

### Controller (Routes)

Its purpose is to deal with requests from the user to the application and deal with the different routes that have different functions. It also modifies the incoming data from the user to be able to consume it within our business logic.

It's also responsible to return the processed or requested data to the user in a suitable format. Generally, _the routing mechanism controls which request hits which controller._

It's tempting to put the business logic inside the controllers but is advised not to, since as your application grows it will quickly become overwhelming and the spaghetti code inside the controller might just overshadow the real purpose of the controllers and create our application vulnerable and lot less maintainable.

***

### Services \~ The Logic

This is where the actual logic of the application resides, it contains our CRUD operations, validations, authentication and authorization of users and data. This is the business logic layer where the data from the user is processed that the service gets from the controller and the data access layer is summoned the return data to the user.

A major advantage of splitting our business logic like this is that our codebase becomes a lot more maintainable, and we write clean code and tend to make a lot less make mistakes.

The service layer is the actual heart or core of our application it contains all the magic of the application, for example the application generates invoices from the user’s data, this is where the invoice will be generated, and the data will be persisted, and a response will be sent to the user via the controller.

***

### Data Access Layer

It's the layer where the logic related to our data persisted in some sort of database is modified, read, and deleted. It doesn’t really have a rigid structure like the other two. And what I mean by structure is that the controllers and the services are put into different files, but this is actually some sort of ORM like _prisma or mongoose_ which **can talk to the persisted data.

***

### Why use it?

The most common question in your mind would be that why should I use this, and not put all the logic inside a single function or layer so to say. I know that I have persisted that it makes the codebase more manageable, but that isn’t such a solid reason to do so, for like small projects with less logic and fewer routes.

To convince you into using this approach, here are some advantages to this 3-tier-architecture:

* **Scalability**

  It makes our application much lighter and that’s what makes it much more scalable.
* **Easy Integration**

  When you are creating an application, it is probably using some sort of third-party library or is talking to some other application to send a response to the user. This service-based architecture which promotes segregation of logic is great at it.
* **Security**

  This approach to the problem is best because it validates the data over and over again and reduces the probability of some malicious data entering our application, since data is modified and validated at every layer checkpoint, so without any extra effort it gives you the security edge.
* **Testing**

  It also makes it much easier to test the application, since the logic is isolated to services and controllers can be checked easily, which results in easy testing experience and fewer bugs in your application.
* **Reusable Code**

  Developers are often pressed by the fact that they should write DRY (don’t repeat yourself) code, this service-based architecture is perfect for code reusability, for example a service can be used in multiple controllers or other services with methods like **dependency injection**, etc.

***

### Is it that good?

The Service-Oriented Architecture is not the best fit for all sorts of use cases, in some cases it might be a disaster to adopt this.

One of the major disadvantages of it is its high overhead, and high starting cost.

* **High Overhead**

  This architecture has a high overhead within the application, and not only it may suck in memory but also in developer experience as there’s a lot of boilerplate code required to make this work.
* **High Investment**

  This architecture demands high investment of money and time from the start. It demands different developer roles for each layer and also required a lot of development time.

***

### Should I adopt it?

The big question is, should I actually adopt this highly boilerplate rich, secure and complex architecture? This question can’t be answered with just this. You have to make this decision yourself based on your requirements, **project size, budget and time**.

If you’re creating a minuscule application that just have few routes and some logic, you shouldn’t adopt this, since it will become a pain to maintain it for such tiny project.

But, If you wish to create a highly complex large project with a lots or routes and tons of logic, it may be a good fit for you.

Either way, you’ve to decide this yourself. There are some really nice frameworks out there that make starting with it much easier, like _NestJS, Spring for APIs and Backend Stuff and AngularJS for Client Side._