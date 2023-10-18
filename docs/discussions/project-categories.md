# What are the project categories?
These categories structure our re-usable code. The semantic.works framework consists of a number of components. A number that is growing steadily and will keep growing in the future. If you’re new to the stack, it’s not always easy to find your way through it. Therefore, we introduced different categories, findable through naming conventions. By adhering to these naming conventions we try to facilitate the hunt for the component you’re looking for.

All the rules described below are just some simple and straightforward naming conventions, but adhering to them may drastically speed up the search process in the future.



## Applications (`app-*`)
A full project, combining microservices into a full application. An example structure can be seen below:
![A flow chart. Single Page App points to Identifier, Identifier to Dispatcher. Dispatcher points to 4 differently coloured blocks: they say Registration, Login, Resources, and Files. These all point towards a regular coloured block in between them. This block says Store & Sync, and it contains a Triplestore header, under which Ontology, Delta and Security are noted](../../assets/simple-mental-model-advanced.excalidraw.svg)

## Core components (`mu-*`)
As the name reveals – core components are the core of the semantic.works framework. Without these components, the framework cannot function correctly.

Core examples include [mu-identifier](https://github.com/mu-semtech/mu-identifier) and [mu-dispatcher](https://github.com/mu-semtech/mu-dispatcher).


## Services (`*-service`)
### Naming conventions:
- Repo: `*-service` (e.g. *acmidm-login-service*)
- Image: `org-name/*-service` (e.g. *lblod/acmidm-login-service*)
- Docker Compose name: `*` (e.g. *login*)

In case a microservice needs to be configured in a specific programming language the name may also include the name (or an abbreviation) of the programming language name. For example, a service named export-ruby-service requires an export configuration in Ruby while export-js-service requires a configuration in javascript


Services are the configurable semantic microservices that compose the backend of an application, which don't require (a lot) more coding to implement a feature, generic or more specific.

For example, `mu-cl-resources` is an easily configurable solution in case you need to "list" resources (e.g. a shopping cart, people in a database...). Frontends are hard to re-use as your customer will probably want a specific feel or implementation, but universal features can be re-used to fit the needs of multiple projects.

Services in the semantic.works stack should...
- Contain hareable/re-usable/runnable functionality (see the paragraph above)
- Require minimal configuration (sensible defaults are very okay)
- Persist its state in the database
- Be user facing (usually)


Services can be found through...
- Searching for the `mu-service` topic on GitHub
- Finding projects extending the templates (e.g. `mu-javascript-template`)
- Asking around



## Templates (`mu-*-template`)
These are a base for a custom built microservice with minimal overhead. From these you should be able to have a microservice going in your preferred language in minutes.

Since the programming language of a template is important – you will need to develop the microservice in this language – the name of the language is included in the template name.

Templates in the semantic.works stack allow...
- Minimal setup time
- Nudging (yet not enforcing) conventions
- Flexpoints: e.g. allowing template updates where new features & conventions get added with minimal effort 

There are also some guidelines to using a template most effectively:
- Ground truth in database
    *(Caching is allowed, but keep in sync with the db)*
- Only talks to the database, not other microservices
    *(If this happens, it's for a well thought out reason: it is the exception, not the rule)*
- Limit abstractions
    *Keep the service easy to understand from a first read*
- Think about reuse
    *(Split where others might reuse the service)*


To allow for easier development, the templates...
- Often support live reload, so you do not have to manually restart during development
- Don't require (nor recommend) cloning the template repository. Simply using the Dockerfile is enough, and allows for a cleaner codebase
- Some templates have a debugger on one of the ports


## Ember add-ons (`ember-mu-*`/`ember-*`)
### Naming conventions:
- We follow the Ember community naming convention by prefixing the package names with `ember-`
- To indicate when they are part of the semantic.works stack, we instead use `ember-mu-` (e.g. *ember-mu-login*)

Like the configurable services, but for Ember.js frontend components and functionalities. These are the building blocks for the frontend application. Basically, it are just NPM packages you can install in your Ember app. 

## Utilities
Utilities is the collective term of all tools that facilitate the initiation and development of a mu.semte.ch application. Since they are so diverse, they don’t follow a strict naming convention. The name just tries to cover the functionality. Nonetheless, similar utilities have a similar name. For example, all generators are postfixed with `-generator``.



*This document includes content adapted from Erika Pauwels' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/06/22/find-your-way-through-the-stack/)*
