# docker-activemq

[![Build and Push Docker Images](https://github.com/filviu/docker-activemq/actions/workflows/docker-build.yml/badge.svg)](https://github.com/filviu/docker-activemq/actions/workflows/docker-build.yml)
[![Docker Pulls](https://img.shields.io/docker/pulls/filviu/activemq.svg?maxAge=2592000)](https://hub.docker.com/r/filviu/activemq/)

[Docker](https://www.docker.io/) file for trusted builds of [ActiveMQ](http://activemq.apache.org/) on https://registry.hub.docker.com/r/filviu/activemq.

Run the latest container with:

    docker pull filviu/activemq
    docker run -p 61616:61616 -p 8161:8161 filviu/activemq

The JMX broker listens on port 61616 and the Web Console on port 8161.

## Image Tags

    filviu/activemq:5.10.0
    filviu/activemq:5.10.1
    filviu/activemq:5.10.2
    filviu/activemq:5.11.0
    filviu/activemq:5.11.1
    filviu/activemq:5.12.0
    filviu/activemq:5.12.1
    filviu/activemq:5.12.2
    filviu/activemq:5.13.0
    filviu/activemq:5.13.1
    filviu/activemq:5.13.2
    filviu/activemq:5.13.3
    filviu/activemq:5.13.4
    filviu/activemq:5.14.0
    filviu/activemq:5.14.0-alpine
    filviu/activemq:5.14.1
    filviu/activemq:5.14.1-alpine
    filviu/activemq:5.14.2
    filviu/activemq:5.14.2-alpine
    filviu/activemq:5.14.3
    filviu/activemq:5.14.3-alpine
    filviu/activemq:5.14.4
    filviu/activemq:5.14.4-alpine
    filviu/activemq:5.14.5
    filviu/activemq:5.14.5-alpine
    filviu/activemq:5.15.2
    filviu/activemq:5.15.2-alpine
    filviu/activemq:5.15.3
    filviu/activemq:5.15.3-alpine
    filviu/activemq:5.15.4
    filviu/activemq:5.15.4-alpine
    filviu/activemq:5.15.5
    filviu/activemq:5.15.5-alpine
    filviu/activemq:5.15.6
    filviu/activemq:5.15.6-alpine
    filviu/activemq:5.15.9
    filviu/activemq:5.15.9-alpine

No `latest` tag ? No, you shouldn't be using it anyway.

## Port Map

    61616 JMS
    8161  UI
    5672  AMQP  (since `filviu/activemq:5.12.1`)
    61613 STOMP (since `filviu/activemq:5.12.1`)
    1883  MQTT  (since `filviu/activemq:5.12.1`)
    61614 WS    (since `filviu/activemq:5.12.1`)

## Customizing configuration and persistence location

By default data and configuration is stored inside the container and will be
lost after the container has been shut down and removed. To persist these
files you can mount these directories to directories on your host system:

    docker run -p 61616:61616 -p 8161:8161 \
               -v /your/persistent/dir/conf:/opt/activemq/conf \
               -v /your/persistent/dir/data:/opt/activemq/data \
               filviu/activemq

ActiveMQ expects that some configuration files already exists, so they won't be
created automatically, instead you have to create them on your own before
starting the container. If you want to start with the default configuration you
can initialize your directories using some intermediate container:

    docker run --user root --rm -ti \
      -v /your/persistent/dir/conf:/mnt/conf \
      -v /your/persistent/dir/data:/mnt/data \
      filviu/activemq:5.15.4-alpine /bin/sh

This will bring up a shell, so you can just execute the following commands
inside this intermediate container to copy the default configuration to your
host directory:

    chown activemq:activemq /mnt/conf
    chown activemq:activemq /mnt/data
    cp -a /opt/activemq/conf/* /mnt/conf/
    cp -a /opt/activemq/data/* /mnt/data/
    exit

The last command will stop and remove the intermediate container. Your
directories are now initialized and you can run ActiveMQ as described above.

## Fork

Based on a lot of good work done by Roman Mohr, [here](https://github.com/filviu/docker-activemq).
