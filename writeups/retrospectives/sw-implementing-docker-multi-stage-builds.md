# Docker multi-stage builds benefits (Publishing an EmberJS app using Docker multi-stage builds)
![](http://mu.semte.ch/wp-content/uploads/2017/05/mu_semte_ch_of_oz-1024x683.png)

In [the ember-proxy-service repo](https://github.com/mu-semtech/ember-proxy-service#tutorial-hosting-an-emberjs-app-with-a-backend-api) we explained how to host an EmberJS application in nginx with a backend API.  First, we had to build the EmberJS app through ember-cli’s build command:
```bash
ember build -prod
```

Next, we copied the resulting `dist/` folder into the nginx Docker image before building it. This process is cumbersome since it requires two manual steps to publish an EmberJS Docker image.

It would be more clean if we could execute the ember build step during the Docker image build. However, this requires that (a) the ember build command is available and (b) all our source files and dependencies (e.g. the node_modules folder) are copied inside the Docker container. As a consequence the resulting image would be much larger since it contains a lot more than just the built javascript assets from the dist/ folder.

To tackle this problem Docker introduced the concept of [multi-stage builds in Docker 17.05.](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)  Multi-stage builds allow to use multiple FROM statements in your Dockerfile. Each FROM statement begins a new stage of the build starting from the specified base image. The base images may differ per FROM statement. Artifacts can be copied from one stage to another. Everything else will be left behind when moving to a next stage. Therefore the resulting image will be much smaller. It only contains the image built in the final stage and the artifacts copied from previous stages. Have a look at the [Docker documentation](https://docs.docker.com/engine/userguide/eng-image/multistage-build/#before-multi-stage-builds) for a more in-depth explanation.

Our case would benefit from a 2-stage build:

1.  Building the EmberJS assets
2.  Building the nginx image to host the EmberJS app

This would result in the following Dockerfile:
```Dockerfile
# Stage 1 to build the EmberJS assets
FROM madnificent/ember:2.15.0 as builder

MAINTAINER Erika Pauwels <[\[email protected\]](/cdn-cgi/l/email-protection)\>

WORKDIR /app
COPY package.json .  # postpone cache invalidation 
RUN npm install
COPY . .
RUN ember build -prod
# End stage 1

# Stage 2 to build the nginx with our EmberJS app
FROM semtech/ember-proxy-service:1.1.0

COPY --from=builder /app/dist /app
# End stage 2
```

Docker multi-stage builds are a simple, yet powerful concept to fully automate application build processes while keeping the resulting Docker image small. In our case we were able to reduce the image size from 1.24GB to 185MB. Note however that multi-stage builds don’t work on Docker Hub, which is deprecated in favor of Docker Cloud.

*This document has been adapted from Erika Pauwels' mu.semte.ch article. You can view it [here](https://mu.semte.ch/2017/11/02/publishing-an-emberjs-app-using-docker-multi-stage-builds/)*
