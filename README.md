# DOMToretto
DOMToretto is a lightweight DOM manipulation library inspired by jQuery. 

Functionality:
* Select and Manipulate DOM elements
* Queue and Remove Event Listeners
* Send HTTP Requests

## API

[`toretto$`](#toretto)

[DOM Selection](#dom-selection)
  * [`children`](#children)
  * [`parent`](#parent)
  * [`find`](#find)

[DOM Manipulation](#dom-manipulation)  
  * [`html`](#html)  
  * [`empty`](#empty)  
  * [`append`](#append)  
  * [`remove`](#remove)  
  * [`attr`](#attr)  
  * [`addClass`](#addclass)  
  * [`removeClass`](#removeclass)
  * [`hide`](#hide)
  * [`css`](#css)

[Effects](#effects)
  * [`animate`](#animate)
  * [`fadein`](#fade-in)
  * [`fadeout`](#fade-out)
  
[Event Listeners](#event-listeners)  
  * [`on`](#on)  
  * [`off`](#off)  

[`toretto$.ajax`](#torettoajax)

## `toretto$`
The DOMToretto library uses the global variable toretto$ as a wrapper for all methods in the DOMToretto library.  

1. toretto$ can be used to select one or multiple elements via CSS selectors (ex: `toretto$('.test')`).  This returns a `DOMNodeCollection` object containing an array of `HTMLElements`.  

2. toretto$ can take unwrapped `HTMLElements` and wrap them into a `DOMNodeCollection`.

3. toretto$ can queue functions to run once the DOM content of the page has fully loaded.  

Ex:
```javascript
// will run once DOM Content is loaded

toretto$(() => {
  console.log('DOM Content Loaded')
})

// can also queue functions to run once loaded

const start1 = () => {
  console.log('start 1')
}

const start2 = () => {
  console.log('start 2')
}

toretto$(start1)
toretto$(start2)
```

## DOM Selection
In addition to the `toretto$` method, there are methods defined on `DOMNodeCollection` that aid in the traversal and selection of DOM elements.  

#### `children`
Returns a `DOMNodeCollection` containing all child elements of the wrapped `HTMLElement`.  Note this only includes *direct* children.

#### `parent`
Returns a `DOMNodeCollection` containing the parent elements of the wrapped `HTMLElement`.

#### `find( selector )`
Returns a `DOMNodeCollection` containing descendants of all wrapped `HTMLElement`s, filtered by selector.

## DOM Manipulation

#### `html( [string] )`

* With argument: modifies the `innerHTML` of each element wrapped in the `DOMNodeCollection`.
* Without argument: returns the `innerHTML` of the first element wrapped in the collection.

#### `empty`

Empties the innerHTML of each `DOMNodeCollection` element.

#### `append( selection )`

Takes a single `HTMLElement`, `DOMNodeCollection`, or `string` argument and appends it to each wrapped element.

#### `remove( selector )`

Removes `HTMLElements` that are descendants of wrapped elements, filtered by selector.

#### `attr( attribute )`

Returns the selected HTML attribute for the first wrapped element in the `DOMNodeCollection`.

#### `addClass( string/array )`

* With `string`: adds the class to all wrapped elements in the `DOMNodeCollection`.
* with `array`: adds all classes in the array to all wrapped elements.

#### `removeClass( string/array )`

* With `string`: removes the class from all wrapped elements in the `DOMNodeCollection`.
* With `array`: removes all classes in the array from all wrapped elements.

#### `hide`

Hides all wrapped elements in the `DOMNodeCollection` by setting their display property.

#### `css( propName, [propVal] )`

The `css` method will provide different functionality based on the arguments given.

* With `propName` as `string`: returns the value of the specified css property on the first wrapped element.
* With `propName` as `array`: returns the values of the specified css properties on the first wrapped element.
* With `propName` as `object`: sets the specified css properties for each wrapped element.

Ex:
```javascript
// will apply all css properties specified in the POJO

c$('.test').css({
  width: '50px',
  height: '50px',
  'background-color': 'blue'
})
```

* With `propName` as `string` and `propVal` as `string`: will set the specified css property to the specified value for each wrapped element.
* With `propName` as `string` and `propVal` as `function`: will set the specified css property to the return value of the provided function.  The function receives 2 arguments: the index of the wrapped element, and the current css property value.

## Effects

#### `animate( properties, [duration, easing, callback] )`

Perform an animation on the wrapped elements.

Animate will accept the following arguments:
* Properties object, with key-value pairs pointing towards css properties.
* Duration, given in milliseconds, in which the animation will take place.
* Easing function, given as a string (ex: `ease-in`, `easeOutElastic`)
* Callback

#### `fadein( [duration, callback] )`

Fades the wrapped elements in.

#### `fadeout( [duration, callback] )`

Fades the wrapped elements out.

## Event Listeners

Ex:
```javascript
// define a handler
const handler = () => {
  console.log("Click event fired")
}

// select the appropriate elements
const collection = toretto$('.test')

// add/remove listeners
collection.on('click', handler)
collection.off('click')
```

#### `on( listener, callback )`

Adds event listener to each wrapped element.  List of events are available [here](https://developer.mozilla.org/en-US/docs/Web/Events).

#### `off( listener )`

Removes event listener from each wrapped element.

## toretto$.ajax

Sends HTTP Request and returns a `Promise` object.  Accepts an object as an argument with any of the following attributes:
  * method (default: 'GET'): HTTP Request method or type
  * url (default: window.location.href): URL for HTTP Request
  * success: success callback
  * error: error callback
  * data: data object (for 'POST')
  * contentType (default: 'application/x-www-form-urlencoded; charset=UTF-8'): content type of HTTP Request

Ex:
```javascript
// basic GET request

const test = toretto$.ajax({
  url: '/api/example',
  method: 'GET',
  success: (Data) => {
    console.log('Example created!');
  }
})
```

Because `toretto$.ajax` returns a promise, additional callbacks can be chained to the above HTTP request.

Ex:
```javascript
...

test.then(
  success => console.log(`the result was: ${success}`),
  er => console.log(`we hit an error: ${er}`)
)
```