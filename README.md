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
* [Optimise selectors for performance](#optimise-selectors-for-performance)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Optimise selectors for performance
### Why?
* Always try to create a selector that is exactly specific enough for your case. Make sure you don't depend on a specific HTML structure.
* Creating good selectors makes your HTML more flexible.
* Querying on a specific element is faster than the whole document. This is really easy with module based development.

### How?

``` javascript
/* avoid: overly specific */
var $amount = $('[data-table] [data-table-stats] [data-table-amount]');

/* recommended: using `.find()` which is highly optimised on the parent element */
var $amount = $('[data-table]').find('[data-table-amount]');
```

.

[↑ back to Table of Contents](#table-of-contents)
