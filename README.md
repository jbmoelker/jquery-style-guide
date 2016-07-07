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
* [Use .on() for event binding](#use-on-for-event-binding)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Use [.on()](http://api.jquery.com/on/) for event binding

Methods like [`.click()`](http://api.jquery.com/click/) or [`.change()`](http://api.jquery.com/change/) are just alias for `.on('click')` and `.on('change')`. 

### Why?
Using the [.on()](http://api.jquery.com/on/) method instead can reduce memory usage and works on dynamically added elements.

### How?
``` javascript
// Bad
$('.todo-item').click(function(event) {});

// Good
$('.todo-item').on('click', function() {});

```

[↑ back to Table of Contents](#table-of-contents)