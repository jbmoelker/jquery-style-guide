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
* [Avoid using `.css()`](#avoid-using-css)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Avoid using `.css()`
jQuery can get and set styling directly on an element with the [`.css()`](https://api.jquery.com/css/) method.
When using `.css()` to set CSS it will set the styles inline and you are mixing concerns (styling and logic).

### Why?
* By toggling classes the same thing can be accomplished.
* Separation of concerns, don't mix CSS with JavaScript.
* When the `.css()` method is not used it can be omitted when creating a custom jQuery build.

### How?
``` javascript
/* avoid: mixing JavaScript and CSS */
$('[my-element]').css('border', '1px solid green');

/* recommended: using a class */
$('[my-element]').addClass('is-active');
```

``` css
/* CSS */
.is-active { border: 1px solid green; }
```

[↑ back to Table of Contents](#table-of-contents)
