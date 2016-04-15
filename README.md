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
* [Prefer promises over callbacks](#prefer-promises-over-callbacks)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Prefer promises over callbacks
### Why?
Use promises instead of callbacks to keep code more readable and future proof when handling asynchronous requests.

### How?
``` javascript
// Bad
$.ajax('//example.com/articles/1/author', {
  dataType : 'json',
  success: function(response) {
    // ...
  },
  error: function(response) {
    // ...
  },
  complete: function(response) {
    // ...
  }
});

// Good
var request = $.ajax('//example.com/articles/1/author', {
  dataType : 'json'
});
request.done(function(response) {
  // ...
});
request.fail(function(response) {
  // ...
});
request.always(function(response) {
  // ...
});
```

[↑ back to Table of Contents](#table-of-contents)