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
* [Cache jQuery lookups](#cache-jquery-lookups)


## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Cache jQuery lookups

Every call to `$(element}` asks jQuery to rescan for the matching element, wrap it in a jQuery object, and create a new instance of something you already have in memory. This is something avoidable if you already did it once.

### Why?
* It avoids querying the DOM for the element everytime want to use it, which greatly improves performance.
* You can use descriptive variable names which convey more meaning.

### How?

``` javascript
/* avoid: repeating jQuery lookups */
$('button').addClass('is-active');
$('button').on('click', function(event) {});

/* recommended: cache jQuery lookup in variable */
var $button = $('button');
$button.addClass('is-active');
$button.on('click', function(event) {});
```

[↑ back to Table of Contents](#table-of-contents)