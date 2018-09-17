# jQuery Style Guide

> Opinionated *jQuery Style Guide* for teams by [De Voorhoede](https://twitter.com/devoorhoede).

[![style guide jquery](https://img.shields.io/badge/style%20guide-jquery-5ed9c7.svg)](https://github.com/voorhoede/jquery-style-guide)

## Purpose

This guide aims to improve the way your team uses [jQuery](http://jquery.com/). It helps you write code which is

* close to modern web technologies and best practices.
* easy to understand for developers / team members.
* easy to reuse (in other projects).
* more performant.
* avoids overuse of jQuery and needless vendor locking.


## Table of Contents

* [About jQuery](#about-jquery)
* [Consider native browser features](#consider-native-browser-features)
* [Consider lightweight alternative](#consider-lightweight-alternative)
* [Avoid `.ready()`](#avoid-ready)
* [Assign `jQuery` to `$`](#assign-jquery-to-)
* [Cache jQuery lookups](#cache-jquery-lookups)
* [Optimise selectors for performance](#optimise-selectors-for-performance)
* [Use `.first()` for single element](#use-first-for-single-element)
* [Use `.on()` for event binding](#use-on-for-event-binding)
* [Use event delegation](#use-event-delegation)
* [Avoid `.show()`, `.hide()` and `.toggle()`](#avoid-show-hide-and-toggle)
* [Avoid using `.css()`](#avoid-using-css)
* [Prefer CSS animations over `.animate()`](#prefer-css-animations-over-animate)
* [Prefer native array methods](#prefer-native-array-methods)
* [Prefer promises over callbacks](#prefer-promises-over-callbacks)
* [Lint your script files](#lint-your-script-files)
* [Create a smaller build](#create-a-smaller-build)


## About jQuery

[jQuery](http://jquery.com/) is a utility library for easy DOM access & manipulation, event handling, Ajax and more. By using jQuery you can write consise and expressive code which works across modern and legacy browsers. jQuery has extensive tests, detailed documentation, a large active community and an ecosystem of plugins.


## Consider native browser features

Since the release of jQuery many of its features now have a native browser equivalent. For example adding a class to an element can now be achieved via `element.classList.add(className)` instead of `$(element).addClass(className)`. Depending on the needs of your project you may be able to use only native browser features.

### Why?

* Native browser features are following the spec, making them future proof.
* Native browser features are closer to the metal, making them faster than jQuery.
* Developers who already know JavaScript don't need to learn the specifics of jQuery.

### How?

* Only apply Javascript after feature detection. Use a server-side rendered page as a basis. Enhance parts of a page only if the browser natively supports the required technology.
* Consult [platform.html5.org](https://platform.html5.org/) for overview of native browser technologies and [Can I use](http://caniuse.com/) and [Kangax tables](http://kangax.github.io/compat-table/es5/) for browser compatibility.
* Consult [you might not need jQuery](http://youmightnotneedjquery.com/) for native alternatives to jQuery features.

[↑ back to Table of Contents](#table-of-contents)


## Consider lightweight alternative

jQuery is the swiss army knife for DOM and event handling and much more. While jQuery offers a wide range of features, you might not need most of them in your project. Simply because you have little functionality or only modern browsers to support. In that case consider a lightweight alternative.

### Why?

* Lightweight alternatives have a smaller file size and can therefore be downloaded faster.
* Lightweight alternatives have less wrapper functionality which typically makes them faster.
* Some lightweight alternatives support only newer browsers and can therefore stay closer to native browser methods. This typically makes them more performant and easier to understand.
* Some lightweight alternatives mimic the jQuery API. Which means if you know jQuery, you know the alternative.

### How?

* Use a lightweight alternative like [Cash](https://github.com/kenwheeler/cash), [Dominus](https://github.com/bevacqua/dominus#readme), [Shoestring](https://github.com/filamentgroup/shoestring#readme) or [Zepto](https://github.com/madrobby/zepto#readme).
* [Create a jQuery custom build](#create-a-jquery-custom-build) to only include the features you need.

[↑ back to Table of Contents](#table-of-contents)


## Avoid `.ready()`

jQuery's [`.ready()`](https://api.jquery.com/ready/) ensures your script is not executed before the DOM is ready. This is important because we typically want to access the DOM in our script. However, since the script can't be executed before the DOM is ready, a better practice is to defer script execution.

### Why?

* Using `.ready()` means scripts have been loaded too early. Therefore defer loading instead.
* Script loading blocks page rendering. Therefore script loading should be defered. 

### How?

Avoid loading scripts too early and waiting for the DOM to be ready using `.ready()`.
Defer script loading by placing scripts just before the closing `</body>` tag or using the [`defer` attribute](https://developer.mozilla.org/en/docs/Web/HTML/Element/script#attr-defer):

```html
<!-- recommended: load in tail -->
<body>
   ...
	<script src="path/to/jquery.min.js"></script>
	<script>/* DOM is ready, use jQuery directly */</script>
</body>
```

```html
<!-- recommended: defer script loading -->
<head>
	...
	<script defer src="path/to/jquery.min.js"></script>
	<script defer src="path/to/my-app.min.js"></script>
</head>
```

Note: Be aware [using `defer` in IE <= 9 can cause issues](https://github.com/h5bp/lazyweb-requests/issues/42). So consider your browser scope before using this setup.

```html
<!-- avoid: -->
<head>
	...
	<script src="path/to/jquery.min.js"></script>
	<script>jQuery.ready(function(){ /* ... */ });</script>
	<!-- same applies to external script containing `.ready()` -->
</head>
```

[↑ back to Table of Contents](#table-of-contents)


## Assign `jQuery` to `$`

### Why?

* Assigning `jQuery` to `$` is a common practice, which developers are familiair to.
* Assigning `jQuery` to `$` within a scope, avoids unexpected conflict with other scripts using `$`.
* The jQuery documentation and other resources typically use `$`.

### How?

Explicitly assign `jQuery` to `$` within a scope. When using a module loader (like CommonJS) assign it directly to a variable named `$`. Otherwise use an [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (immediately-invoked function expression):

```javascript
/* recommended when using module loader, like CommonJS: */
const $ = require('jquery');
// use jQuery as $
```

```javascript
/* recommended: use an IIFE */
(function($){
	// use jQuery as $
}(jQuery));
```

[↑ back to Table of Contents](#table-of-contents)

## Cache jQuery lookups

Every call to `$(element}` asks jQuery to rescan for the matching element, wrap it in a jQuery object, and create a new instance of something you already have in memory. This is something avoidable if you already did it once.

### Why?
* It avoids querying the DOM for the element everytime want to use it, which greatly improves performance.
* You can use descriptive variable names which convey more meaning.

```javascript
/* avoid: repeating jQuery lookups */
$('button').addClass('is-active');
$('button').on('click', function(event) {});

/* recommended: cache jQuery lookup in variable */
var $button = $('button');
$button.addClass('is-active');
$button.on('click', function(event) {});
```

[↑ back to Table of Contents](#table-of-contents)


## Optimise selectors for performance

### Why?
* Always try to create a selector that is exactly specific enough for your case. Make sure you don't depend on a specific HTML structure.
* Creating good selectors makes your HTML more flexible.
* Querying on a specific element is faster than the whole document. This is really easy with module based development.

### How?

``` javascript
/* avoid: overly specific */
var $amount = $('[data-table] [data-table-amount]');
var $percentage = $('[data-table] [data-table-percentage]');

/* recommended: using `.find()` which is highly optimised on the parent element */
var $table = $('[data-table]');
var $amount = $table.find('[data-table-amount]');
var $percentage = $table.find('[data-table-percentage]');
```

[↑ back to Table of Contents](#table-of-contents)


## Use `.first()` for single element

jQuery always returns a collection when using `$(selector)`, while sometimes you are only interested in / only expect one element. In vanilla JS you would use `.querySelector(selector)` instead of `.querySelectorAll(selector)`.

### Why?
To make it clear for other developers of you intention of just using one element

### How?

```javascript
/* collection of buttons (akin querySelectorAll) */
$buttons = $form.find('button');

/* versus just a single button (akin querySelector) */
$submitButton = $form.find('[type="submit"]').first();
```

Naturally variable names should also reflect this. So plural for collections (`$buttons`), singular for a individual element (`$button`).

[↑ back to Table of Contents](#table-of-contents)


## Use [.on()](http://api.jquery.com/on/) for event binding

Methods like [`.click()`](http://api.jquery.com/click/) or [`.change()`](http://api.jquery.com/change/) are just alias for `.on('click')` and `.on('change')`. 

### Why?

* `.on()` supports [event delegation](http://api.jquery.com/on/#direct-and-delegated-events) resulting in more flexibility and better performance.
* It's a way to keep consistency as all your events have the same signature.
* Avoiding using aliases let you trim the jQuery custom build. That way you reduce load/parse times and the file size.

### How?

``` javascript
/* avoid: .click() */
$button.click(function(event) {});

/* recommended: .on() */
$button.on('click', function() {});
```

[↑ back to Table of Contents](#table-of-contents)


## Use event delegation

> When a selector is provided, the event handler is referred to as delegated. jQuery bubbles the event from the event target up to the element where the handler is attached and runs the handler for any elements along that path matching the selector.
>
> — [jQuery](http://api.jquery.com/on/#direct-and-delegated-events)

### Why?

* Using delegated events allows for events to be processed even to elements added to the document later
* Keeps the scope of the event *bubbling* shorter and thus performant.

### How?

```javascript
$list = $('[todo-list]').first();
$items = $list.find('[todo-item]');

/* avoid: event listener on each item */
$items.on('click', function(event) { /* ... */ });

/* recommended: event delegation on list */
$list.on('click', '[todo-item]', function(event) { /* ... */ });
```

[↑ back to Table of Contents](#table-of-contents)


## Avoid `.show()`, `.hide()` and `.toggle()`

JQuery lets you [`.show()`](http://api.jquery.com/show/), [`.hide()`](http://api.jquery.com/hide/) and [`.toggle()`](http://api.jquery.com/toggle/) elements. jQuery toggles an inline `display: none` to achieve this. 
HTML5 introduces a [new global attribute named `[hidden]`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden), which is styled as `display: none` by default. It's better practice to use this native standard and toggle `[hidden]` instead of using  `.show()`, `.hide()` and `.toggle()`.

### Why?

* Avoid `.show()`, `.hide()` and `.toggle()` as they use inline styles, which are hard to overwrite.
* Prefer `[hidden]` as it semantically indicates an element is not yet, or no longer, relevant.
* Styles should be managed in (external) CSS stylesheets (not inlined) for separation of concerns.

### How?

Add, remove or toggle the `[hidden]` attribute instead of using `.show()`, `.hide()` or `.toggle()`.

Note: If you need to support pre HTML5 browsers use CSS to style `[hidden]` correctly:
```css
/* recommended: ensure hidden elements are not displayed in pre HTML5 browsers */
[hidden] { display: none !important; }
```

#### Show elements

```javascript
/* avoid: `.show()` elements */
$elements.show();

/* recommended: remove `[hidden]` attribute */
$element.removeAttr('hidden')
```

#### Hide elements

```javascript
/* avoid: `.hide()` elements */
$elements.hide();

/* recommended: add `[hidden]` attribute */
$elements.attr('hidden', '');
```

#### Toggle elements

```javascript
/* avoid: `toggle()` elements */
$elements.toggle();
```

```javascript
/* recommended: toggle hidden attribute (with jQuery): */
// add `toggleHidden` functionality as jQuery plugin
$.fn.toggleHidden = function() {
	return this.each(function(index, element) {
		var $element = $(element);
		if ($element.attr('hidden')) {
			$element.removeAttr('hidden')
		} else {
			$element.attr('hidden', '');
		}
	});
};

// call `toggleHidden` on element:
$elements.toggleHidden();
```

```javascript
/* recommended: toggle hidden attribute (without jQuery): */
$elements.get().forEach(toggleHidden);

function toggleHidden(element) {
	if (element.hasAttribute('hidden')) {
		element.removeAttribute('hidden')
	} else {
		element.setAttribute('hidden', '');
	}
}
```
[↑ back to Table of Contents](#table-of-contents)


## Avoid using `.css()`

jQuery can get and set styling directly on an element with the [`.css()`](https://api.jquery.com/css/) method.
When using `.css()` to set CSS it will set the styles inline and you will be mixing concerns (styling and logic).

### Why?
* By toggling classes the same thing can be accomplished.
* Separation of concerns, don't mix CSS with JavaScript.
* When the `.css()` method is not used it can be omitted when creating a custom jQuery build. This reduces file size.

### How?
``` javascript
/* avoid: mixing JavaScript and CSS */
$element.css('border', '1px solid green');
```

``` javascript
/* recommended: using a class */
$element.addClass('is-active');
```

``` css
/* CSS */
.is-active { border: 1px solid green; }
```

[↑ back to Table of Contents](#table-of-contents)


## Prefer CSS animations over `.animate()`

jQuery lets you create complex animation sequences using [.animate()](http://api.jquery.com/animate/). Since the introduction of jQuery native CSS has caught up and now also provides methods to transition and animate elements.

### Why?

* Defining animations in CSS separates presentation from (interaction) logic.
* Native CSS transitions and animations can be hardware-accelerated (using GPU), resulting in smoother animations.

### How?

For simple animations use a [CSS transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition):

```javascript
/* avoid: jquery animate */
$element.animate({ left: '50px' }, 150, 'easeout');
```

```javascript
/* recommended: css animations */
$element.addClass('is-active');
```

```css
/* vendor prefix might be required */
.is-active {
	transform: translate(50px);
	transition: transform 150ms ease-out;
}
```

For more complex animations use a [CSS keyframes animation](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes):

```javascript
/* avoid: jquery animate */
function blink() {
  $element
  	.animate({ opacity: 0 }, 1000)
    .animate({ opacity: 1 }, 1000, blink);
}
blink();
```

```javascript
/* recommended: css animations */
$element.addClass('is-blinking');
```

```css
/* vendor prefix might be required */
.is-blinking {
    animation: blink 2s infinite;
}

@keyframes blink {
	0%   { opacity: 1; }
	50%  { opacity: 0; }
	100% { opacity: 1; }
}
```

[↑ back to Table of Contents](#table-of-contents)


## Prefer native Array methods

### Why?

jQuery’s array methods are non-standard. They use the signature `(index, item/element)` while native uses `(item/element, index)`.

### How?

Make sure you check your browser scope [supports native array methods](http://kangax.github.io/compat-table/es5/). Then use [`.get()`](http://api.jquery.com/get/) to get a native array of HTML elements. Use [native array methods](https://dev.opera.com/articles/javascript-array-extras-in-detail/), like `forEach`, `map`, `filter` and `reduce` to process the elements:

``` javascript
/* avoid: jQuery array methods */
$elements.map((index, el) => /* ... */)

/* recommended: use native methods */
$elements.get().map(el => /* ... */)
```
[↑ back to Table of Contents](#table-of-contents)


## Prefer promises over callbacks

A Promise represents a proxy for a value not necessarily known when the promise is created. It allows you to associate handlers to an asynchronous action's eventual success value or failure reason.

### Why?

Use promises instead of callbacks to keep code more readable and future proof when handling asynchronous requests.
Promises can also be passed around so other modules can chain (.then) onto it, instead of complex callbacks inside callbacks (pyramid of doom) structures.

### How?

``` javascript
/* avoid: callbacks */
$.ajax('example.com/articles/1/author', {
  success: function(response) {},
  error: function(err) {}
});

/* recommended: use promise */
var request = $.ajax('example.com/articles/1/author');
request.then(function(response) {});
request.catch(function(err) {});
```

[↑ back to Table of Contents](#table-of-contents)


## Lint your script files

Linters like [ESLint](http://eslint.org/) and [JSHint](http://jshint.com/) improve code consistency and help trace syntax errors.

### Why?

* Linting files ensures all developers use the same code style.
* Linting files helps you trace syntax errors before it's too late.

### How?

Configure your linter to accept jQuery and `$` as global variable.

#### ESLint

```json
{
	"env": {
		"browser": true
	},
	"globals": {
		"jQuery": true,
		"$": true
	}
}
```

#### JSHint

```json
{
	"jquery": true,
	"browser": true
}
```

[↑ back to Table of Contents](#table-of-contents)


## Create a smaller build

> Special builds can be created that exclude subsets of jQuery functionality. This allows for smaller custom builds when the builder is certain that those parts of jQuery are not being used.
>
> — [jQuery](https://github.com/jquery/jquery#modules)

### Why?

* Smaller builds download quicker, are parsed quicker and require less memory.
* Excluding methods best avoided restricts developers from using them (no more `$('p').css('red')`, etc).

### How?

Follow the [official documention on creating a custom build](https://github.com/jquery/jquery#what-you-need-to-build-your-own-jquery) to get you setup.

Then create your own custom build excluding [modules](https://github.com/jquery/jquery#modules) you don't need: 

	grunt custom:-css,-css/showHide,-deprecated,-effects,-event/alias,-core/ready,-exports/amd

This custom build is an example that reflects the guidelines presented. Following this guide saves you 17KB (6kb gzipped) on your final jQuery size.

[↑ back to Table of Contents](#table-of-contents)


---

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

[De Voorhoede](https://twitter.com/devoorhoede) waives all rights to this work worldwide under copyright law, including all related and neighboring rights, to the extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
