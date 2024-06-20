---
title: "Postgres Dataypes"
description: ""
lead: ""
date: 2023-01-05T10:56:23-08:00
lastmod: 2023-01-05T10:56:23-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "postgres-dataypes-3b501fc4e7fe7faa3d7773c4425ba8cc"
weight: 999
toc: true
---

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

## Boolean

Choosing boolean is easy because there's only one `BOOLEAN` dataype. Choose boolean or do not.

- Boolean can represent `TRUE`, `FALSE`, or `NULL`
  - `0`, `true`, `t`, `yes`, `y` evaluates to `true`
  - `1`, `false`, `f`, `no`, `n` evaluates to `false`

## String Data Types

|               |                                                            |
| ------------- | ---------------------------------------------------------- |
| **Datatypes** | `VARCHAR(n)`, `CHARACTER(n)`, `TEXT`                       |
| **Accepts**   | Text: `hello`, `Hello!!1!`, `2412`                         |
| **Fidelity**  | Stored exactly                                             |
| **Use**       | When storing numbers and letters                           |
| **Avoid**     | When storing numbers only                                  |
| **Which**     | `VARCHAR(n)` for fixed length, `TEXT` for unlimited length |

### VARCHAR vs CHAR vs TEXT

| Datatype     | Length                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------ |
| `VARCHAR(n)` | Variable length, with maximum length specified                                             |
| `VARCHAR`    | Variable length, unlimited size                                                            |
| `CHAR(n)`    | Fixed length, with maximum length specified, blank spaces padded to reach charachter limit |
| `CHAR`       | Equal to `CHAR(1)`                                                                         |
| `TEXT`       | Variable length, unlimited size (same as `VARCHAR`)                                        |

- `VARCHAR(n)` allows for a variable length string
  - `VARCHAR(n)` where `n` is a positive integer that sepcifies the maximum number of charachters (not bytes)
  - Inputs over the charachter length are rejected by the database
  - Inputs under the charachter length are added at the same length as the string
- `CHAR(n)` allows for a fixed length string
  - `CHAR(n)` where `n` is a positive integer that sepcifies the maximum number of charachters (not bytes)
  - Inputs over the charachter length are rejected by the database
  - Inputs under the charachter length are padded with blank spaces to reach the charachter limit, then stored and displayed that way
- `TEXT` allows for a variable length string with no charachter limit

General Notes:

- The longest possible charachter string that can be stored is about 1GB. If storing massive strings, omit the maximum charachter limit and use `VARCHAR` or `TEXT`.
- `VARCHAR(n)`, `CHAR(n)`, and `TEXT` are all stored the same way under the hood. Using `VARCHAR` over `TEXT` is a matter of preference; there is no functional difference between the two.

## Number Data Types

{{<callout icon="🎓" text="Precision is the total number of digits in a number. The number 432.33 has a precision of 5 and scale of 2." />}}

### Integers

Integer datatypes allow for storing of integers (whole numbers), with variable precision, and are stored exactly.

`SMALLINT`, `INTEGER`, `BIGINT`
Integers only: `-64`, `0`, `2214555746`
W a a a a
When storing integers (no decimals)
When storing decimals
`INTEGER` is good, safe option

|               |                                         |
| ------------- | --------------------------------------- |
| **Datatypes** | `SMALLINT`, `INTEGER`, `BIGINT`         |
| **Accepts**   | Integers only: `-64`, `0`, `2214555746` |
| **Fidelity**  | Stored exactly                          |
| **Use**       | When storing integers (no decimals)     |
| **Avoid**     | When storing decimalsy values           |
| **Which**     | `INTEGER` is good, safe option`         |

If storing integers, choosing the `INTERGER` datatype is the best option. `SMALLINT` can be reservered where disk space is urgent.

| type       | short code                                   |
| ---------- | -------------------------------------------- |
| `SMALLINT` | -32768 to 32767                              |
| `INTEGER`  | -2147483648 to 2147483647                    |
| `BIGINT`   | -9223372036854775808 to 9223372036854775807` |

