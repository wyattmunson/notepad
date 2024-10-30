---
title: "Dependencies"
description: ""
summary: "Managing dependencies in Python. Create a virtual environment with venv."
date: 2024-06-22T16:05:55-07:00
lastmod: 2024-06-22T16:05:55-07:00
draft: false
weight: 25
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

Run the below command to create a virtual environment named `venv`, active the virtual environment, and install dependencies in the `requirements.txt` file.

```bash { title="Run to bootstrap a virtual environment" }
# create virtual env named venv
python -m venv venv

# activate environment
source venv/bin/activate

# install deps
pip install -r requirements.txt
```

### Full `vevn` reference

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

## Requirements File

Use a `requirements.txt` file to specify libraries to install.

```txt { title="requirements.txt" }
requests
```

```bash { title="Use pip to install requirements.txt file" }
pip install -r requirements.txt
```
