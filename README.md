# fastn

Create ultra-lightweight UI components

[![Build Status](https://travis-ci.org/KoryNunn/fastn.svg?branch=master)](https://travis-ci.org/KoryNunn/fastn)
[![Join the chat at https://gitter.im/KoryNunn/fastn](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/KoryNunn/fastn?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

![fastn](http://korynunn.github.io/fastn/images/fastn-sml.png)

## [Homepage](http://korynunn.github.io/fastn/), [Try it](http://korynunn.github.io/fastn/try/), [Example app](http://korynunn.github.io/fastn/example/)

# Usage

The absolute minimum required to make a fastn component:

initialise fastn with default DOM components:

```javascript
var fastn = require('fastn')(
    // Default components for rendering DOM.
    require('fastn/domComponents')(/* optional extra constructors */)
);
```

Or use your own selection of constructors:

```javascript
// Require and initialise fastn
var fastn = require('fastn')({
    // component constructors.. Add what you need to use

    text: require('fastn/textComponent'), // Renders text
    _generic: require('fastn/genericComponent') // Renders DOM nodes
});
```

Make components:
```javascript
var something = fastn('h1', 'Hello World');

```

Put them on the screen:
```javascript
something.render();

window.addEventListener('load', function(){
    document.body.appendChild(something.element);
});
```
[^ try it](http://korynunn.github.io/fastn/try/#InJldHVybiBmYXN0bignaDEnLCAnSGVsbG8gV29ybGQnKTsi)

`fastn` is a function with the signature:

```javascript
fastn(type[, settings, children...])
```

which can be used to create a UI:

```javascript
// Create some component
var someComponent = fastn('section',
        fastn('h1', 'I\'m a component! :D'),
        fastn('a', {href: 'http://google.com'}, 'An anchor')
    );

someComponent.render();

// Append the components element to the DOM
document.body.appendChild(someComponent.element);
```
[^ try it](http://korynunn.github.io/fastn/try/#InJldHVybiBmYXN0bignc2VjdGlvbicsXG5cdGZhc3RuKCdoMScsICdJXFwnbSBhIGNvbXBvbmVudCEgOkQnKSxcblx0ZmFzdG4oJ2EnLCB7aHJlZjogJ2h0dHA6Ly9nb29nbGUuY29tJ30sICdBbiBhbmNob3InKVxuKTsi)

You can assign bindings to properties:

```javascript

var someComponent = fastn('section',
        fastn('h1', 'I\'m a component! :D'),
        fastn('a', {href: fastn.binding('url')},
            fastn('label', 'This link points to '),
            fastn('label', fastn.binding('url'))
        )
    );

someComponent.attach({
    url: 'http://google.com'
});

```

Which can be updated via a number of methods.


```javascript

someComponent.scope().set('url', 'http://bing.com');


```
[^ try it](http://korynunn.github.io/fastn/try/#InZhciBzb21lQ29tcG9uZW50ID0gZmFzdG4oJ3NlY3Rpb24nLFxuICAgICAgICBmYXN0bignaDEnLCAnSVxcJ20gYSBjb21wb25lbnQhIDpEJyksXG4gICAgICAgIGZhc3RuKCdhJywge2hyZWY6IGZhc3RuLmJpbmRpbmcoJ3VybCcpfSxcbiAgICAgICAgICAgIGZhc3RuKCdsYWJlbCcsICdUaGlzIGxpbmsgcG9pbnRzIHRvICcpLFxuICAgICAgICAgICAgZmFzdG4oJ2xhYmVsJywgZmFzdG4uYmluZGluZygndXJsJykpXG4gICAgICAgIClcbiAgICApO1xuXG5zb21lQ29tcG9uZW50LmF0dGFjaCh7XG4gICAgdXJsOiAnaHR0cDovL2dvb2dsZS5jb20nXG59KTtcblxuc2V0VGltZW91dChmdW5jdGlvbigpe1xuXHRzb21lQ29tcG9uZW50LnNjb3BlKCkuc2V0KCd1cmwnLCAnaHR0cDovL2JpbmcuY29tJyk7XG59LCAyMDAwKTtcblxucmV0dXJuIHNvbWVDb21wb25lbnQ7Ig==)

## Special component types

There are a few special component types that are used as shorthands for some situations:

### `text`

if a string or `binding` is added as a child into a containerComponent, fastn will look for a `text` component, set it's `text` to the string or `binding`, and insert it. This is handy as you don't need to write: `fastn('text', 'foo')` all over the place.
[^ try it](http://korynunn.github.io/fastn/try/#InJldHVybiBmYXN0bignZGl2Jyxcblx0ZmFzdG4oJ3RleHQnLCB7dGV4dDogJ0V4cGxpY2l0IHRleHQsICd9KSxcblx0J0ltcGxpY2l0IHRleHQsICcsXG4gICAgZmFzdG4uYmluZGluZygnYm91bmRUZXh0JylcbikuYXR0YWNoKHtcbiAgXHRib3VuZFRleHQ6ICdCb3VuZCB0ZXh0J1xufSk7Ig==)

### `_generic`

If the type passed to fastn does not exactly match any known components, fastn will check for a `_generic` component, and pass all the settings and children through to it.
[^ try it](http://korynunn.github.io/fastn/try/#InJldHVybiBmYXN0bignZGl2JyxcbiAgICAgICAgICAgICBcblx0ZmFzdG4oJ3NwYW4nLCAnV29vIGEgc3BhbiEnKSxcbiAgICAgICAgICAgICBcblx0ZmFzdG4oJ2JyJyksIC8vIEJyIGJlY2F1c2Ugd2UgY2FuIVxuICAgICAgICAgICAgIFxuICAgIGZhc3RuKCdhJywge2hyZWY6ICdodHRwczovL2dpdGh1Yi5jb20va29yeW51bm4vZmFzdG4nfSwgJ0FuIGFuY2hvcicpLFxuICAgICAgICAgICAgIFxuXHRmYXN0bignYnInKSwgLy8gQW5vdGhlciBiciBmb3IgcmVhc29uc1xuICAgICAgICAgICAgIFxuICAgIGZhc3RuKCdpbWcnLCB7dGl0bGU6ICdBd2Vzb21lIGxvZ28nLCBzcmM6ICdodHRwOi8va29yeW51bm4uZ2l0aHViLmlvL2Zhc3RuL3RyeS9mYXN0bi1zbWwucG5nJ30pXG4pOyI=)

## Default components

fastn includes 4 extremely simple default components that render as DOM nodes. It is not necessary to use them, and you can replace them with your own, enabling you to render to anything you want to.

### textComponent

A default handler for the `text` component type that renders a textNode. e.g.:

```javascript
fastn('something', // render a thing
    'Some string passed as a child' // falls into the `text` component, renders as a textNode
)
```

### genericComponent

A default handler for the `_generic` component type that renders DOM nodes based on the type passed, e.g.:

```javascript
fastn('div') // no component is assigned to 'div', fastn will search for _generic, and if this component is assigned to it, it will create a div element.
```

### listComponent

Takes a template and inserts children based on the result of its `items` property, e.g.:

```javascript
fastn('list', {
    items: [1,2,3],
    template: function(){
        return fastn.binding('item')
    }
})
```
Templated components will be attached to a model that contains `key` and `item`, where `key` is the key in the set that they correspond to, and `item` is the data of the item in the set.

#### Lazy templating

If you need to render a huge list of items, and you're noticing a UI hang, you can choose to enable
lazy templating by setting a lists' `insertionFrameTime` to some value:

```javascript
fastn('list', {
    insertionFrameTime: 32, // Only render items for 32 milliseconds at a time before awaiting idle time.
    items: [1,2,3],
    template: function(){
        return fastn.binding('item')
    }
})
```

### templaterComponent

Takes a template and replaces itself with the component rendered by the template. Returning null from the template indicates that nothing should be inserted.

The template function will be passed the last component that was rendered by it as the third parameter.

Note: The template function will run immediately upon component creation. This means that if your data is a binding, the first run of the template function will always receive undefined as it's item.

```javascript
fastn('templater', {
    data: 'foo',
    template: function(model, scope, currentComponent){
        if(model.get('item') === 'foo'){
            return fastn('img');
        }else{
            return null;
        }
    }
})
```

An optional property of `attachTemplates` can be provided. When set to true (default), the children rendered from the template will be attached to a new scope that contains the templater data, under the key of `item`.

When set to false, the children rendered from the template will inherit their attachment from the templator.

```javascript
var appData = {
    foo: 'bar'
};

fastn('templater', {
    data: binding('foo'),
    attachTemplates: false,
    template: function(model, scope, currentComponent){
        // model.get('item') -> is appData
    }
}).attach(appData)
```

## A little deeper..

A component can be created by calling `fastn` with a `type`, like so:

```javascript
var myComponent = fastn('myComponent');
```

This will create a component registered in `components` with the key `'myComponent'`

If `'myComponent'` is not found, fastn will check for a `'_generic'` constructor, and use that if defined. The generic component will create a DOM element of the given type passed in, and is likely the most common component you will create.

```javascript
var divComponent = fastn('div', {'class':'myDiv'});
```

The above will create a `component`, that renders as a `div` with a class of `'myDiv'`

__the default genericComponent will automatically convert all keys in the settings object to properties.__

## `fastn.binding(key)`

Creates a binding with the given key.

A binding can be attached to data using `.attach(object)`.


# The Bits..

There are very few parts to fastn, they are:

`component`, `property`, and `binding`

If you are just want to render some DOM, you will probably be able to just use the default ones.

## `component`

A fastn `component` is an object that represents a chunk of UI.

```javascript

var someComponent = fastn('componentType', settings (optional), children (optional)...)

```

## `property`

A fastn `property` is a getterSetter function and EventEmitter.

```javascript

var someProperty = fastn.property(defaultValue, changes (optional), updater (optional));

// get it's value
someProperty(); // returns it's value;

// set it's value
someProperty(anything); // sets anything and returns the property.

// add a change handler
someProperty.on('change', function(value){
    // value is the properties new value.
});

```

Properties can be added to components in a number of ways:

via the settings object:

```javascript

var component = fastn('div', {
        property: someProperty
    });

```
at a later point via property.addTo(component, key);

```javascript

someProperty.addTo(component, 'someProperty');

```

## `binding`

A fastn `binding` is a getterSetter function and `EventEmitter`.

It is used as a mapping between an object and a key or path on that object.

The path syntax is identical to that used in [enti](https://github.com/KoryNunn/enti#paths)

```javascript

var someBinding = fastn.binding('foo');

// get it's value
someBinding(); // returns it's value;

// set it's value
someBinding(anything); // sets anything and returns the binding.

// add a change handler
someBinding.on('change', function(value){
    // value is the properties new value.
});

```

You can pass multiple paths or other bindings to a binding, along with a fuse function, to combine them into a single result:

```javascript

var anotherBinding = fastn.binding('bar', 'baz', someBinding, function(bar, baz, foo){
    return bar + foo;
});

```

### `binding.from(value)`

 - if value is a binding: return `value`,
 - else: return a binding who's value is `value`.

useful when you don't know what something is, but you need it in a binding:

```javascript

var someBinding = fastn.binding('someKey', fastn.binding.from(couldBeAnything), function(someValue, valueOfAnything){

    });

```

### A note on the difference between `properties` and `bindings`

On the surface, properties and bindings look very similar.
They can both be used like getter/setter functions, and they both emit change events.

They differ both in usage and implementation in that properties don't have any awareness of a model or paths,
and bindings don't have any awareness of components.

This distinction shines when you design your application with 'services' or 'controllers' that encapsulate models and how to interact with them.
Check out the example applications [search service](https://github.com/KoryNunn/fastn/blob/gh-pages/example/search.js) and
[search bar component](https://github.com/KoryNunn/fastn/blob/gh-pages/example/searchBar.js).
The service only deals with data, and the component only deals with UI.

# Browser Support

Fastn works in all the latest evergreen browsers.

Fastn *May* work in other browsers, but will almost certainly need a few polyfills like WeakMap, Map, WeakSet, Set, etc...
