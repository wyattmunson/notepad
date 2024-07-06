---
title: "Templates"
description: ""
summary: ""
date: 2024-07-05T19:49:36-07:00
lastmod: 2024-07-05T19:49:36-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Basic

```md { title="/content/about.md" }
---
title: About
---

Basic about page.
```

### Basic Layout

Create a single layout page

```html {title="/layouts/_default/list.html"}
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>{{ .Page.Title }}</title>
  </head>

  <body>
    {{ .Content }}
  </body>
</html>
```

### Use multiple blocks

```go {title="/layouts/_default/baseof.html"}
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>{{ .Page.Title }}</title>
  </head>

  <body>
    {{ block "main" . }}
    {{ end }}
  </body>
</html>
```

```go {title="/layouts/_default/list.html"}
{{ define "main" }}
 {{ .Content }}
{{ end }}
```

## Add CSS styles

```css

```

```go { title="/layouts/_default/baseof.html" }
{{ $style := resources.Get "sass/main.scss" | resources.ToCSS | resources.Minify }}
<link rel="stylesheet" href="{{ $style.Permalink }}">
```

## General Templates

### Output from front matter

This is in the top of the stuff

```go { title="/layouts/_default/baseof.html" }
<h2>{{ .Params.summary }}</h2>
```

### Conditionals (If statements)

Use an if/else statement to evaludate conditional logic.

```go { title="/layouts/_default/baseof.html" }
{{ if isset .Params "title" }}
  <title>{{ .Params.title }}</title>
{{ else }}
  <title>{{ .Site.title }}</title>
{{ end }}
```

> In the above example, the template checks if `title` is set in the page's content. If so, that becomes the title, else it falls back to the global, site-wide title.

### Set and reference variable

Set and reference a variable with a `$` sign.

```go
{{ $favorite_day := "Friday" }}
{{ $favorite_day }}
```

### Loop

```go
{{ $states := slice "california" "new york" "kansas" "florida" }}

<ul>
  {{ range $states }}
    <li>{{ . }}</li>
  {{ end }}
</ul>
```

### Use with for nested key values

```md
---
title: Appearance
apperance:
  eyes: green
  snoot: boopable
  whiskers: true
  limbs:
    - claws: 5
      side: left
      position: front
---
```

```go
{{ with .Params.appearance }}
<h3>My top appearance traits</h3>
<dl>
 <dt>Whiskers</dt>
 <dd>{{ .whiskers }}</dd>

   {{ with .limbs }}
   <dt>Claws</dt>
   <dd>
       <ul>
     {{ range . }}
       <li>{{ .position }} {{ .side }} {{ .claws }</li>
     {{ end }}
     </ul>
   </dd>
 {{ end }}
</dl>
{{ end }}
```
