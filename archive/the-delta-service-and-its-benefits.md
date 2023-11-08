**Editors note**
The following has been archived due to delta-service being superseded by delta-notifier. More info on the deprecation can be found in the [delta-service repo](https://github.com/mu-semtech/archived-mu-delta-service)

*This document has been adapted from Jonathan Langens' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/05/11/the-delta-service-and-its-benefits/)*

---

# The delta service and its benefits
The delta-service has already been mentioned and used in [previous blog posts on reactive programming](https://mu.semte.ch/tag/delta-service/) to make microservices hook into changes introduced by other microservices. In this article we will elaborate in more depth on this service. What is the delta-service and what are the benefits of using it?

## What is the delta-service?
![](http://mu.semte.ch/wp-content/uploads/2017/05/wild_e_delta-300x200.png)The delta-service is a microservice that offers a SPARQL endpoint. It accepts SPARQL queries, analyses them, calculates the differences a query introduces into the graph store and notifies interested parties about these differences.

The reports that will be sent to interested parties are formed with triples as the basic building blocks as triples are the resources that those SPARQL stores (mentally) have in common.

Conceptually such a report would looks somewhat like this:

    + <subject> <predicate> <object>
    + <subject> <predicate> <object>
    - <subject> <predicate> <object>
    - <subject> <predicate> <object>
    - <subject> <predicate> <object>
    

All triples with a ‘+’ in front will be inserted and all triples preceded by ‘-’ are triples that will be deleted by the query.

## What are the benefits?

### Graph store independence
The delta-service has the obvious benefit that the framework can make abstraction from the graph store you are actually using. For instance, on a Virtuoso graph database both update (INSERT, DELETE) and select queries (SELECT, ASK, DESCRIBE) are sent to the same endpoint: [http://localhost:8890/sparql](http://localhost:8890/sparql) by default. On an OWLIM database on the other hand update queries are sent to [http://localhost:8001/openrdf-workbench/\[repository\]/statements](http://localhost:8001/openrdf-workbench/%5Brepository%5D/statements) while a select query is sent to [http://localhost:8001/openrdf-workbench/\[repository\]/query](http://localhost:8001/openrdf-workbench/%5Brepository%5D/query). The delta-service may even support idiosyncracies of a certain triple store like reforming queries if you know that the graph store cannot properly handle a specific query.

### Messaging
Additionally a service that can notify interested parties of the changes in the graph store can easily replace messages and message queues. While message queues do scale, they still require a shared mental model between the producing and the consuming microservices. This mental model is enforced by most messaging systems while with the delta-service you are free to subscribe to whatever you want. As long as it is expressible in a triple, a microservice can hook into it.

The producing microservice does not need to compose a specific message for the message queue. It just writes the triples to the triple store as it normally would (within the [mu.semte.ch](http://mu.semte.ch/) framework the triple store holds the ground truth) and the delta-service takes care of the rest. So here the win is: no mental model change.  Also the consuming microservice on the other hand does not need to know the transformation model the producing microservice uses to transform the message to the message queue format. Again it only needs to know triples.

And here we win a lot with the added semantic value of the controlled vocabularies used with linked data technology. For example should I install a microservice on my platform that performs actions every time a concept scheme is added to the SPARQL store, then that microservice just needs to listen to all changes on URIs that are a SKOS:ConceptScheme.

In short, while you can build a message queue with this technology the messages would always handle a view of the model but by being able to consume the delta reports all the reactiveness is being leveraged by nothing but manipulating the model.

### Scaling
The delta-service may also facilitate the scaling of the mu.semte.ch platform. If we would introduce a master graph database instance to which all updates (but preferably no selects) are proxied then we could put a delta-service in front of this master graph database and send mutation reports of the master graph database to a slave battery updater. This slave battery updater can then update an array of slave stores with every report. The slaves can be used for select queries. This way we have free scaling of the graph database and even more: we can have multiple different types of graph databases all in sync.

### Reactiveness
The delta-service enables us to use a new approach to developing applications: reactive programming. Make a microservice hook into changes produced by another microservice. Have a look at [the blog posts we’ve already published on this topic](https://mu.semte.ch/tag/reactive-programming/) to learn how to make a microservice reactive.

## Conclusion
The delta-service is just a microservice, but one that offers a lot of benefits. In the future we will publish more blog posts illustrating how you can use the delta-service in your mu.semte.ch project and easily benefit from the difference reports it generates.