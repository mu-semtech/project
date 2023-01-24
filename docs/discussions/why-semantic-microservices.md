
# Why Semantic Micro Services? (Semantic Micro Services, Why Bother?)
![](http://mu.semte.ch/wp-content/uploads/2017/06/semantic_architect-1024x768.png)

## What is a semantic microservice?
What I mean (in this post) with a semantic microservice is a small self-sustainable piece of software. That software should only handle one specific logic. And it has to have a semantic model, a model that is expressed in RDF with the usage of (preferably) published controlled vocabularies.

## Decoupling the dependencies

Each piece of software essentially operates on a model, an abstract representation of the world, and manipulates that model to fulfil the tasks it has. In traditional software approaches this model, for persistence purposes at least, is described in normalised tables. Most of the time the internal software model is designed to reflect this.

In any case there is a requirement on the logical level of the software to be aware of that model that can be found in the database. This is a dependency that can be found between all microservices that compose a model. True, this is not an obstacle that cannot be overcome. If you look at Drupal you see that all modules that you install happily work together. This is because they share the same database model. Of course the pieces of logic that are not Drupal-specific also need to be aware of this model.

But it goes further than that. Traditional microservices are bound together by that shared model. In any case if I have a table describing company personnel then that table will contain all data that is available to the software. Of course there is a practice of splitting of information blocks that are shared between different types of entities (addresses being the typical example).

With RDF there is no such dependency. Since triples are used to express the model there is no need for a combined model. When it comes to the standard user information in the [mu.semte.ch](http://mu.semte.ch/) stack for instance, you will see that a user has the following properties: a login name, a password, a real name, an email address and a date of joining. But the login microservice doesn’t care about the last 3 “fields”. So it restricts itself to the predicates for login and password. After that it adds a session “field” to that user in the database which in turn is not even being considered by the subscription microservice.

## Real reusage of microservices
This semantic model also allows us to completely reuse microservices without having to consider the model used. Here the controlled vocabularies help a lot. If I have knowledge described in my application and should I have had the sensibility to describe that knowledge with the SKOS vocabulary then I can take just about any taxonomy/concept scheme microservice and install it in my system. The beauty of these semantic vocabularies is that the disambiguate meaning. In a normalised configuration we would have ended up with tables that would for instance be called concept\_scheme, ConceptScheme, conceptScheme, CONCEPT\_SHEMA, taxonomy, Taxonomy, … not to mention the same in dutch or whatever language. The controlled vocabularies allow us to identify the “class” of a concept scheme without ambiguity. When you consider properties this advantage only gets bigger.

If this interests you just google “semantic web” or “linked open data”.

## SPARQL, Triples and Reactive software

SPARQL is the language that is used to query an RDF store. These store usually express statements in triples. Within the [mu.semte.ch](http://mu.semte.ch/) microservice framework we have developed a component that calculates changes introduced by queries in the database. This component then notifies microservices that have subscribed to these changes. This allows us to take a microservice that was developed for a certain task, plug it in our application and without any wiring have it “react” to what other microservices are doing.

This also replaces our needs for a message queue. One can of course point out that such a message queue has several advantages such as speed. But it also comes with a hidden disadvantage. Namely every action taken or message send has to be formatted in some way. This is yet again another dependency in your software system. Any microservice that you want include in your system and that needs to process the messages from certain other components will need to be developed with the knowledge of that message model in mind.

Another dependency removed with semantic technologies…

*This document has been adapted from Jonathan Langens' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/06/15/semantic-micro-services-why-bother/)*
