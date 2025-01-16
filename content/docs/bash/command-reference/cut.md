---
title: "cut"
description: "Cut stuff."
summary: ""
date: 2025-01-16T02:29:55-08:00
lastmod: 2025-01-16T02:29:55-08:00
weight: 68
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Cut stuff.

## Cut by Field

Cut by field using the `-f` flag. The default delimiter is TAB.

```bash
# sales.txt - example dataset
2022-20-11  11:34   134.23      Electronics
2022-20-10  10:23   500.34      Appliances
```

Use `cut` to extract the first and third column:

```bash
$ cut sales.txt -f 1,3

# output
↪ 2022-20-11  134.23
  2022-20-10  500.34
```

Display first through third field:

```bash
$ cut sales.txt -f -3

# output
↪ 2022-20-11  11:34   134.23
  2022-20-10  10:23   500.34
```

### Cut with a Delimiter

Use the `-d` flag to specify a delimiter. By default, cut uses `tab` the delimiter. See all [delimiters](#delimiters).

```bash
cut sales.txt -d ":" -f 1,2
```

```bash
$ echo "hello local host" | cut -c1-5
↪ hello
```

## Extract a field

```bash
# extract single field
$ echo "foo:bar:fizz" | cut -d':' -f1
↪ foo

# extract multiple fields (exclusive)
$ echo "foo:bar:fizz" | cut -d':' -f1,3
↪ foo:fizz

# extract multiple fields (inclusive)
$ echo "foo:bar:fizz" | cut -d':' -f1-3
↪ foo:bar:fizz

# exclude a specific field
$ echo "foo:bar:fizz" | cut -d':' --compliment -f2
↪ foo:fizz
```

## Delimiters

Specify a delimiter with `-d`. Default is tab.

```bash
cut -d':'   # colon
cut -d','   # comma
cut -d'|'   # pipe
cut -d' '   # space
cut -d'#'   # custom
```

## Misc

### Preprocessing Data with `tr`

Convert spaces to tabs for compatibility with cut:

```bash
cat data.txt | tr ' ' '\t' | cut -f3
```

### Handling Multi-Character Delimiters

For multi-character delimiters, preprocess with sed or awk:

```bash
cat data.txt | sed 's/::/|/g' | cut -d'|' -f2
```

## Snippets

### Extract AWS credentials

```bash
echo -e "===> EXISTING IDENTITY: "
aws sts get-caller-identity
aws configure list

echo -e "===> Assuming the role..."
aws sts assume-role --role-arn arn:aws:iam::123456789:role/roleName --role-session-name role-session-${UNIQUE_ID} >> credentials.json
export AWS_SECRET_ACCESS_KEY=$(cat credentials.json | grep "SecretAccessKey" | cut -d ':' -f2 | cut -d '"' -f2)
export AWS_SESSION_TOKEN=$(cat credentials.json | grep "SessionToken" | cut -d ':' -f2 | cut -d '"' -f2)
export AWS_ACCESS_KEY_ID=$(cat credentials.json | grep "AccessKeyId" | cut -d ':' -f2 | cut -d '"' -f2)

echo -e "===> ASSUMED IDENTITY:"
aws sts get-caller-identity
aws configure list
```
