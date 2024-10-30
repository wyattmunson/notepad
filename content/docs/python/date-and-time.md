---
title: "Date and Time"
description: ""
summary: "Python snippets around date and time."
date: 2024-10-29T21:54:22-07:00
lastmod: 2024-10-29T21:54:22-07:00
weight: 20
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Format Date and Time

Use `datetime.now()` to get the current time. Use `.strftime()` to format the timestamp object to a human-readable format.

```python
from datetime import datetime
now = datetime.now()

year = now.strftime("%Y") # => 2018

week = now.strftime("%a") # => Sun
week = now.strftime("%A") # => Sunday
week = now.strftime("%w") # => 0

day = now.strftime("%d") # => 01
day = now.strftime("%-d") # => 1

month = now.strftime("%b") # => Jan
month = now.strftime("%B") # => January
month = now.strftime("%m") # => 01
month = now.strftime("%-m") # => 1

hour = now.strftime("%H") # => 00..23
hour = now.strftime("%-H") # => 0..23
hour = now.strftime("%I") # => 01...12
hour = now.strftime("%=I") # => 1..12
meridian = now.strftime("%H") # => AM..PM
minute = now.strftime("%M") # => 00..59
minute = now.strftime("%-M") # => 0..59
second = now.strftime("%S") # => 00..59
second = now.strftime("%-S") # => 0..59
microsecond = now.strftime("%f") # => 00000..99999

utc_offset = now.strftime("%z") # => 00..59
time_zone_name = now.strftime("%Z") # => 0..59ode

local_timestamp = now.strftime("%c") # => Mon Jun  3 17:40:16 2024
```

### Convert Times

```python
from datetime import datetime

# convert datetime to string
strftime("%Y")

# convert string to datetime
datetime.strptime(date_string, "%d")
```
