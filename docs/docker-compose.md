# docker-compose

References:

* Tutorial: [Get started with Docker Compose](https://docs.docker.com/compose/gettingstarted/)
* Kitchen Sink: [Illustrative example](https://docs.docker.com/compose/compose-file/#illustrative-example)
* Volumes & Bind Mounts: [Manage data in Docker](https://docs.docker.com/storage/)
* Env Vars: [Environment variables in Compose](https://docs.docker.com/compose/environment-variables/)

## What's a sample docker-compose file look like? Say I want 2 services: web and redis, where redis uses an image from DockerHub and web is built using a local dockerfile and exposed on port 8000 with application port 5000.

```yaml
version: "3.9"
services:
  # Service "web"
  web:
    # Build Dockerfile in current dir
    build: .
    # Expose app port 5000 on machine as 8000
    ports:
      - "8000:5000"
    # mount the project directory (current directory) on the host to /code inside the container
    # modify the code on the fly, without having to rebuild the image.
    volumes:
      - .:/code
    # tell flask run to run in development mode and reload the code on change
    environment:
      FLASK_ENV: development
  # Service "redis" from image redis:alpine
  redis:
    image: "redis:alpine"
```

## How can I start my services in the background?

```bash
docker compose up -d
```

## How can I see what environment variables are available to the $SERVICE service defined in my docker-compose file?

```bash
SERVICE=web
docker compose run $SERVICE env
```

Note how this creates a container and then exists
```
$ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS                     PORTS     NAMES
0352eb7204cf   docker-compose_web   "env"                    2 seconds ago    Exited (0) 1 second ago              docker-compose_web_run_cf43592bca36
567aa67651ab   docker-compose_web   "env"                    3 minutes ago    Exited (0) 3 minutes ago             docker-compose_web_run_a506a07db24
```

## How can I stop services running in the background?

```
docker compose stop
```

## How can I bring everything down, removing the containers entirely? What if I also want to remove the data volume?

```
docker compose down
```
And to also remove the data volume:
```bash
docker compose down --volumes
```

## Will stopping (ctrl+C or `docker compose stop`) remove containers?

No, the containers just exit. `docker compose down` will remove the containers. The stopped container is why the counter will remain the same when runing `docker compose up` in a loop while stopping it each time, the data remains the redis container.

## How can I get a full list of compose commands?

```bash
docker compose --help
```

Output:
```
...
Commands:
  build       Build or rebuild services
  convert     Converts the compose file to platform's canonical format
...
```

## By default, when a file is created inside a container, where is it stored?

On a writable container layer

## Whate are the two options for containers to store files on the host machine, so that the files are persisted even after the container stops?

volumes, and bind mounts

## What are the three options for storing data in docker?

volumes, bind mounts, & tmpfs (memory)

## Where are Docker volumes stored?

Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. 

Volumes are the best way to persist data in Docker.

## Where are Docker bind mounts stored?

anywhere on the host system. 

They may even be important system files or directories. 

Non-Docker processes on the Docker host or a Docker container can modify them at any time.

## Where are tmpfs mounts stored?

In the host system’s memory only, and are never written to the host system’s filesystem.

## How can I explictly create a docker volume? How can I remove unused volumes?

```
docker volume create
```
Remove unused volumes
```
docker volume prune
```

## What is the usecase for tmpfs data?

Since it is used by a container during the lifetime of the container, to store non-persistent state or sensitive information.

## What type of storage should I use if I want to share data among multiple running containers?

Volumes

> Sharing data among multiple running containers. If you don’t explicitly create it, a volume is created the first time it is mounted into a container. When that container stops or is removed, the volume still exists. Multiple containers can mount the same volume simultaneously, either read-write or read-only. Volumes are only removed when you explicitly remove them.

## What type of storage should I use when the Docker host is not guaranteed to have a given directory or file structure?

Volumes

> When you want to store your container’s data on a remote host or a cloud provider, rather than locally.

## What time of storage should I use to back up, restore, or migrate data from one Docker host to another?

Volumes

> When you need to back up, restore, or migrate data from one Docker host to another, volumes are a better choice. You can stop containers using the volume, then back up the volume’s directory (such as /var/lib/docker/volumes/<volume-name>).

## What type of storage should I use when my application requires high-performance I/O on Docker Desktop?

Volumes

> When your application requires high-performance I/O on Docker Desktop. Volumes are stored in the Linux VM rather than the host, which means that the reads and writes have much lower latency and higher throughput.

## What type of storage should I use when my application requires fully native file system behavior on Docker Desktop?

Volumes

> When your application requires fully native file system behavior on Docker Desktop. For example, a database engine requires precise control over disk flushing to guarantee transaction durability. Volumes are stored in the Linux VM and can make these guarantees, whereas bind mounts are remoted to macOS or Windows, where the file systems behave slightly differently.

## What type of storage should I use when sharing configuration files from the host machine to containers?

Bind mounts

> Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting /etc/resolv.conf from the host machine into each container.

## What type of storage should I use when sharing source code or build artifacts between a development environment on the Docker host and a container

Bind mounts

> Sharing source code or build artifacts between a development environment on the Docker host and a container. For instance, you may mount a Maven target/ directory into a container, and each time you build the Maven project on the Docker host, the container gets access to the rebuilt artifacts.

> If you use Docker for development this way, your production Dockerfile would copy the production-ready artifacts directly into the image, rather than relying on a bind mount.

## What type of storage should I use when the file or directory structure of the Docker host is guaranteed to be consistent with the bind mounts the containers require?

Bind mounts

> When the file or directory structure of the Docker host is guaranteed to be consistent with the bind mounts the containers require.

## If I start a container with a volume that does not exist yet, what happens?

Docker creates the volume for you

## What command should I run to run a container using image `nginx:latest` named `devtest`, mount a volume named `myvol` on `/app`? Additionally, how can I verify the volume was created?

```bash
# Run a container in the background
docker run -d \
  # Name it devtest
  --name devtest \
  # Mount source as myvol and specify the target
  --mount source=myvol2,target=/app \
  # Image to create instance
  nginx:latest
```

You can inspect the container and review the `.Mounts`
```
docker inspect devtest
```

## Write a docker-compose file that has a single service `frontend` that uses image `node:lts` and has a volume called `myapp` that is mounted ate `/home/node/app`

```yaml
version: "3.9"
services:
  # Service named frontend
  frontend:
    # Specify Image
    image: node:lts
    # Volumes specifies $NAME:$PATH
    volumes:
      - myapp:/home/node/app
volumes:
  # Create empty volume myapp
  myapp:
```

## Say I've created a volume outside of docker compose. How can I use it in docker compose?

Specify `external: true`
```yaml
volumes:
  myapp:
    external: true
```

## Give an example of how I can change the image tag docker-compose file with an environment variable

```yaml
web:
  image: "webapp:${TAG}"
```

## If I have many environment variables, how can I pass them with docker compose?

Use a `.env` file with defaults or pass with `--env-file`

## In my docker-compose file, how can I be sure to throw an error if the environment variable is not defined?

Unset or empty:
```
${VARIABLE:?err}
```
If unset:
```
${VARIABLE:err}
```

## Give an example of how I can set an environment variable `DEBUG` to `1` in a container with docker-compose

```yaml
web:
  environment:
    - DEBUG=1
```

## Give an example of how I can set an environment variable `DEBUG` from my shell in a container with docker-compose

```yaml
web:
  environment:
    - DEBUG
```
