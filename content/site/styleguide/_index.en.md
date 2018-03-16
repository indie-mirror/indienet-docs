---
title: "Style Guides"
date: 2018-03-06T13:47:49+01:00
draft: false
---

## CSS Style Guide

Below are the basic rules we use for writing CSS consistently.

### Basics

CSS is written to aid readability and findability through consistency.

* Elements and curly brackets are always on their own lines.
* Single-line comments are used to describe CSS on the line below.
* Rules are always in alphabetical order, except 4-sided rules (margins, padding, borders) which are ordered by top, left, bottom, right, following the shorthand order. 
* Don’t break comments or rules mid-line. Let rules fill full-width so developers can use their chosen word wrap length.

```
.element {
    /* comment explains the line *below* */
    /* rules are in alphabetical order */
    font-size: 1em;
    font-weight: bold;
    margin: 0;
    /* padding, margin and border (4-sided) rules are in top-left-bottom-right order */
    padding-top: 1em;
    padding-left: 20%;
    padding-bottom: 0;
    padding-right: 2em;
}
```

### Structure

Elements are ordered from least specific to more specific, according to the cascade.

#### Headings and comments

Headings (as comments) are used to make it easy to skim-read and find specific rules. Use an empty line below every heading.

* H2-level headings are in UPPERCASE, and use dashes to segment groups of rules.

```
/* 
GENERAL RULES
- - - - - - - - - - - - - - - - - - - - - - - - */
```

* H3-level headings are in sentence case, and use a shorter series of dashes to segment groups of rules.

```
/*
Component name
- - - - - - - - - - - - */

Single line or multiline comments outside element refer to the element below. Use one line of space in between for readability.

/* Images are circular with grey border */

img
{
    border: 1px solid grey;
    border-radius: 50%;
}
```

Single line comments inside element refer to the line below.

```
/* mask image into circle using borders */
border-radius: 50%;
```

Avoid multi-line comments inside elements.

### Naming coventions

For the sake of compatibility, class names follow the Bulma naming conventions.

```
<div class='card-content'>
```

When overriding Bulma’s CSS, we override on the Bulma classes. *(While the example illustrates the point, we no longer override `rem`s for `em`s!)*

```
.card-content
{
    /* override Bulma’s use of rems on font sizes so font scales according to our global styles */
    font-size: 1em;
}
```

When creating custom (non-Bulma) components, we follow the Bulma-style naming convention of lowercase hyphen-separated class names.

```
<div class='profile-bio'>
```

### Units

We use `rem`s everywhere to keep our sizes relative to the root font size. This means that:

* All sizes will be relative to a person’s minimum font size as chosen in their browser settings.
* The entirety of the interface can be scaled using one media query to adjust the root font size.
* Values are always consistent and proportional to other values.