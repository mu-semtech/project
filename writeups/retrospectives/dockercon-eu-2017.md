# mu.semte.ch at DockerCon EU
The mu.semte.ch stack relies on Docker as it perfectly suites a microservice architecture. We don’t only use it for deployment, but also for development. By using Docker we try to make our workflow as productive as possible. Since the Docker community is moving very fast, we attended DockerCon EU in Copenhagen last week to keep up to date with the latest Docker news.

![](http://mu.semte.ch/wp-content/uploads/2017/10/dockercon-eu.png)

The Docker team announced two major topics in the keynotes:

- [seamless integration of Kubernetes into the Docker platform](https://www.docker.com/kubernetes) (alongside Swarm)
- [expanded partnership for the Modernize Traditional Applications (MTA) program](https://goto.docker.com/rs/929-FJL-178/images/SB_MTA_04.14.2017.pdf) 

The video recordings of the keynotes are freely available on [Keynote Day 1](https://play.vidyard.com/h5fj14BB2Gkai1WHcbAyTv) and [Keynote Day 2](https://play.vidyard.com/wztT1ekFnTjDYLcYfJWALX).

Traditonal applications are typically developed as monoliths. With the MTA program Docker tries to support companies to upgrade their traditionally deployed apps to a more modern container infrastructure like Docker. Although the mu.semte.ch architecture builds on the concept of microservices, Docker’s MTA program may teach us useful lessons on the migration from monoliths to microservices. After all, projects don’t always start from scratch but often originate from an existing (monolith) solution.

Next to the keynote sessions there were also a lot of breakout sessions on a broad range of topics. All slides will soon become available on [Docker’s slideshare](https://www.slideshare.net/Docker). We will just highlight a few of the topics covered:


- Abby Fuller from AWS explained how to create effective Docker images covering topics like the recently introduced [multistage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Some of the mu.semte.ch templates and services will benefit from these tips to reduce the size of their Docker image.

- Adrian Mouat, one of the Docker captains, gave [a talk with some tips and tricks](https://www.slideshare.net/Docker/tips-and-tricks-of-the-docker-captains). Nothing new, no mind blowing stuff, but just some useful practical tips to make your day-to-day Docker usage more efficient.

- Dan Finneran, an ex-Docker captain that recently became a Docker employee, prestented [an overview of the Docker networking](https://www.slideshare.net/Docker/practical-design-patterns-in-docker-networking). This included the Swarm overlay networks and some practical design patterns the mu.semte.ch architecture might benefit from when deploying on multiple machines.

- Bret Fisher, another Docker captain, gave a _playful_ talk on [the deployment to production with Docker](https://d.pr/f/d7diCc/1AEjBc2m). He warned for bad practices like turning servers from cattles into pets and making Dockerfiles environment specific. For sure some lessons we should take into account when deploying a mu.semte.ch app.

- Riccardo Tommasini, a PhD Student from Milano, gave a short presentation in the Community Theatre on his ongoing research to empower Docker with Linked Data principles. One of his research results, a Docker ontology to represent Dockerfiles in RDF, will be [presented next week at ISWC 2017](https://iswc2017.semanticweb.org/paper-528/).


These are just a few highlights of all the topics covered at the conference. The video recordings of all presentations will soon become available on the Docker website. A lot of interesting material to learn from within the context of mu.semte.ch.

*This writeup has been adapted from Erika Pauwels' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/10/19/mu-semte-ch-at-dockercon-eu/)*
