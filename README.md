# Cozy Nginx - Reverse proxy

This is a Nginx reverse proxy for Cozy Cloud. This docker image is designed to work out of the box
with other spiroid/cozy-* images. See the run with docker-compose section below.

It is configured to only work in HTTPS as it is recommended by cozy cloud developers. It provides a
self signed certificate that is used by default and is not bound to a specific domain name.

This image is based on the [official Nginx docker image](https://hub.docker.com/_/nginx/).


## Pull the image

```
$ docker pull spiroid/cozy-nginx
```


## Build it yourself

```
$ git clone git@github.com:spiroid/cozy-nginx.git
$ cd cozy-nginx
$ doker build -t spiroid/cozy-nginx .
```


## Run:

### With docker-compose

A sample docker-compose.yml configuration file with the complete stack :

```
configuration:
    image: spiroid/cozy-conf

couchdata:
    image: spiroid/cozy-couchdb-data

couchdb:
    image: spiroid/cozy-couchdb
    restart: always
    volumes_from:
        - couchdata
        - configuration

dataindexer:
    image: spiroid/cozy-data-indexer
    hostname: dataindexer
    restart: always
    volumes_from:
        - configuration

controller:
    image: spiroid/cozy-controller
    restart: always
    links:
        - couchdb
        - dataindexer
    volumes_from:
        - configuration
        - dataindexer

nginx:
    image: spiroid/cozy-nginx
    restart: always
    links:
        - controller
    ports:
        - "443:443"
```


Then, in the directory where you put the configuration file :

 * docker-compose up
 * init the cozy stack if you haven't already
 * visit https://localhost/

## Related images

This configuration image was created to work with the following images:

  * [cozy conf](https://github.com/spiroid/cozy-conf)
  * [cozy couchdb data](https://github.com/spiroid/cozy-couchdb-data)
  * [cozy couchdb](https://github.com/spiroid/cozy-couchdb)
  * [cozy data indexer](https://github.com/spiroid/cozy-data-indexer)
  * [cozy controller](https://github.com/spiroid/cozy-controller)


# Inspirations

 * https://forum.cozy.io/t/deployer-cozy-avec-docker-et-des-containers-autonomes/468
