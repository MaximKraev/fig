---
layout: default
title: Compose CLI reference
---

CLI reference
=============

Most commands are run against one or more services. If the service is omitted, it will apply to all services.

Run `docker-compose [COMMAND] --help` for full usage.

## Options

### --verbose

 Show more output

### --version

 Print version and exit

### -f, --file FILE

 Specify an alternate compose file (default: docker-compose.yml)

### -p, --project-name NAME

 Specify an alternate project name (default: directory name)

## Commands

### build

Build or rebuild services.

Services are built once and then tagged as `project_service`, e.g. `composetest_db`. If you change a service's `Dockerfile` or the contents of its build directory, you can run `docker-compose build` to rebuild it.

### help

Get help on a command.

### kill

Force stop running containers by sending a `SIGKILL` signal. Optionally the signal can be passed, for example:

    $ docker-compose kill -s SIGINT

### logs

View output from services.

### port

Print the public port for a port binding

### ps

List containers.

### pull

Pulls service images.

### rm

Remove stopped service containers.


### run

Run a one-off command on a service.

For example:

    $ docker-compose run web python manage.py shell

By default, linked services will be started, unless they are already running.

One-off commands are started in new containers with the same configuration as a normal container for that service, so volumes, links, etc will all be created as expected. The only thing different to a normal container is the command will be overridden with the one specified and by default no ports will be created in case they collide.

Links are also created between one-off commands and the other containers for that service so you can do stuff like this:

    $ docker-compose run db psql -h db -U docker

If you do not want linked containers to be started when running the one-off command, specify the `--no-deps` flag:

    $ docker-compose run --no-deps web python manage.py shell

If you want the service's ports to be created and mapped to the host, specify the `--service-ports` flag:
	$ docker-compose run --service-ports web python manage.py shell

### scale

Set number of containers to run for a service.

Numbers are specified in the form `service=num` as arguments.
For example:

    $ docker-compose scale web=2 worker=3

### start

Start existing containers for a service.

### stop

Stop running containers without removing them. They can be started again with `docker-compose start`.

### up

Build, (re)create, start and attach to containers for a service.

Linked services will be started, unless they are already running.

By default, `docker-compose up` will aggregate the output of each container, and when it exits, all containers will be stopped. If you run `docker-compose up -d`, it'll start the containers in the background and leave them running.

By default if there are existing containers for a service, `docker-compose up` will stop and recreate them (preserving mounted volumes with [volumes-from]), so that changes in `docker-compose.yml` are picked up. If you do no want containers to be stopped and recreated, use `docker-compose up --no-recreate`. This will still start any stopped containers, if needed.

[volumes-from]: http://docs.docker.io/en/latest/use/working_with_volumes/


## Environment Variables

Several environment variables can be used to configure Compose's behaviour.

Variables starting with `DOCKER_` are the same as those used to configure the Docker command-line client. If you're using boot2docker, `$(boot2docker shellinit)` will set them to their correct values.

### FIG\_PROJECT\_NAME

Set the project name, which is prepended to the name of every container started by Compose. Defaults to the `basename` of the current working directory.

### FIG\_FILE

Set the path to the `docker-compose.yml` to use. Defaults to `docker-compose.yml` in the current working directory.

### DOCKER\_HOST

Set the URL to the docker daemon. Defaults to `unix:///var/run/docker.sock`, as with the docker client.

### DOCKER\_TLS\_VERIFY

When set to anything other than an empty string, enables TLS communication with the daemon.

### DOCKER\_CERT\_PATH

Configure the path to the `ca.pem`, `cert.pem` and `key.pem` files used for TLS verification. Defaults to `~/.docker`.