### Arbitrary Precision

Arbitrary datatypes like `NUMERIC` can accept decimals, with variable-precision, and are stored exactly.

|               |                                                                                                                |
| ------------- | -------------------------------------------------------------------------------------------------------------- |
| **Datatypes** | `NUMERIC` / `DECIMAL`                                                                                          |
| **Accepts**   | integers and decimals: `-832.23`, `0`, `4.99`, `45`                                                            |
| **Fidelity**  | are stored exactly, at the expense of compute and calculation time                                             |
| **Use**       | when exactness is required, like monetary amounts                                                              |
| **Avoid**     | when accuracy and precision is not required as calculations are much slower compared to other number datatypes |
| **Which**     | `NUMERIC` is the only option here                                                                              |

#### Specifiy limits on precision

The `NUMERIC` dataype allows for arbitrary precision, but the maximum scale and precision can also be defined.

`NUMERIC(precision, scale)` - where `precision` is total number of digits and `scale` is the number of digits after the decimal place

- If precision is ommited, any precision and scale is allowed up to bounds of NUMERIC dataype
- `NUMERIC` is bounded to 131,072 digits (precision) and 16,383 after the decimal (scale).

---

### Floating Point Types

Floating point datatypes like `REAL` and `DOUBLE PRECISION` are inexact, variable-precision datatypes.

|               |                                                                    |
| ------------- | ------------------------------------------------------------------ |
| **Datatypes** | `REAL`/`FLOAT4`, `DOUBLE PRECISION`/`FLOAT8`                       |
| **Accepts**   | integers and decimals: `-832.23`, `0`, `4.99`, `45`                |
| **Values**    | are stored exactly, at the expense of compute and calculation time |
| **Use**       | When exactness is not required and data contains decimals          |
| **Avoid**     | When accuracy and precision is required, e.g., monetary values     |
| **Which**     | `REAL` for less precision, otherwise `DOUBLE PRECISION`            |

Variable-precision mean the precision, or length, or the data can vary, e.g., (`123`, `3.5121`, `2999410248746`)

#### Avoid when exactness is important

{{<callout icon="⚠️" text="Do not use floats where exact numbers, like money, are important. Use NUMERIC instead." />}}

Avoid using floating point when the exactness of the values is important, like dealing with money.

Inexact means the data is stored as an approximation to reduce size and boost calculation speed. The inexactness of the data used in the calculations means you start get stuff like `2 + 2 = 4.0000000001`.

Do not use if performing complex calculations for anything of importance. Equality comparisons between floating point data may not always operate as expected.

#### Specify limits on precision

- FLOAT(p) where p is precision in binary digits (max of 8)
- Floating point dataypes can also store `'Inifinity'`, `'-Infinity'`, `'Nan'`
- `REAL` has minimum precision of 6 digits and `DP` has minimum of 15

Float:

- Examples: `REAL`/`FLOAT4`, `DOUBLE PRECISION`/`FLOAT8`
- Range: The range varies with precision, as precision increases the range drops

test

## Short Codes

| type                      | short code    |
| ------------------------- | ------------- |
| `SMALLINT`                | `INT2`        |
| `INTEGER`                 | `INT4`, `INT` |
| `BIGINT`                  | `INT8`        |
| `SMALLSERIAL`             | `SERIAL2`     |
| `SERIAL`                  | `SERIAL4`     |
| `BIGSERIAL`               | `SERIAL8`     |
| `NUMERIC`                 | `DECIMAL`     |
| `REAL`                    | `FLOAT4`      |
| `DOUBLE PRECISION`        | `FLOAT8`      |
| `BOOLEAN`                 | `BOOL`        |
| `TIME WITH TIMEZONE`      | `TIMEZ`       |
| `TIMESTAMP WITH TIMEZONE` | `TIMESTAMPZ`  |
