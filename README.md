# Janus Docker

Setup your own janus server locally or on production environment hassle free. this repo contains both `Dockerfile` and `docker-compose.yaml` for building as well as deployment of janus server.

# Recommended way

Use `Janus` folder to build or deploy janus gateway using docker-compose
it uses pre-build core image which comes with pre-installed janus gateway dependencies cuts down lot of compile time since nobody's got time to compile and wait for hours
you can find `JanusCoreDeps` docker image [here](https://hub.docker.com/repository/docker/shivanshtalwar0/januscoredeps/tags)
or can build your own if you got lot of time in this world. by using [JanusCoreDeps](./JanusCoreDeps/) folder it outputs docker image containing just janus peer dependencies


# QuickStart

    cd Janus
    docker-compose up -d # note option -d to start it in detached mode

# Build

    cd Janus
    docker build -t dockerusername/janusgateway:latest .
    docker run -d dockerusername/janusgateway:latest
  
