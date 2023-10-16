# (Lessons learned from) OSLO²

[Dacota.one](http://dacota.one) used mu.semte.ch to publish [CRAB-LOD as Linked Open Data](http://data.vlaanderen.be/doc/adres/2179183).

Publishing linked open data is not at the core of the mu.semte.ch stack, yet the choice makes sense. As Linked Data is the core data model in mu.semte.ch to connect the microservices, all necessary information is available.

In the light of publishing Linked Open Data correctly, we should publish the contents in various ways. One of the ideal methods of publication are the subject pages.  These pages provide a human view on the rendered contents.

Following is a set of lessons learned on using the mu.semte.ch stack for publishing Linked Open Data.

## Frontend generators

[OSLO²](https://overheid.vlaanderen.be/producten-diensten/OSLO2) uses mu-cl-resources to generate the API of the resources.  In this case [addresses, streets and municipalities](https://overheid.vlaanderen.be/producten-diensten/adressenregister-crab).  The content of the subject pages is everything specified in the mu-cl-resources configuration. This means we can derive the contents of the subject pages by interpreting the mu-cl-resources configuration.

Based on a core idea of generated pages on mu.semte.ch, similar to the [generators in Rails,](http://guides.rubyonrails.org/generators.html) the CRAB-LOD team extended our PoC efforts.  The core benefit of generating the pages is that they are adaptable.  The generators construct views which you can alter as needed, giving you a fast starting point.  The generators are handy when adding new fields, you can overwrite the generated contents.  But you can also override the general feel of the website whilst looking at the real data to publish, thereby making large style changes easier.  By materialising the generated views, customisation becomes easier.  Not all of these features were needed by CRAB-LOD, but it allows the approach to be used in more places.

The approach of using frontend generators worked fine.  We will further clean up the repositories and document the approach for future use.

## Content negotiation

There are multiple ways in which we may share a piece of information.  Content negotiation allows the browser to request the content in the way he wants it shared.

Based on the type of content the user requests, we should be able to dispatch to a different microservice.  One service may be responsible for hosting content as triples, another as a json-api. The mu-dispatcher does not support this.  In order to make the platform better suited for Linked Data publishing, we should extend the dispatcher with content negotiation capabilities.

We are strongly considering this extension to the mu-dispatcher.


## Read-only query optimisation

There are no live updates to the data in OSLO².  As such, many database queries can be cached, relieving the triplestore and rendering the pages more swiftly.

SPARQL queries can be sent as HTTP requests, which is the method we use for our microservices.  These requests are easy to cache.  Slight problem is that the requests are HTTP POST requests, which by HTTP semantics are not supposed to be cached.  This is correct, the contents of the triplestore may change and should thus, in the traditional interpretation, not be cached.

Bert Van Nuffelen built [a proxy microservice which caches the HTTP POTS requests to the triplestore](https://github.com/bertvannuffelen/SimpleSparqlCache).  Given a static database, like CRAB-LOD, this removes stress from the triplestore.  We have not ran benchmarks yet, but the API feels much more snappy this way.

## Linked Data Fragments

In a short wrap-up discussion with Pieter Colpaert on the subject, [Linked Data Fragments](http://linkeddatafragments.org/) (LDF) were pushed forward.  Although not connected to CRAB-LOD yet, it is a method of publishing Linked Data.

Given the brief discussion with Pieter, it appears to be easy to add this method of publishing Linked Open Data to the outside world.  LDF can be easy on the triplestore and makes federated SPARQL querying the default.  Check their [website](http://linkeddatafragments.org/) for more information.

We are looking into making Linked Data Fragments a drop-in addition to the framework.  Thereby giving you a free and promising extra way of sharing your contents with the world.

## Conclusion

The mu.semte.ch stack has proven to be a nice way of publishing Linked Data.

Given content available in the triplestore, manipulation is easy.  Performance is swift.  And with the generated subject pages, both adding content, as well as customising views when necessary is feasible.  With the integrated application, it is possible to add a different look and feel to the frontend, whilst still having much reuse.

It is nice to be used at an organisation like the Flemish Government and expect similar projects to appear in the future.

*This document has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/04/06/lessons-learned-from-oslo%c2%b2/)*
