# mu.semte.ch primer (About)
Microservices are a good idea, if only they were easy to understand and integrate.  The answer is here, mu.semte.ch.  Your framework for building microservices that are easy to install, integrate, and reuse.  All microservices are reusable in the backend, the frontend integrates everything into a unified look-and-feel.

## How do we do it?
We cheat.  Instead of letting microservices depend on each other, we lean on the semantic web for data integration.  The semantic web, as envisioned by the W3C, allows computers from all over the world to form one gigantic database.  All without really knowing about each other.  As that looks like a nice feature for microservices, we used our extensive knowledge on semantic technologies to let microservices interoperate using these models.  Only using the database as a syncing point.

## Deployment?
We tackle that using Docker and Docker Compose.  It’s simple to understand, and trivial to get running.  We use the docker-compose.yml file to describe the main topology, containing all microservices in your stack.  With a single command, you can download all the microservices of your stack, and start them up.  This makes deployment a fun and easy task.

## Reuse?
Many backend services are easy to reuse.  We have services available for login, registration, regular resource listing and creation (mu-cl-resources), migrations, exports, …  But the real gem lies in how to roll your own.  We have templates for various languages, like JavaScript and Ruby, making it trivial to build your own microservice.

## Integration?
The look and feel of an application is important.  It needs to look nice and integrated, but it also needs to be cheap.  Mass-production, but custom.  We integrate all microservices in a Single Page App.  We standardize on EmberJS, the most stable choice for frontend development and reuse.  Its structure makes it easy for us to reuse know-how in disconnected projects.  It also allows us to share frontend logic, and sometimes visualisation, using addons.  In the past three years, it has been the logical step forward.

## Sustainability?
After open sourcing our hobby project, various entities have contributed to mu.semte.ch.   As such, we are being used at the European Commission, in various research institutes, at the Flemish government, and even by some SMEs.  We are actively working together with each of them, to ensure the platform is used optimally, and in line with our future vision.
