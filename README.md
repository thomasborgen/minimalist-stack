# Minimalist Stack

> The minimalist stack aims to be as simple as possible, running in one container deployable anywhere. We manage to do that by using [fastAPI](https://fastapi.tiangolo.com/) and [SQLite](https://www.sqlite.org/). SQLite is backed up by [Litestream](https://litestream.io/) that streams all changes to cloud storage and also restores when the app starts up. [Alembic](https://alembic.sqlalchemy.org/en/latest/) is used for migrations. For data/database models we use [SQLModel](https://sqlmodel.tiangolo.com/) and we create HTML with [Hypermedia](https://github.com/thomasborgen/hypermedia) and use [htmx](https://htmx.org/) to make the website dynamic. For icons we use material icons and I have not landed what we do with CSS yet.


# Running locally

```sh
poetry run uvicorn server.main:app --reload
```

Then navigate to

`http://locahost:8000/`


# Using this stack

## Hypermedia

You can use whatever you want for rendering html. Hypermedia lets you write html in very basic python. Many people like Jinja2, but you loose type safety and autocompletion and have to learn how Jinja2 works. Hypermedia is basically like writing Html with python classes instead.

## SQLModel

## Migrations - Alembic

## Litestream

Litestream runs in our container and makes sure that any changes we do to our SQLite database is persisted to the configured cloud storage.

To use Litestream you will need to create cloud storage with one of the [supported providers](https://litestream.io/guides/#replica-guides).


### Using Litestream with GCP:

First create a bucket

Then download your keyfile and save it as key.json and point to it with env var:

```sh
gcloud iam service-accounts keys create key.json --iam-account your_service_account@your_project.iam.gserviceaccount.com
```

```sh
export GOOGLE_APPLICATION_CREDENTIALS='key.json'
```

### Using Litestream

First configure the `.envrc` with litestream replica type, name and path.

Then run the application and backup changes with

`litestream replicate --config litestream.yml`

To restore a backup to a local file

`litestream restore --config litestream.yml database.db`

or directly.

`litestream restore -o my.db gcs://BUCKET/PATH`


### Env variables locally

You should have something that reads the envrc file and populates the environment variables of you current shell. For example [Direnv](https://direnv.net/).
