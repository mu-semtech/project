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
        into the image [[7]](#notes)
    -   Use /template-name/startup.sh for launching the webserver.
    -   Use the ENVIRONMENT variable to enable development mode &
        live-reload [[8]](#notes)
        -   Debugging console (if possible)
        -   Enable debugging symbols (if possible)
        -   Enable error output (if configurable)

## Helper functions
Ideally, your template should expose some helper functions, including
but not limited to the following. Extra parameters can be added as 
needed or wanted.


| Function name      | Parameters            | Description |
| ------------------ | --------------------- | ----------- |
| error              | title: `string`, http_error_code: `int` | Returns a JSONAPI compliant error response with the given status code (default: 400). |
| query              | query: `string`       | Executes the given SPARQL select/ask/construct query. [This function should automatically pass the correct headers](#passing-headers).  Also see `update`. |
| session_id_header  | request: `object`     | Get the session id from the HTTP request headers. |
| sparql_escape      | value: `any`          | Detects the value type and runs the appropiate [sparql escape function](#sparql_escape). |
| update             | query: `string`       | Executes the given SPARQL update query. [This function should automatically pass the correct headers](#passing-headers). Also see `query`. |
| uuid               | None                  | Generates an uuid. Meant for use in linked data (`mu:uuid`). | 

*Note: camelcase is used in this documentation. However, when writing the helpers in the language of your choice, use the notation that is in line with the language in question.*

### sparql_escape
The following helper functions all have the same description/funcionality, but for different types. As such, the description column is omitted.

**Description:** Converts the given object to a SPARQL-safe RDF object string with the right RDF-datatype.
This functions should be used especially when inserting user-input to avoid SPARQL-injection.

| Function name          | Parameters        |
| ---------------------- | ----------------- |
| sparql_escape_string   | value: `string`   |
| sparql_escape_uri      | value: `url`      |
| sparql_escape_decimal  | value: `decimal`  |
| sparql_escape_int      | value: `int`      |
| sparql_escape_float    | value: `float`    |
| sparql_escape_date     | value: `date`     |
| sparql_escape_datetime | value: `datetime` |
| sparql_escape_bool     | value: `bool`     |

### Passing headers
The following headers should be passed when making queries:

| Header name            | Type     | Description             |
| ---------------------- | -------- | ----------------------- |
| MU-SESSION-ID          |          |                         |
| MU-CALL-ID             |          |                         |
| MU-AUTH-ALLOWED-GROUPS |          | (bidirectional)         |
| MU-AUTH-USED-GROUPS    |          |                         |


## Notes

1.  This constraint should be dropped in the distant future, but it is
    our default choice at the time. If you don\'t do anything special,
    this will be ok.

2.  Docker makes it easy to set POSIX environment variables. These are
    the variables you\'d be able to access in a shell (`$PATH` is a
    good example). These should be accessible so services can override
    your service.

3.  Live-reload does not exist on all systems, but you can almost always
    fake it. Some languages need a restart (like nodejs and ruby), for
    some others you should connect to the service and reload the code
    through your editor (like Common Lisp with slime). This reloading
    does not need to auto-reload the dependencies, as these rarely
    change and thus warrant restarting the service.

4.  Query parameters is the content that goes behind the question mark
    (?) in the URL. It constitutes of key-value pairs in which the
    keys needn\'t be unique.

5.  This requirement comes forth from the work Jonathan Langens is
    performing on Sharing Knowledge (link should be added to basic
    ground work in the field).

6.  The ONBUILD statement of Docker allows you to execute commands when
    someone extends your image. This means you can automatically copy
    the relevant contents of the repository in your /app and your
    /config. This further reduces the overhead of implementing a new
    template.

7.  This construction makes it predictable how extensions of this
    template should behave. If someone builds a template on top of
    your template (inception-style), then it\'s easily understandable
    what needs to be called.

8.  It may be that development mode can not be enabled using an
    environment variable as it requires too many extra dependencies.
    This is strongly advised, but it is an option. In that case, the
    development image should be maintained in the same repository as
    the template repository. The name of the development should be
    `semtech/<template-name>-dev`
