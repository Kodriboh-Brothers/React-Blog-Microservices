# React-Blog-Microservices

## Introduction 

Microservices allow for the division of features into self-contained services, allowing for teams to be split on the basis of expertise, different tools/langauges to be used interchangeably to fit the task of each service, and overall modularity within the application itself. 

Whilst microservices may appear more complex in their initial creation, the benefits of microservices are great. Other than the afforementioned advantages presented by microservices each service in and of itself becomes relatively simple, far easier to maintain and update, and more robust to failure. 

True microservices do not have a single point of failure. Each service should function normally in and of itself, despite the status of other services. This drastically improves up-time of your applications, and means that if one part of your application stops working the other services should still remain live. 

## Architecture

The most important consideration when building microservices is the architecture. A monolithic application can be seen as an application which contains all of the necessary items for the functionality of all of our features.

Microservices on the other hand can be described as containers containing all of the encessary items for one specific feature to funciton. For example, if we have a business which relies on renting out sports venues, we may have a service for Football Grounds, and another service for Tennis Courts. If something goes wrong with the Football Ground service, rendering it inaccessible, users should still be able to use our Tennis Court service normally. 

### The Big Data Problem

The biggest problem we face with Microservices can be considered a Big Data problem. If we think about it, the issue occurs due to how monolithic applications communicate data. In a monolithic architecture we would usually have a centralised database to store all of our data, in some cases we may have multiple databases, however, the point being they are all connected to the same application. 

If we think of microservices as a collection of projects, i.e. multiple small applications, the first question arises: how do we communicate data between these services? we could connect a database to all of these services, however, this once again creates a centralised point of failure. It would be far better if we could have a separate database for each service, but what if one service needs to access the data in another?

We cannot simply access the data through the other service, lest we create a dependency between the two services (all services should be self-contained). Instead, we should have each service manage it's own database, which should contain a copy of the data, this allows this particular service to work with data from another service, even if that other service goes down. 

Whilst this does create some sort of data duplication our services can manage this data via event emission upon database changes. Should a service go down we can therefore still access the other services, even if their "cached" data may be somewhat out of date.


### Synchronous vs Asynchronous Communication

As mentioned previously, when desinging our microservices we have a choice between various communication patterns, both of which have their own advantages and disadvantages. Synchronous communication is the style of communication be have briefly discussed. If we use a synchronous model we have our service communicate with other services to access data in their databases. Whilst this is conceptually much easier to understand, and your additional service will not need it's own database, this approach introduces dependencies between services. Additionally, if any single inter-service request fails, the overall requst will also fail. Finally, the entire request is only as fast as the slowest request in the chain, therefore even if one service only taks 1ms to respond, your users will still be waiting on that large 20s response, this can also introduce complexity in the form of a web of requests. 

Asynchronous communication on the other hand removes these dependencies by giving each service it's own everything. Each service communicates with it's own database, databases are managed via events emitted upon a database change. These are communicated to services via an event bus, which then sends the data to other services on the event endpoint, any service which cares about this data will then use this event to update it's data in it's own database. 

Without the database, it should be noted that this would be akin to synchronous communication as the key here is we are storing local copies of our data on each service which is interested in that data. This makes the speed at which services can access data faster, more reliable, and reduces dependency between services.
This can however be much harder to udnerstand conceptually and be much more complex to implement initially. 

<hr>

## Client-Service

## Posts-Service

## Comments-Service

## Query-Service

## Event-Bus