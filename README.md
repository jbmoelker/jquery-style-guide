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
* [Avoid using the `.css()` method](#avoid-using-the-.css()-method)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Avoid using the `.css()` method
### Why?
In most cases toggling a class can accomplish the same thing.
By not using the  `.css()` method this can be omitted when creating a custom jQuery build to reduce the size.

### How?
``` javascript
var $myElement = $('[my-element]');

// Bad
$myElement.css('border', '1px solid green');

// Good
$myElement.addClass('is-active');
```

[↑ back to Table of Contents](#table-of-contents)