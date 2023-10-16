# Masterclass
In this document, you will learn the following:
1. The core values of the semantic.works stack and their benefits
2. How our design decisions reflect those values
3. What this resulted in


## Core values
### Keep It Simple Stupid
#### 1. Productivity through simplicity
- 1a) **Efficient development:** We wanted to get stuff done, without the requirement of being an expert on every topic, or creating a system that only the computer would understand
- 1b) **Clear overview:** Efficient development also comes in the form of overbloated applications, where you can easily lose track of what is handled where. We want to be able to look at a microservice and know immediately what it does.

#### 2. Functionality through interoperability
- 2a) **Maximize freedom:** There need to be rules: as few and liberating as possible. A lack of rules would cost interoperability, which would subsequently limit your freedom in the end.
- 2b) **Expandability:** We also wanted orthogonal features: features that would extened eachother at no extra cost.
- 2c) **Longevity:** Something built years ago should still work as expected.


### Not getting stuck
#### 3. 

<br>

## Design decisions
### Reuse everything
- Small but focused services allow reusing everything ([1a](#1-productivity-through-simplicity))

### Micro
- Small but focused services allow reusing everything ([1a](#1-productivity-through-simplicity))
- They are oftenly very easy to read, allowing a basic understanding even for those who do not know the language or framework in question
- Due to the small size of the services, debugging is easier: there are only a few hundred lines of code per service ([1a: efficient development & 1b: clear overview](#1-productivity-through-simplicity)).

### Standard API's
- Thanks to using technologies like JSON:API, services built years prior can still work as expected. ([2c: longevity](#2-functionality-through-interoperability))

### Centralised communication
The microservices never talk to eachother directly. Instead they use a **shared linked-data** database. This is complemented by microservices using a semantic model. ([2a: maximize freedom & 2b: expandability](#2-functionality-through-interoperability))

### Semantic models
![A venndiagram of two circles. The registration service circle encapsulates E-mail, Date of birth, Username and Password. The login service circle goes over Username and Password](../../assets/shared-data-models.excalidraw.svg)
Semantic models have a bunch of advantages:
- They can easily be appended upon ([2b: expandability](#2-functionality-through-interoperability))
- They allow re-using the same models: the same model can be used for username/password, OAuth, or even mock logins ([2a: Maximize freedom, 2b: Expandability](#2-functionality-through-interoperability))

## Implementations

### Simple Mental Model
#### Basic
![A flow chart. Single Page App contains "Client" and points to Support Layer. Support layer contains "Identifier" and "Dispatcher", and points to Microservices. Microservices contains many microservices, and points to Store & Sync. This contains a Triplestore header, under which Ontology, Delta and Security are noted](../../assets/simple-mental-model.excalidraw.svg)

#### In-depth
![A flow chart. Single Page App points to Identifier, Identifier to Dispatcher. Dispatcher points to 4 differently coloured blocks: they say Registration, Login, Resources, and Files. These all point towards a regular coloured block in between them. This block says Store & Sync, and it contains a Triplestore header, under which Ontology, Delta and Security are noted](../../assets/simple-mental-model-advanced.excalidraw.svg)
([micro](#micro))

1. The **identifier** will create a session cookie. This doesn't identify you as a person (as this depends on what is in the database), but it gives a general hook ([centralised communication](#centralised-communication))
2. The **dispatcher** will decide which **microservice** will be called for the made request
3. Then the **triplestore** will change the requested state, yield a response of what has changed in the database, read the information you requested...

For example: this setup also means that the other services don't care what registration method was used, as they all work from the same [semantic models](#semantic-models) and database, yet care about different aspects of it.

### Limited base technologies
- **HTTP for communication:** If you build web application, you're probably already familiar with HTTP. ([Standard api's](#standard-apis))
- **JSON(:API) for sending data:** any programming language or framework that can read the widely known and used JSON will be able to be used. We then selected JSON:API to ensure consistency in our structure ([Standard api's](#standard-apis))
- **SPARQL for storing data:** SPARQL queries are also just strings: any technology that can manage JSON will be able to use SPARQL in one way or another. This way we can store all our data as linked data, whilst enjoying the compatibility of JSON.


### Docker & Docker-Compose
Docker allows for most technologies to be used on most systems. Docker-Compose are YAML files describing the topology of Docker containers and how they interact whith eachother, which gives us a place to define a central structure of our project. ([Micro](#micro), [Standard API's](#standard-apis))

#### Naming conventions:
To ensure quick onboarding, a lot of our docker-compose files following a naming scheme:
|           Filename            | Description |
| ----------------------------- | ----------- |
| `docker-compose.yml`          | General **production** setup, basis for all other docker-compose-*.yml |
| `docker-compose.dev.yml`      | Overrides for **development** | 
| `docker-compose.override.yml` | Overrides for specific systems/deployments. Do *not* commit |
| `docker-compose.demo.yml`     | (Extra) Demo to help people get started |


<br>

### Categories
These categories structure our re-usable code.

#### Applications (`app-*`)
A full project, combining microservices into a full application. The structure is as seen above in [#Simple Mental Model](#simple-mental-model)

#### Templates
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


#### Services (`*-service` / `mu-*`)
##### Naming conventions:
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


#### Ember add-ons
Like the configurable services, but for Ember.js frontend components and functionalities.


*This document has been adapted by Denperidge from Aad Versteden's masterclass 01 - How and why pt1, pt & pt3. You can view the original video [here]()*
