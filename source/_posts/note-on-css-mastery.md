---
title: CSS Mastery笔记
date: 2017-08-21
tags: CSS
---

我们的宗旨是：__backward compatible and future friendly__

<!-- more -->

---

SEPARATION OF CONCERNS
---
>  “Small pieces, loosely joined.” 

Unix管道的设计哲学吗？

> You could think of these small pieces of code as LEGO bricks. Each brick is incredibly simple, but they can be joined together in numerous ways to create objects of immense complexity.

CSS HISTORY
---
* CSS 1 became a W3C recommendation at the end of 1996 and contains very basic properties such as fonts, colors, and margins.
* CSS 2 became a recommendation in 1998 and added advanced concepts such as floating and positioning, as well as new selectors like the child, adjacent sibling, and universal selectors.
* CSS 3 is a slightly different beast. In fact, there is no CSS 3 as such, but a collection of modules each `leveled` independently. 

WHICH VERSION
---
> What is important is knowing what parts of HTML and CSS have been implemented in browsers, and how robust and bug-free those implementations are.
* http://caniuse.com
* http://webplatform.org
* http://developer.mozilla.org
> The ability to make judgment calls on backward compatibility vs. future-friendly code is part of what defines the true CSS Master.

PROGRESSIVE ENHANCEMENT
---
> “start by making it work well for
the lowest common denominator, but feel free to take things further where they are supported.”

* `<!DOCTYPE html>`: a machine-readable hint

SEMANTICS
---
Semantic markup is the foundation of any good HTML document. *Semantics* is the scientific study of meaning. In the context of a made-up language with a formal set of symbols, such as HTML and its elements and attributes, semantics refers to what we mean by using a certain symbol. Put simply, **semantic markup is the practice of using the right element in the right place, resulting in meaningful documents.**

CLASS AND ID
---
> Naming something is one of the most important (and often most difficult) parts of writing code.

> There are only two hard problems in Computer Science: cache invalidation and naming things — Phil Karlton

* In CSS we think of `class` names as a way of defining what a thing is.
* We should choose a name that indicates what type of component it is. `product-list` is over `large-centered-list`。

STRUCTURAL ELEMENT
---
Creating logical sections of an HTML document.　→ http://html5doctor.com/
* section
* header
* footer
* nav
* article
* aside
* main

"shim" vs "polyfill"　→https://github.com/aFarkas/html5shiv

**divitis**　→`div`
> The practice of [authoring](https://en.wiktionary.org/wiki/authoring) web-page code with many `div` elements in place of meaningful semantic HTML elements.

`span` is distinct from `div` in that it is a text-level element, and is used to provide structure within the flow of a piece of text.

```html
<p>At <time datetime="20:07">7 minutes past eight</time> Harry shouted, <q>Can we just end
this, now!</q> He was <strong>very</strong> angry.</p>
```
PRESENTIONAL
---
* `<em>` means __emphasis__
* `<strong>` means __strong emphasis__

ARIA
---
> Accessible Rich Internet Applications

https://www.w3.org/TR/wai-aria/roles#role_definitions

VALIDATION
---
*nonconformant*

In fact, rules for dealing with invalid HTML are included in the HTML specification to ensure browser makers deal with error handling in a consistent way.

**html validator**
* http://validator.w3.org/
* http://chrispederick.com/work/web-developer/

**css validator**
* http://jigsaw.w3.org/css-validator/
