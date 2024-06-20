---
title: "PSQL Reference"
description: ""
lead: ""
date: 2023-01-05T23:14:01-08:00
lastmod: 2023-01-05T23:14:01-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "psql-reference-291beae45c9c1bae7ede1e799fee1c09"
weight: 999
toc: true
---

## Postgres flavored SQL

Postgres implements the SQL language as expected for the most part. Postgres layers on additional datatypes and functions.

## Creating tables

> [Postgres datatype reference](#postgres-data-types)

### Basic Example

```sql
CREATE TABLE cars (
    car_id        INTEGER PRIMARY KEY SERIAL,
    manufacturer  INTEGER REFERENCES manufacturers(manufacturer_id)
    awd           BOOLEAN,
    description   VARCHAR(500),
    notes         VARCHAR,
    create_date   TIMESTAMP WITH TIMEZONE,
    update_date   TIMESTAMP,
);
```

The table above creates:

- A `car_id` primary key, using `SERIAL` to automatically generate and populate the primary key
- A `manufacturer` foreign key reference to the `manufacturer_id` column in the `manufacturers` table. It is stored as an `INTEGER` in the cars table, the same datatype as `manufacturer_id`.
- A `awd` boolean field that can store true, false, or null
- A `description` field with a fixed 500 charachter limit
- A `notes` field with no fixed charachter limit
- A `create_date` timestamp (time and date) including timezone information
- `update_date` - timestamp without timezone information

### Full Example

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

## Drop table

Delete a table, handle errors, handle object dependencies.

```sql
-- drop (delete) a table
DROP TABLE table_name;

-- prevent SQL error if talbe doesn't exist
DROP TABLE IF EXISTS table_name;

-- drop table and dependant objects in foreign table
DROP TABLE table_name CASCADE;
```

## Alter table and delete rows

Add columns, change column data types, delete columns.

```sql
ALTER TABLE items
ADD "itemDescription" VARCHAR(64);

ALTER TABLE items
ALTER COLUMN "lastMoved" TYPE TIMESTAMP WITH TIME ZONE;

ALTER TABLE
DROP COLUMN description;
```

## Insert, Update, and Delete rows

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

## Limit and Offset

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

## Enums

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

## Array Datatypes

Column data types can be arrays

```sql
ALTER TABLE items
ADD "itemList" TEXT[];

INSERT INTO items ("itemList")
VALUES ('{foo,bar}');
```

## Schemas

Schemas are effectively namespaces that help segment data within a Postgres database.

```sql
CREATE SCHEMA some_schema;

CREATE TABLE some_schema.some_table (
    ...
);

SELECT *
FROM some_schema.some_table;
```

## PSQL Tid Bits

Find rows with duplicate records in `itinerary`, e.g., pointing to a foreign key. Matching records are combined into a JSON array. Returns each row with a unique value for `itinerary` and a combined JSON array of matching values.

```sql
SELECT s."itinerary", jsonb_agg(s) AS segments
      FROM segments s
      GROUP BY s."itinerary"
```

Select distinct where set may be split between multiple columns.

```sql
select distinct a as ab
from
(select "originCity" a from itineraries
union all
select "destinationCity" from itineraries
) AS x;
```

```

itinerary | segments
__________|___
1         | [{ itinerary: 1, stops: 3 }, { itinerary: 1, stops: 6 }]
2         | [{ itinerary: 2, stops: 7 }]
```
