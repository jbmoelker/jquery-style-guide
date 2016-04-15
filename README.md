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
* [Avoid the `.show()` and `.hide()` methods](#avoid-the-`.show()`-and-`.hide()`-methods)

## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.

## Avoid the `.show()` and `.hide()` methods
### Why?
The same effect can be accomplished with toggling classes.
These methods are doing a lot in the background and are a performance hit. These can methods can also be omitted when creating a custom build to reduce the size.

### How?
``` javascript
var $myElement = $('[my-element]');
var $mySecondElement = $('[my-second-element]');

// Bad
$myElement.show();
$mySecondElement.hide();

// Good
$myElement.addClass('is-visible');
$mySecondElement.removeClass('is-visible');

```

``` css
.is-visible {
	display: block;
}
```

[↑ back to Table of Contents](#table-of-contents)
