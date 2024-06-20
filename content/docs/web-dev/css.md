---
title: "CSS"
description: ""
lead: ""
date: 2023-01-05T12:58:56-08:00
lastmod: 2023-01-05T12:58:56-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "css-826e2a6212bea8fba7ccec626527b5aa"
weight: 999
toc: true
---

### Positioning

- `position: static;` - default when not specified
  - Not affected by `top`, `bottom`, `left`, or `right` properties
  - Not positioned in any special way; positioned in line with normal page flow
- `position: relative;` - positioned relative to its normal position
  - When you want to move an element; use `top`, `bottom`, `left`, or `right`
- `absolute` - gets positioned relative to closest positioned ancestor (not viewport fixed)
  - Notes moves with page scrolling, overlaps other divs
  - If no positioned ancestors, it uses the document body and moves with page scrolling
  - Can use `top`, `bottom`, `left`, or `right`
- `position: fixed;` - gets positioned relative to the viewport
  - Notes: moves with page scrolling
  - Can use `top`, `bottom`, `left`, or `right`
- `position: sticky;` - gets positioned based on the user's scroll position
  - Uses a combination of `relative` and `fixed`
  - Use when you want to have an element appear, scoll up to the top, then stay sticky as you continue scrolling down

Test

```css
p {
  border: 1px solid black;
  border-radius: 3px;

  border-style: solid;
}
```

## Transitions

### Fade In

Create a fadein animation for class `class_name`. This uses `animation` that points to a `@keyframe` named `fade_in_name`. You can name the keyframe anything unique.

```
.class_name {
  animation: fade_in_name 1s;
}

@keyframes fade_in_name {
  0% { opacity: 0; }
  100% { opacity: 1; }
}

```
