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
* [Cache selectors](#cache-selectors)


## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Cache selectors
### Why?
To avoid using jQuery every time to look up the element in the DOM you should cache your selectors.

### How?
``` javascript
// Bad
$('[my-element]').addClass('is-active');
$('[my-element]').on('click', function() {
  // ...
});

// Good
var $myElement = $('[my-element]');
$myElement.addClass('is-active');
$myElement.on('click', function() {
  // ...
});
```

[↑ back to Table of Contents](#table-of-contents)