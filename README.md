# ghcr.io/johnbcodes/message-db

Docker image for [Message DB](https://github.com/message-db/message-db), built on [`postgres:latest`](https://hub.docker.com/_/postgres).

## Usage

### Docker command line example

```bash
docker pull ghcr.io/johnbcodes/message-db:1.2.6
```

A tag for the `latest` version is also available.

Run it as a container (This does not use a [volume](https://docs.docker.com/storage/volumes/) so the data will NOT persist. It will also remove the container once the process is stopped): 

```bash
docker run --rm -it -p 5432:5432 -e POSTGRES_HOST_AUTH_METHOD=trust -e DATABASE_NAME=mydb ghcr.io/johnbcodes/message-db:1.2.6`
```

### Docker Compose example
Here's a barebones `docker-compose.yml` for an app that uses Message DB:

```yml
version: '3.8'
services:
  app:
    build: .
    depends_on:
      - message_db
    environment:
      - MESSAGE_STORE_URI=postgresql://message_store@message_db:5432/message_store

  message_db:
    image: ghcr.io/johnbcodes/message-db:1.2.6
    environment:
      POSTGRES_HOST_AUTH_METHOD: trust
    expose:
      - 5432
    volumes:
      - data-postgresql:/var/lib/postgresql/data

volumes:
  data-postgresql:
```

## Environment variables

As shown above, the image supports all environment variables that the [PostgreSQL Docker image supports](https://github.com/docker-library/docs/blob/master/postgres/README.md).

It also supports the Message DB install script environment variables for [DATABASE_NAME](http://docs.eventide-project.org/user-guide/message-db/install.html#database-name) and [CREATE_DATABASE](http://docs.eventide-project.org/user-guide/message-db/install.html#disable-database-creation) the first time that PostgreSQL is initialized.

## Updates

I will attempt to add images for new Message DB versions as they arrive. Otherwise, please submit an issue.

## Acknowledgments

Message DB was created by the [Eventide Project](https://eventide-project.org/).

The Dockerfile, PostgreSQL image entry point for Message DB and original documentation was created by [Articulate](https://articulate.com/).

The Github Action CI, publishing of the ghcr.io image, and additional documentation was created by myself.
