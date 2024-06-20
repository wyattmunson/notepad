---
title: "PSQL CLI"
description: ""
lead: ""
date: 2023-01-04T21:01:31-08:00
lastmod: 2023-01-04T21:01:31-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "psql-cli-e1e5487ebe86b6da13e1b4b682cf6c3e"
weight: 999
toc: true
---

`psql` is a CLI front end for postgres. Use it to view and maintain databases or run SQL commands directly.

## Install PSQL

`psql` is not installed natively.

### MacOS

```bash
brew update
brew install libpq
# create symlink for psql and related tools to /usr/local/bin
brew link --force libpq
```

## Logging into `psql`

```bash
psql -h localhost -U postgres -p 5432

# login if you don't know any db name
psql -U postgres
# login if you know the db name (login as current user)
psql databse_name
# login to given database as given user
psql databse_name -U user_name

# all of the flags
psql -h hostname -U username -p port -d db_name

# use connection string to login
psql postgres://USERNAME:PASSWORD@DATABASE_URL:5432/DATABSE_NAME
psql postgres://some_user@localhost:5432/db_name
psql postgres://some_user:secret_pass@10.0.0.5:5432/db_name

# specify username and database when logging in
psql -U USERNAME DATABASE_NAME
```

Be default, when specifying a username, Postgres will attempt to connect a database with the same name. If the database does not exist, the command will fail saying `FAIL: database <USER> does not exist`.

In this case specify a database name using the command above. If you do not know the database names, log in using the `postgres` user first, with `psql -u postgres`.

### Basic `psql` commands

```bash
# LISTING DATABASES
# list databases
\l
# list database with additional info
\l+

# CONNECTING TO DATABASES
# connect to database 'users'
\c users
# see current connection (auto connects to postgres if not connected)
\c

# TABLES
# list tables
\dt
# list tables and sequences
\d
# list tables and sequences with additional info
\d+
# describe a table
\d orders
# describe table with add;t into
\d+ orders
# list a tables relations
\dt orders
# list schemas
\dn orders
\dn+ orders

# quit shell
\q

# open text editors
\e

# see users
\du

CREATE USER username;
```

### Using `psql` with schemas

```bash
# list schemas
\dn

# list table within a schema
\dt schema_name.<TABLE NAME>

# list all tables within a schema
\dt schema_name.*

# List all tables in all schemas
\dt *.*

# select data from a table in schema
SELECT * FROM schema_name.table_name;
```

## Homebrew Postgres

```bash
brew info postgres
brew info postgresql
brew services start postgresql@14
```
