# Template requirements

## PoC (proof of concept) requirements
-   Respond to HTTP requests
    -   Parse JSON & JSONAPI payloads
    -   Support implementation of GET PUT PATCH POST DELETE
    -   On port 80 during production

-   Send out SPARQL queries & updates over HTTP
    -   Perform JSON request to SPARQL HTTP endpoint
    -   Parse JSON query response
    -   Support Virtuoso Open Source [[1]](#1)

-   Generate HTTP response
    -   Build JSON & JSONAPI body
    -   Set outgoing HTTP headers

-   User-code
    -   Load user-code from `/app`
    -   Download & install dependencies on startup

## Full template requirements

-   [**All requirements for the PoC**](#poc-proof-of-concept-requirements)

-   Configuration
    -   Access environment variables [[2]](#notes)
    -   Support live-reload for development [[3]](#notes)

-   Respond to HTTP requests
    -   Parse incoming HTTP headers
    -   Destructure request URL
    -   Parse query parameters [[4]](#notes)

-   Send out SPARQL queries & updates over HTTP
    -   Set headers of the SPARQL HTTP request [[5]](#notes)
    -   Provide escaping methods for SPARQL input

-   User-code
    -   Load user configuration code from /config
    -   Use ONBUILD to automate template extension [[6]](#notes)
    -   Support the generation of UUIDs, namely uuid-v1

-   Enhancing workflow
    -   Use /template-name/setup.sh for downloading all dependencies
        > into the image[[7]](#notes)
    -   Use /template-name/startup.sh for launching the webserver.
    -   Use the ENVIRONMENT variable to enable development mode &
        > live-reload[[8]](#notes)
        -   Debugging console (if possible)
        -   Enable debugging symbols (if possible)
        -   Enable error output (if configurable)

## Notes

1.  This constraint should be dropped in the distant future, but it is
    > our default choice at the time. If you don\'t do anything special,
    > this will be ok.

2.  Docker makes it easy to set POSIX environment variables. These are
    > the variables you\'d be able to access in a shell (\$PATH is a
    > good example). These should be accessible so services can override
    > your service.

3.  Live-reload does not exist on all systems, but you can almost always
    > fake it. Some languages need a restart (like nodejs and ruby), for
    > some others you should connect to the service and reload the code
    > through your editor (like Common Lisp with slime). This reloading
    > does not need to auto-reload the dependencies, as these rarely
    > change and thus warrant restarting the service.

4.  Query parameters is the content that goes behind the question mark
    > (?) in the URL. It constitutes of key-value pairs in which the
    > keys needn\'t be unique.

5.  This requirement comes forth from the work Jonathan Langens is
    > performing on Sharing Knowledge (link should be added to basic
    > ground work in the field).

6.  The ONBUILD statement of Docker allows you to execute commands when
    > someone extends your image. This means you can automatically copy
    > the relevant contents of the repository in your /app and your
    > /config. This further reduces the overhead of implementing a new
    > template.

7.  This construction makes it predictable how extensions of this
    > template should behave. If someone builds a template on top of
    > your template (inception-style), then it\'s easily understandable
    > what needs to be called.

8.  It may be that development mode can not be enabled using an
    > environment variable as it requires too many extra dependencies.
    > This is strongly advised, but it is an option. In that case, the
    > development image should be maintained in the same repository as
    > the template repository. The name of the development should be
    > semtech/\<template-name\>-dev
