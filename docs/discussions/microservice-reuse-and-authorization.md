# On microservice reuse and authorization
Microservices make it easy to reuse logic across applications.  You can easily share the logic between applications by sharing the full microservice.  But there’s a catch, you must share the full microservice.  Hence, you must be able to use that exact logic.  *When a microservice embeds authorization logic, it can only be reused in domains with the same authorization logic.*

## Examples of authorization rules
Authorization rules boil down to the definition of the boundaries of information sharing.  In order to define *who* can see *what* information, the *who*<> and the *what* need to be defined.

As described in earlier blog-posts, the *who* can be connected to the session of the user.  Based on the information in the session, and a SPARQL query, we should be able to formalize the access groups of a user.  Practical examples of *who* are

-   an individual user,
-   a user belonging to an organization,
-   a user which has a specific role in an organization.

The *what* is a less explored domain in the mu.semte.ch stack.  Our goal with *what* is to find a definition which is sufficiently easy to use, but which covers sufficient use-cases.  Cases we’ve discovered are:

-   access to instances of an entity,
-   access to properties of an entity,
-   access to aggregated results.

These two topics can be combined.  The *who* and the *what* can be combined.  The visitor of a webshop could be allowed to see only *his own* (who) *shopping baskets* (what).  Without logging into the webshop, *any user* (who) might be allowed to see all *products* (what).

Aggregated results are a different beast.  A user which hasn’t logged in might be allowed to see the total amount of orders placed on the webshop, to indicate the trustworthiness.  This is an aggregated result.  A user may also be able to see how many orders were placed within his region, given that sufficient orders were placed in the region.  The constraints we could place in this domain seem very broad, and we feel we’ve only scratched the surface.  We’re leaving this to be described and tackled as we gain more experience.

Access rights could be defined in terms of *who* can access *what*.  The *who* boils down to users, groups, and individuals. The *what* which we aim to tackle are instances of a type, and properties of such instances.  The combination of *who* and *what* define what users will eventually be able to access.

## The boundaries of a microservice
The mu.semte.ch stack makes it super easy to build and share microservices.  With the semantic model, the intended data is clear, with Docker it’s easy to share and launch the services.  We consider these challenges intrinsic to the offered services.  You need to know what you are talking about, in order to offer operations on that data.  For most services, authorization is not an intrinsic challenge.  The same service could be offered in many authorization setups.

We have proven that we’re able to share a great set of microservices when the authorization domain is constant.  Application clusters built for a single customer, or solutions which work on open data.  Both of these cases hide the complexity of authorization, but in order to reuse the services with other authorization domains, a solution is necessary once again.

## Moving around authorization
Authorization is a problem which we should tackle transparently to the microservice . When we take a peek at the examples of authorization rules, we notice that many of them work on the data level.  As such, if we could handle authorization in the triplestore directly, we could unlock the reuse of many more microservices.

We have run experiments and have shown this could be achieved with a meta-SPARQL-endpoint.  A SPARQL endpoint which rewrites the queries of each microservice so they obey the access rights of an individual user.  Although there are quite a few side-remarks, such an implementation makes it plausible to reuse services which weren’t reusable earlier.  This is a complex challenge which we will need to face head-on.

With the right primitives, we can transparently rewrite the queries a microservice executes so it only operates on information a user has access to.

## Thoughts on information sharing and access control
Initial discussions revealed various ways in which information can be shared between multiple entities.  Like sharing a book, or filling in a form together.

In database terms, the most obvious sharing is bidirectional sharing.  Two people work on the same dataset, as if it were a shared spreadsheet.  All changes one person makes are visible to the other.

Another easy to understand way of sharing is unidirectional.  You rent a book from the library.  You can read the contents, but you can’t push any changes back to the author.

If you buy the book, you’re allowed to make annotations.  You can highlight contents, add side-remarks, rip whole chapters out of the book and rewrite them in your own phrasing.  You received the information, manipulated it, but do not share it back.

There’s also the notion of live versus offline collection of information.  In a live collection of information, we’re seeing the updates pass on immediately.  Whereas with bulk updates, we’d download some new contents and integrate them in our memory.

Secondly, we discover the act of reading versus writing.  You may be allowed to see certain contents, whilst not being able to write them.  You may be able to write certain contents, but not be allowed to read them (you could send log-files to an admin but never be allowed to read them again).  Or you could do both reading and writing.

In order to keep the approach conceivable, we’re working towards authorization with the three last mentioned forms of reading and writing.  We will consider bidirectional sharing only, and will require push updates to be solvable through escape hatches of the authorization system.

## Conclusion
Authorization is not an intrinsic complexity of microservices.  Extracting authorization rules allows sharing microservices across more applications.  Many authorization rules can transparently be handled by rewriting the SPARQL queries.

*This document has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2019/04/02/on-microservice-reuse-and-authorization/)*
