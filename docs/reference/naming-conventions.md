# Naming conventions (Find your way through the stack)
![](http://mu.semte.ch/wp-content/uploads/2017/06/naming-1024x768.png)

The mu.semte.ch framework consists of a number of components. A number that is growing steadily and will keep growing in the future. If you’re new to the stack, it’s not always easy to find your way through it. Therefore, we introduced some naming conventions. By adhering to these naming conventions we try to facilitate the hunt for the component you’re looking for.

Roughly speaking, we distinguish between 5 types of components in the mu.semte.ch framework:

*   Core components
*   Services
*   Templates
*   Ember addons
*   Utilities

**Core components** – as the name reveals – are the core of the mu.semte.ch framework. Without these components, the framework cannot function correctly. Each of the core components is prefixed with ‘mu-‘. Core examples include [mu-identifier](https://github.com/mu-semtech/mu-identifier) and [mu-dispatcher](https://github.com/mu-semtech/mu-dispatcher).

**Services** are the semantic microservices that compose the backend of an application. Each microservice is postfixed with ‘-service’. A lot of microservices are [already available](https://mu.semte.ch/components/#services), like a login service, a registration service, an export service etc. In case a microservice needs to be configured in a specific programming language the name may also include the name (or an abbreviation) of the programming language name. For example, a service named export-ruby-service requires an export configuration in Ruby while export-js-service requires a configuration in javascript

**Templates** are the starting points to build a microservice.  Since the programming language of a template is important – you will need to develop the microservice in this language – the name of the language is included in the template name. They are all postfixed with ‘-{language}-template’ like for example [mu-ruby-template](https://github.com/mu-semtech/mu-ruby-template) and [mu-javascript-template](https://github.com/mu-semtech/mu-javascript-template).

**Ember addons** are the building blocks for the frontend application. Basically, it are just NPM packages you can install in your Ember app. We follow the Ember community naming convention by prefixing the package names with ’ember-‘.  To indicate they are part of the mu.semte.ch stack we also add ‘mu-‘ to the name. E.g. ember-mu-login.

**Utilities** is the collective term of all tools that facilitate the initiation and development of a mu.semte.ch application. Since they are so diverse, they don’t follow a strict naming convention. The name just tries to cover the functionality. Nonetheless, similar utilities have a similar name. For example, all generators are postfixed with ‘-generator’.

All the components are published on GitHub where they are also added tagged so you can easily find what you’re looking for. E.g. the core components are tagged with ‘mu-project’, while the services are tagged with ‘mu-service’ etc. All the rules described above are just some simple and straightforward naming conventions, but adhering to them may drastically speed up the search process in the future.
