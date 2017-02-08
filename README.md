# Hyperbars
Compile [Handlebars](http://handlebarsjs.com/) templates to javascript which can be used with [Virtual DOM](https://github.com/Matt-Esch/virtual-dom).
This library offers a comprehensive coverage of the Handlebars API and more features will be added soon. Your [Handlebars](http://handlebarsjs.com/) templates 
should work correctly without any modifications. 

Compiles something like this:
```html
<div>
    {{#if profile}}
        {{name}}
    {{/if}}
</div>
```

into this:
```js
(function (state) {
	var Runtime = Hyperbars.Runtime;
	var context = state;
	return h('div', {}, [Runtime.if(context['profile'], context, function (context, parent, options) {
		return ['' + context['name']]
	})])
}.bind({}))
```

then you can call the returned function with a state object:
```js
var compiled = Hyperbars.compile(template)
Hyperbars.createElement( compiled({ profile:null }) ) // <div></div>
Hyperbars.createElement( compiled({ profile:{ name:"Foo bar" }}) ) // <div>Foo bar</div>
```

## Installation
Step 1: In your `index.html` file, include the hyperbars.js or hyperbars.min.js file:
```html
<script type="text/javascript" src="path/to/dist/hyperbars.js"></script>
```

## Usage
Step 1: Compilation & Setup
```js
var template = "<div>{{name}}</div>"
var state = { name: "Foo bar" }
var compiled = Hyperbars.compile(template)
```
Step 2: Displaying
```js
var tree = compiled(state)
var element = Hyperbars.createElement(tree)

// Do what you will from here e.g document.append(element)
```
Step 3: Updating
```js
// State changes somehow
state.name = "Baz Bar"

// Generate new tree based on new state
var newTree = compiled(state)

// Find changes required to update real DOM so it is identical to virtual dom
var patches = Hyperbars.diff(tree, newTree)

// Apply the changes
Hyperbars.patch(element, patches)

// Cache new tree
tree = newTree
```
**Note:** It is best practice to create a function called "setState(newState)" which performs step 3.

## Partials
Currently only basic partials are supported. Please refer to the change log below for the scope of what is supported
with partials. I will be adding more coverage of the Handlebars partials API soon.

Step 1: Register partial with Hyperbars
```js
Hyperbars.partials['myPartial'] = Hyperbars.compile('<nav>{{title}}</nav>', {raw: true})
```
**Note:** Notice the use of `{raw: true}` when compiling the partial. This will return a string rather then the compiled function.

Step 2: Use it in your Handlebars template
```html
<body>
    {{> myPartial}}
</body>
```

### Injecting a context
```html
<body>
    {{> myPartial myContext}}
</body>
```

### Parameters
```html
<body>
    {{> myPartial title="Hello World"}}
</body>
```

To view more on partials please visit see [handlebars partials](http://handlebarsjs.com/partials.html).

## v0.1.10
* Fixed partial parameter bug

## v0.1.9
* Fixed single quote error

## v0.1.7
* Added support for expressions in parameters
* Added support for multiple blocks in parameters

## v0.1.4
* Fixed windows line-break bug

## v0.1.1
* Output is much more readable
* Fixed partial parameter bug

## v0.1.0
* Added CommonJS support
* Added support for basic partials `{{> myPartial}}`
* Added support for partial context `{{> myPartial myContext}}`
* Added support for partial parameters `{{> myPartial title="Hello"}}`
* Added a bunch of tests.

## v0.0.8
* Added support for `{{{no-escape}}}`

## v0.0.7
* Added minified version
* Dependencies are now part of the source
* HTML attribute bug fix

## v0.0.6
* Support for `{{@index}}`, `{{@first}}`, `{{@last}}`
* Support for `../`
* Bug fixes

## Roadmap
* Add support for custom helpers
* Add support for `{{else}}`
* Add support for `{{! comments }}`

## Dependencies
* [htmlparser2](https://github.com/fb55/htmlparser2)
* [virtual-dom](https://github.com/Matt-Esch/virtual-dom)

## See also
* [Virtual DOM Handlebars](https://github.com/jchook/virtual-dom-handlebars)
* [handlebars-react](https://github.com/stevenvachon/handlebars-react)

## License
This software is provided free of charge and without restriction under the [MIT License](LICENSE)