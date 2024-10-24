---
title: "Postgres Basics"
description: ""
summary: "Basic usage of Postgres, connecting to a database, and basic SQL queries."
date: 2023-01-04T20:59:23-08:00
lastmod: 2023-01-04T20:59:23-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "postgres-basics-9fa53406615e106aaebad570aec7551e"
weight: 1
toc: true
---

<!-- # Postgres -->

Postgres is an open source relational database manager.

- [Postgres Convetions](#postgres-conventions)
  - [Postgres connection stting](#postgres-connection-string)
  - [Case sentitive column names](#case-sensitive-column-names)
- [PSQL CLI](#psql-cli)
  - [Logging into psql](#logging-into-psql)
  - [Basic psql commands](#basic-psql-commands)
  - [Using psql wtih schemas](#using-psql-with-schemas)
- [Postgres flavored SQL](#postgres-flavored-sql)
  - [Creating tables](#creating-tables)
  - [Drop table](drop-table)
  - [Alter table and delete rows](#alter-table-and-delete-rows)
  - [Limit and offset](#limit-and-offset)
  - [Enums](#enums)
  - [Schemas](#schemas)
- [Postgres data types](#postgres-data-types)

## Postgres Conventions

### Postgres connection string

```
# basic example
postgres://USERNAME:PASSWORD@DATABASE_URL:5432/DATABSE_NAME

# AWS database
postgres://postgres@some-database.us-east-1.amazonaws.com:5432/secret_database

# bad example - password in connection string is security risk
postgres://postgres:exposed_password@10.0.0.5:5432/secret_database
```

#### Unsecure method

{{< callout icon="❌" text="Using a password in a connection string is unsecure." />}}

You can alternately pass in the password using `USERNAME:PASSWORD` but this is considered unsecure. The password should be omtted from the connection sting, which will then prompt the user to enter it.

### Case sensitive column names

Using case sensitive column names in Postgres is optional. By default, all column names are lower case.

{{< callout icon="💡" text="Use double quotes for case sensitive column names" />}}

Use `"` double quotes around column names when creating tables for case sentitive columns. The double quotes must be used whenever referencing the column in a SQL statement.

```sql
CREATE TABLE people (
  last_name     VARCHAR(64),
  "firstName"   VARCHAR(64)
);
```

The advantage is the column names can be formatted in camel case, meaning one less transformation before returning it to the frontend. The disadvantage is the double quotes must be used on the column name in every SQL statement.

### Database Schemas

> [Using schemas](#schemas)

A Postgres contains multiple databases. A database contains multiple schemas. Schemas in turn contain tables. Schemas are a way to segment a single database and allow multiple user to use the same database without stepping on each other.

By default talbes use the public schema, which does not have to be referenced in SQL queries.

## Postgres flavored SQL

Postgres implements the SQL language as expected for the most part. Postgres layers on additional datatypes and functions.

### Creating tables

> [Postgres datatype reference](#postgres-data-types)

```sql
-- Create a sequence to be used in a table (e.g., id field)
CREATE SEQUENCE box_sequence
    start 2000
    increment 1;

CREATE TABLE boxes (
    -- sequence reference uses 'single quotes'
    "boxId"           INTEGER NOT NULL PRIMARY KEY DEFAULT nextval('box_sequence'),
    -- column names use "double quotes"
    "boxName"         VARCHAR(32),
    -- lower case column names don't need quotes
    notes             VARCHAR(128)
    fixed_text        CHAR(32)
    long_text         TEXT,
    can_delete        BOOLEAN,
    price             FLOAT(2),
    metadata          JSONB,
    id_code           UUID,
    create_time       TIME,
    create_date       DATE,
    created_on        TIMESTAMP,
    with_timezone     TIMESTAMP WITH TIME ZONE
);

CREATE TABLE items (
    -- SERIAL is auto-incremented auto-populated integer
    "itemId"            SERIAL PRIMARY KEY,
    -- foreign key reference to another table
    "box"             INTEGER REFERENCES boxes ("boxId"),
);
```

- `SERIAL` is an auto-incrementing, auto-populating type. It starts at one and is automatically add when the record is created. It's handy for a primary key ID when you're not concerned about the value.

### Drop table

Delete a table, handle errors, handle object dependencies.

```sql
-- drop (delete) a table
DROP TABLE table_name;

-- prevent SQL error if talbe doesn't exist
DROP TABLE IF EXISTS table_name;

-- drop table and dependant objects in foreign table
DROP TABLE table_name CASCADE;
```

### Alter table and delete rows

Add columns, change column data types, delete columns.

```sql
ALTER TABLE items
ADD "itemDescription" VARCHAR(64);

ALTER TABLE items
ALTER COLUMN "lastMoved" TYPE TIMESTAMP WITH TIME ZONE;

ALTER TABLE
DROP COLUMN description;
```

### Insert, Update, and Delete rows

Insert new rows, update existing rows, and delete rows.

```sql
-- INSERT (strings use 'single quotes')
INSERT INTO users ("userId", notes)
VALUES (500, 'Greg Benish');

-- UPDATE ROW(S)
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;

-- DELETE ROW(S)
DELETE FROM table_name
WHERE condition;
```

### Limit and Offset

Limit number of returned rows, skip a number of rows before returning the retult set.

```sql
-- Limiting the rows returned
SELECT *
FROM users
ORDER BY create_date
LIMIT 200;

-- Skip a number of rows before returning limit
SELECT *
FROM users
LIMIT 200
OFFSET 20;
```

### Enums

Create predefined values for a column.

```sql
-- Create an enum
CREATE TYPE item_category AS ENUM ('Electronic', 'Camping', 'Other');

-- Update existing enum
ALTER TYPE item_category ADD VALUE 'None';

-- Use as column type
ALTER TABLE items
ADD "itemCategory" ITEM_CATEGORY;

-- List enum values
\dT+ ENUM_NAME
\dT+ item_category
```

### Array Datatypes

Column data types can be arrays

```sql
ALTER TABLE items
ADD "itemList" TEXT[];

INSERT INTO items ("itemList")
VALUES ('{foo,bar}');
```

### Schemas

Schemas are effectively namespaces that help segment data within a Postgres database.

```sql
CREATE SCHEMA some_schema;

CREATE TABLE some_schema.some_table (
    ...
);

SELECT *
FROM some_schema.some_table;
```

### PSQL Tid Bits

Find rows with duplicate records in `itinerary`, e.g., pointing to a foreign key. Matching records are combined into a JSON array. Returns each row with a unique value for `itinerary` and a combined JSON array of matching values.

```sql
SELECT s."itinerary", jsonb_agg(s) AS segments
      FROM segments s
      GROUP BY s."itinerary"
```

```

itinerary | segments
__________|___
1         | [{ itinerary: 1, stops: 3 }, { itinerary: 1, stops: 6 }]
2         | [{ itinerary: 2, stops: 7 }]
```

````

## Postgres Data Types

| Type          | Code                        | Desc.                                                                                                    |
| ------------- | --------------------------- | -------------------------------------------------------------------------------------------------------- |
| Boolean       | `BOOLEAN` or `BOOL`         | `0`, `true`, `t`, `yes`, `y` evaluates to `true`. <br>`1`, `false`, `f`, `no`, `n` evaluates to `false`. |
| Charachter    | `CHAR(n)`                   | Fixed length string. Strings less than `n` are padded with spaces.                                       |
| Charachter    | `VARCHAR(n)`                | Variable length string. Limited to up to `n` charachters.                                                |
| Charachter    | `VARCHAR`                   | Variable length string. Unlimited charachters.                                                           |
| Charachter    | `TEXT`                      | Variable length string. Unlimited length.                                                                |
| Small Integer | `SMALLINT`                  | 2-byte signed integer from -32,768 to 32,767                                                             |
| Integer       | `INT` or `INTEGER`          | 4-byte singed integer from -2,147,483,648 to 2,147,483,647                                               |
| Serial        | `SERIAL`                    | Similar to integer, but Postgres will automatically generate and populate the value.                     |
| Float         | `FLOAT(n)`                  | Floating-point number with precision of `n`. Maximum of 8 bytes.                                         |
| Float         | `real` or `float8`          | 4-byte floating point number                                                                             |
| Float         | `numeric` or `numeric(p,s)` | Real number with `p` digits and `s` number after the decimal place.                                      |
| Date          | `DATE`                      | Date only.                                                                                               |
| Date          | `TIME`                      | Time only. (without timezone)                                                                            |
| Date          | `TIME WITH TIMEZONE`        | Time only. Includes timezone.                                                                            |
| Date          | `TIMESTAMP`                 | Time and date.                                                                                           |
| Date          | `TIMESTAMPZ`                | Time and date with timezone.                                                                             |
| Date          | `INTERVAL`                  | Periods of time.                                                                                         |
| UUID          | `UUID`                      | RFC 4122 compliant UUIDs.                                                                                |
| JSON          | `JSON`                      | Plain JSON types that require for each processing.                                                       |
| JSON          | `JSONB`                     | Plain JSON types that are faster to inster but slower to insert. Supports indexing.                      |
| Array         | `???`                       | Plain JSON types that are faster to inster but slower to insert. Supports indexing.                      |

# Assorted Troubleshooting

## Delete `postmaster.pid` on certain hard restarts

{{< callout icon="⚠️" text="Do not blindly delete the `postmaster.pid` file! This can cause data corruption and loss" />}}

The `postmaster.pid` file points to the process ID running Postgres.

If the computer crashes the `postmaster.pid` may not get cleaned up and point to an old process Id. The existance of this file will prevent Postgres from starting again and may need to be deleted if the process it's referencing is confirmed to be outdated.

### Check Porcess ID

```bash
# inspect postmaster.pid
# this location may be different
cat /opt/homebrew/var/postgres/postmaster.pid

# check processes with that process id
lsof | grep PROCESS_ID
lsof | grep 1023
````

The top line of the `postmaster.pid` file should refer to the process id. Use `lsof` and `grep` to check that process id.

No processes may be associated with that process id, confirming the process id is outdated.

If processes return, verify they are unrelated to Postgres. The process id may have been reassigned to a new, unrelated process, confirming the `postmaster.pid` file is outdate and can be deleted.

# Running Postgres

## Commands

```bash
# check if Postgres is running
ps auxwww | grep postgres

```

## Install Location

Install location with Homebrew on new M1 Macs: `/opt/homebrew/var/postgres`.

Otherwise may be located in `/usr/local/var/postgres`.

## Log Location

New Homebrew installations in M1 Macs: `/opt/homebrew/var/log/postgres.log`
