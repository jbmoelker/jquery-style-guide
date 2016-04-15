# jQuery Style Guide

Opinionated *jQuery Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).

## Purpose

This guide aims to improve the way your team uses [jQuery](http://jquery.com/). It helps you write code which is

* close to modern web technologies and best practices.
* easy to understand for developers / team members.
* easy to reuse (in other projects).
* more performant.
* avoids overuse of jQuery and needless vendor locking.


## Table of Contents

* [About jQuery](#about-jquery)
* [Optimise selectors by avoiding excessive specificity](#optimise-selectors-by-avoiding-excessive-specificity)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Optimise selectors by avoiding excessive specificity
### Why?
When selecting elements you should always try to do it by creating a selector that is exactly specific enough for your case.

### How?
``` javascript
// Bad
var $amount = $('.data table.stats td.amount');

// Better: Drop the middle if possible
var $amount = $('.data td.amount');

// Better: Using `.find()` which is highly optimised
var $amount = $('.data').find('.amount');

// Best: Add a specific selector to the element
var $amount = $('[data-table-amount]');
```

[↑ back to Table of Contents](#table-of-contents)