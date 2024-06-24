---
title: "Deps"
description: ""
summary: ""
date: 2024-06-22T16:05:55-07:00
lastmod: 2024-06-22T16:05:55-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Install deps with pip

```bash
# install deps
pip install -r requirements.txt

# updated deps
pip install --upgrade -r requirements.txt
```

## Using `venv` virtual environment

```bash
# create virtual env
python -m venv <environment-name>
python -m venv venv

# activate environment
source <environment-name>/bin/activate
source venv/bin/activate

# install deps
pip install -r requirements.txt

# deactive environment
deactivate

# remove environment
rm -rf venv
```
