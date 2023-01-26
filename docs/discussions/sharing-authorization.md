# On sharing authorization
Authorization is a cross-cutting concern\*.  By carving out the pieces each component needs to know, we minimize dependencies, maximize code-reuse, and make the system easy to understand.  In this article, we’ll describe the inner and outer workings of the accessibility model.

## Who needs to know?
Various entities need to have different information from the authorization system.  The database needs all knowledge, caching systems need a well-structured system for knowing whether a cached result can be used or not.

The database needs to have full access to the authorization system.  It needs to know what information can be read, and what information can be written.  As all information about the users should be stored in the database, the database itself should be able to define the access rights for a particular user.  Requirement: the database needs to receive the user’s session.  To some effect, the database can be considered the authority on user access information.  The database can derive the access rights and interpret them based on the session identifier.

Common microservices shouldn’t be bothered with access rights.  A simple microservice would execute operations (read or write) on the database in the name of the user.  If the microservice requests information which it is not allowed to see, the database will reject the operation.  Otherwise, the request passes through, based on the information this particular user is allowed to see.  Responsibility is thereby fully handed over to the database and the microservice needs no information about the access rights.

Microservices are allowed to cache results in separate databases, as long as they abide the semantic model.  With authorization handled by the database, this model now also consists of the authorization information.  The construction of a cache could take many shapes, and optimizations may be dependent on the specific authorization scheme.  It is our goal to provide a usable interface for common caching scenario’s.  A cache should be able to automatically detect whether it applies to the access rights of a particular user, and whether or not it can be used for other users.

In an ideal world, these caches can work automatically.  Any system may have unsupported cases.  We aim to support them with escape hatches as they appear.

## Defining access right tokens
All operations which occur in the stack are based on SPARQL queries.  Hence, the access right tokens should apply to SPARQL queries.  We consider two interesting sets: one set contains all access rights of the current user, the other set contains the access rights used to answer the query.

### Scoping access right tokens

In order to intelligently apply and use access right tokens we need to be able to compare them.  The tokens are JSON objects with two keys: *name* and *variables*.

Examples are:
```js
{ name: "user_basket", variables: ["42"] }
{ name: "public", variables: [] }
{ name: "suborganization_tickets", variables: ["rocket_engineers","nozzle_team"] }
```

In each of these examples, the *name* specifies a certain grouping, and the variables present a configuration of that grouping.  The logic constraints of these two properties will be described later.

### Access rights and executing queries
The SPARQL endpoint has all information available to execute a query.  The endpoint will emit both the access rights of the current user, as well as the used access rights to answer the query.  Let’s explain that in a bit more detail.

The access rights of the current user is an array of access tokens.  An example could be:
```js
[{ name: "public", variables: [] }, { name: "user_basket", variables: ["42"] }]
```

This user has access to all public information as well as to their own shopping basket.  These access rights will not change during normal operation.  Changes can only occur by logging in/out or explicitly receiving new access rights.  Hence the access rights can be cached.

Now assume the user executes a query for the names of all publicly available products in the webshop.  In such a case, the “public” access token is used, but no content for the “user_basket” token would be used.  In fact, in a realistic scenario, no user_basket would ever contain a product name.  The access right system could share that the user had these two access tokens, but that only the *public* access token was used in this request.

We specify the data structures of a token by use of the *name* property.  Each token with the same *name*, regardless of its *variables* may contain the same data structures.  The variables define scopings of the actual data.  the user_basket with variable 42 may contain different information than the user_basket with variable 68.

The access right system shares all groups to which a user had access in the MU_AUTH_ALLOWED_GROUPS response header.  The groups which could have been used to answer the query is returned in the MU_AUTH_USED_GROUPS.  The latter contains all groups which, based on the structure of their data, could have helped to provide an answer.

## Resulting properties
The authorization system has some interesting implications for practical implementation.  The most prominent ones are described in this section.

Tokens are communicated as JSON properties in the MU_AUTH_ALLOWED_GROUPS and MU_AUTH_USED_GROUPS headers.  These base technologies are already part of the mu.semte.ch microservice requirements.

A large set of microservices operate in the name of the current user, and can be implemented transparently.  These microservices can be upgraded solely by upgrading their microservice template.  Resulting in virtually no impact on existing services.

The tokens for a particular session are near-constant.  The only very common case for changing tokens is logging in/out.  This means that for almost all requests the user makes, we can use a cached set of access tokens.  These access tokens can be stored in the user’s session.

When executing a SPARQL query, the set of mu_auth_allowed_groups will always be a superset of mu_auth_used_groups.  If a group with name foo was in mu_auth_allowed_groups and not in mu_auth_used_groups, then this will be the case for all further executions of this query, regardless of the variables applied to the tokens.  Because of this we can...
1. Cache the result of a product listing query for a user which has tokens:
    ```js
    [{ name: “public”, variables: [] }, { name: “user_basket”, variables: [“42”] }]
    ```
2. Discover only...
    ```js
    [{ name: “public”, variables: [] }]
    ```
    ...was used to answer the query by checking the mu_auth_used_groups
3. And use the response for a user with tokens ...
    ```js
    [{ name: “public”, variables: [] }]
    ```
    ... or ...
    ```js
    [{ name: “public”, variables: [] }, { name: “user_basket”, variables: [“68”] }]
    ```


Results of executing multiple SPARQL queries can be simply combined.  Say a user has `tokens [a,b,c,d]`.  Assume a first SPARQL query has `used_groups [a,b]` and a second SPARQL query has `used_groups [a,c]`.  The resulting required access tokens for the full response would become `[a,b,c]`.  As such, it is easy for systems to combine the access rights for SPARQL queries.  This property allows for simple `access-rights-aware` caching.

## Conclusion
The authorization system can work by use of access tokens.  Most microservices can be upgraded merely by bumping their base microservice template.  The access rights tokens are JSON objects which allows us to inspect them.  The system is near-transparent for the end-user.


(*) The vast majority of authorization could be handled by the database.  Based on who is logged in, we can define what information they are allowed to see or manipulate.  Knowing the session, we could derive all information about access rights at the moment the user logs in.  However, there’s a catch.  Microservices are allowed to have local caches.  As these caches can be stored locally, responses may be generated without requests to the database.  In order to keep this path open, we need to be able to share access rights to these systems.

*This document has been adapted from Aad Versteden's mu.semte.ch article. You can view it [here](https://mu.semte.ch/2019/01/02/on-sharing-authorization/)*
