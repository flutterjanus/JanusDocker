# Janus Docker
Get all `Dockerfile` here for building janusgateway image

# Recommended way
Use JanusMultistreamingCoreDeps folder to build or deploy janus gateway using docker-compose
it uses pre-build core image which comes with pre-installed janus gateway dependencies cuts down lot of compile time since nobody's got time to compile and wait for hours
you can find januscoredeps docker image [here](https://hub.docker.com/repository/docker/shivanshtalwar0/januscoredeps/tags)
or can build your own if you got lot of time in this world. by using JanusCoreDeps folder it outputs docker image containing just janus peer dependencies
by default this repo assumes you love and support unified plan so only includes example of multistream (a.k.a unified plan) branch you can replicate for master branch if you like.


# Build & Run using dockerfile
	cd JanusMultistreamingCoreDeps
	docker build -t dockerusername/imagename:tag .
	docker run -d dockerusername/imagename:tag
		
# docker compose
        cd JanusMultistreamingCoreDeps
	docker-compose up -d # note option -d to start it in detached mode
