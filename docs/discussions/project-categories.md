# What are the project categories?
These categories structure our re-usable code.


## Applications (`app-*`)
A full project, combining microservices into a full application. An example structure can be seen below:
![A flow chart. Single Page App points to Identifier, Identifier to Dispatcher. Dispatcher points to 4 differently coloured blocks: they say Registration, Login, Resources, and Files. These all point towards a regular coloured block in between them. This block says Store & Sync, and it contains a Triplestore header, under which Ontology, Delta and Security are noted](../../assets/simple-mental-model-advanced.excalidraw.svg)

## Templates
These are a base for a custom built microservice with minimal overhead. From these you should be able to have a microservice going in your preferred language in minutes.

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


## Services (`*-service` / `mu-*`)
### Naming conventions:
- Core service: `mu-*` (e.g. *mu-dispatcher*)
- Regular service (repo): `*-service` (e.g. *acmidm-login-service*)
- Regular service (image): `org-name/*-service` (e.g. *lblod/acmidm-login-service*)
- Docker Compose name: `*` (e.g. *login*)

(Configurable) services built to not require (a lot) more coding to implement a (generic) feature. For example, `mu-cl-resources` is an easily configurable solution in case you need to "list" resources (e.g. a shopping cart, people in a database...). Frontends are hard to re-use as your customer will probably want a specific feel or implementation, but universal features can be re-used to fit the needs of multiple projects.

Services in the semantic.works stack should...
- Contain hareable/re-usable/runnable functionality (see the paragraph above)
- Require minimal configuration (sensible defaults are very okay)
- Persist its state in the database
- Be user facing (usually)

Services can be found through...
- Searching for the `mu-service` topic on GitHub
- Finding projects extending the templates (e.g. `mu-javascript-template`)
- Asking around


## Ember add-ons
Like the configurable services, but for Ember.js frontend components and functionalities.

