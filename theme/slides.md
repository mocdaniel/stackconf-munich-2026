---
theme: ./
# some information about your slides (markdown enabled)
title: The NWS Slidev Theme
description: What's possible and what's not
info: |
  Some additional info can go here
---

# This is a theme demo

What's possible with the nws-slidev-theme

A place far away <br/> a long, long time ago

---

# Who is this for?

## A quick overview

This theme is for you if...

- ...you present at conferences
- ...don't like Powerpoint
- ...need powerful customizations in your slides

---
layout: section
---

# Let's get started

---

# Sections

## What you just saw

This just now was a section. Create it using the `section` layout in a slide's frontmatter:

```md
---
layout: section
---

# Let's get started
```

---

# Code Blocks

You can display code `` `inline` `` or in code blocks with Markdown, beautifully highlighted with the `Dracula` theme:

```js
import {fetch} from 'node';

const greeting = 'Hello world!';

const greetSomeone = (name) => {
    console.log(`${greeting}, ${name}`);
}

greetSomeone()
```

Content of code blocks can be copied, highlighted, or even be interactive.

You can find more information [here](https://sli.dev/guide/syntax#code-block).
---
layout: two-cols
---

# You can also divide content

## The `two-cols` layout makes it possible

::left::

Create a slide like this:

```md
---
layout: two-cols
---

# You can also divide content

## The `two-cols` layout makes it possible

::left::

Create a slide like this:

...

::right::

Beautiful recursion happens here ❤️
```

::right::

Beautiful recursion happens here ❤️

---
layout: quote
---

# I really don't like PowerPoint at all. It's just not working for me.

## Daniel Bodky

---
layout: default
---

# Quotes

## Quote somebody smart

You can create quote slides like the one just now using the `quote` layout:

```md
---
layout: quote
---

# Errare humanum est

## Homer Simpson
```