# Develop with your local IDE and tools inside a Docker container

## docker-compose
[mu.semte.ch](http://mu.semte.ch/) applications consist, in the back end, of microservices. Those microservices are docker containers that run a lightway server framework and offer logic on certain routes. Those routes are described in the dispatcher and the composition of the microservices is described in the docker-compose.yml file.

Generally it is undesired that a microservice would talk to another microservice. In exceptional circumstances this is however allowed. We will maybe at some other point publish a blog post describing these cases but for this post I will use the example of a cache microservice. Conceptually this is a simple idea that caches the responses that a microservice returns on certain requests. In this case you would need to know the calls that come in from the front end, through the identifier and the dispatcher and the responses that come from the microservice in question.

It is clear that mocking this is non-trivial.

## front end complexity
Within the [mu.semte.ch](http://mu.semte.ch/) applications a lot of complexity is sewn together in the front end. The back end is an API that offers logic on the world (SPARQL endpoint) and the front end calls it. There are many examples of times when it is hard to figure out exactly what is going wrong. You could use a library that abstracts the API you are consuming away but how will you then know exactly which calls it makes?

You could open your developer tools and copy the network calls but this would still fail to capture the mutations that happen in the identifer and the dispatcher. Furhter it can be really handy to just click and see the calls happening while your break points are conditional and thus only fire when needed. Clickig costs less time than fabricating curls.

## wrapping 3rd party applications
While it is true that it’s not too hard to wrap applications like SOLR, Elastic Search, Spark, Unified Views, … in a docker. It is far more convenient to see how the tool reacts when you can run it locally as opposed to in a docker. While this is certainly not the largest use case it can be frustrating to figure out what is wrong with such an application when something goes wrong. In this case it also helps if you can develop on localhost.

## so we develop locally
One could exec in the running docker and dev from there. Most major language offer reasonable dev and debugging tools for the command line. But still this never matches the full blown capabilities of an IDE such as IntelliJ, Visual studio, Atom or \[you favorite here\].

![](http://mu.semte.ch/wp-content/uploads/2017/07/mu-proxy-1024x768.png)

But we also want to have the convenience of having the docker composition, the complete application, there. The wiring can be complex, so can the logic, so mocking this entirely is in most cases unfeasable.

Therefor we use, for these use cases, a proxy that proxies the calls that would be made to your microservice in the docker-composition (dispatcher by the dispatcher to “http://your-microservice:port/route”) to your localhost.

## how to
Suppose we have a docker-compose.yml file that somewhere between it’s lines has the following:
```yaml
    # ...
    - ./config/resources:/config
YOUR_MICROSERVICE:
  image: your_microservice:1.0
  ports:
    - "3000:3000"
  volumes:
    - ./config/your-microservice:/config
```

Then when running in the complete application your microservice would receive it’s real calls on port 3000 within its container. To have this redirect to your localhost we use just another instance of the dispatcher image, changing the configuration to:
```yaml
    # ...
    - ./config/resources:/config
YOUR_MICROSERVICE:
    image: semtech/mu-dispatcher:1.0.1
    volumes:
      - ./config/localhost_dispatcher:/config
    ports:
      - "3000:80"
```

Just as the standard [mu.semte.ch](http://mu.semte.ch/) dispatcher the localhost dispatcher has a `dispatcher.ex` file. Here you would find something along the lines of:
```ex
defmodule Dispatcher do

  use Plug.Router

  def start(_argv) do
    port = 80
    IO.puts "Starting Plug with Cowboy on port #{port}"
    Plug.Adapters.Cowboy.http __MODULE__, [], port: port
    :timer.sleep(:infinity)
  end

  plug Plug.Logger
  plug :match
  plug :dispatch

  match "/*path" do
    Proxy.forward conn, path, "http://[YOUR LOCALHOST'S IP]:5678/"
  end
end
```

As you can see this will forward any incoming call on port 80, which is in the `docker-compose.yml` file mapped to port 3000 ensuring that this dispatcher will serve (as an underboundary) the calls that your microservice would need to support.

A second observation would be the exposed port on your localhost, that can of course not be 3000 in this setup because we already fire a docker on that port.

## tada
To make this work you only have to fire up whatever you want to test locally on port 5678 and TADA you can dev on your local machine with your local tools as if you were running everything in a docker and in a docker-compose.yml composition.

Of course sane thing to do here is to use emacs for development. The terminal version can be installed in a docker and you can dev by exec-ing into it :).

*This document has been adapted from Jonathan Langens' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/07/20/develop-with-your-local-ide-and-tools-inside-a-docker-container/)*
