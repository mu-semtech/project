# Creating applications
This document will help you get a basic understanding of how to spin up your own application.

*Note: every service with an asterisk (*) is required.*

## Adding mu-identifier *
This is the entrypoint of your application. It...
- ... identifies the browser agent (mu-session-id header)
- ... caches user's access rights (mu-auth-allowed-groups header)

```yml
# docker-compose.yml
services:
  # ...
  identifier:
    image: semtech/mu-identifier:1.10.1
    ports:
      - "80:80"
    links:
      - dispatcher:dispatcher
```

## Adding mu-dispatcher *
The identifier selects the right service for a request

```yml
# docker-compose.yml
services:
  # ...
  dispatcher:
    image: semtech/mu-dispatcher:2.0.0
    volumes:
      - ./config/dispatcher:/config
```

```ex
# config/dispatcher/dispatcher.ex

defmodule Dispatcher do
  use Matcher

  # This can be expanded down the road to allow paths to only accept a certain type (e.g. json, html)
  # These are called accept headers
  # See the mu-dispatcher docs for more information
  define_accept_types [ ]

  # Defines a GET path on /example, including any and all subpaths (/example/test, /example/example)
  get "/example/*path", _ do
    # The servicename should be replaced with the service name defined in docker-compose
    forward conn, path, "http://servicename/examplepath/"
  end

  # General catch-all, returning a 404 error message
  match "/*_", %{ last_call: true } do
    send_resp( conn, 404, "{ \"error\": { \"code\": 404, \"message\": \"Route not found.  See config/dispatcher.ex\" } }" )
  end

end
```

## Adding virtuoso *
It stores your data!

```yml
services:
  # ...
  dispatcher:
    image: redpencil/virtuoso:1.2.0-rc.1
    environment:
        SPARQL_UPDATE: "true"
        DEFAULT_GRAPH: "http://mu.semte.ch/application"
    volumes:
      - ./data/db:/data
```

## Adding mu-cl-resources
Our frontends speak JSON:API, whilst our backends speak SPARQL. 
Mu-cl-resources allows these two to talk to eachother easily, by taking an API description and generating an implementation from it.

For information on implementing mu-cl-resources, view the documentation on the [semantic.works website](https://semantic.works/docs/mu-cl-resources).

