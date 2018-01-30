# Topics

* Different ways to create Objects in Javascript.
* Javascript Object property descriptors.
* What are Object prototypes.
* `this` keyword demystified.
* Sharing data between components.
* Content Projection.
* Accessing child elements.
* Directives.
* Host Listener.
* Host Binding.

## Different ways to create objects in Javascript

### 1. Object literals

```javascript
    var a = {};
    var b = {
        name: 'John',
        age: 20
    };
```

### 2. `Object.create(prototype)`

```javascript
    var c = Object.create(null);
    var d = Object.create({name: 'John', age: 21});
```

Creates object from the provided prototype.

### 3. Using a function prototype*

```javascript
    var Fn = function (n, a) {
        this.name = n;
        this.age = a;
    };

    var e = new Fn('John', 20);
```

## Javascript Object property descriptors

Object property descriptors define properties of an object property,
some of the important descriptors are

* `configurable`
* `writable`
* `value`
* `enumerable`

To access the property descriptors we can use `Object.getOwnPropertyDescriptor()`

```javascript
var f = {
    name: 'jon',
    age: '20'
};

// to obtain individual property descriptors
Object.getOwnPropertyDescriptor(f, 'name');
Object.getOwnPropertyDescriptor(f, 'age');

// to obtain all property descriptors of an object as an array
Object.getOwnPropertyDescriptors(f);
```

To set property descriptors, we can use
`Object.defineProperty()`

```javascript
var f = {
    name: 'jon',
    age: '20'
};

// to make the name property immutable
Object.defineProperty(f, 'name', { writable: false });

// to make the name mutable again
Object.defineProperty(f, 'name', { writable: true });

// to make name property immutable PERMANENTLY
Object.defineProperty(f, 'name', { writable: false, configurable: false });

// will not work
Object.defineProperty(f, 'name', { configurable: true });
Object.defineProperty(f, 'name', { writable: true });

// to make all properties of an object immutable
Object.freeze(f);   // uses Object.defineProperty under the hood
```

When a object property's enumerable property descriptor is set as `false`, it can only be accessed by using `Object.getOwnPropertyNames()`

```javascript
    var z = {
        name: 'Bill',
        age: 30
    }

    Object.keys(z);                     // ['name', 'age']

    Object.defineProperty(z, 'name', { enumerable: false });
    Object.keys(z);                     // ['age']
    
    // to get all properties (including non-enumerable properties)
    Object.getOwnPropertyNames(z);      // ['name', 'age']
```

## Object Prototypes

Javascript follows prototypal inheritance, objects inherit directly from objects (not from a class definition)

`Object.getPrototypeOf()` can be used to get the prototype of an object

```javascript
    var b = {
        name: 'John',
        age: 20
    };
    Object.getPrototypeOf(b); // returns {} (null prototype)

    var d = Object.create({name: 'John', age: 21});
    Object.getPrototypeOf(d); // returns {name: 'John', age: 21}
```

## `this` keyword

In the global context `this` refers to the javascript runtime's global object.

When used in functions, the value of `this` depends on how the enclosing function is called and also if **strict mode** is enabled/disabled.

```javascript
    function f1() {
    return this;
    }

    // In a browser:
    f1() === window; // true 
```

```javascript
    function f2() {
        'use strict';
        return this;
    }

    f2() === undefined; // true
    window.f2() === window // true
```

What if we want to call the function with a different execution context ?

```javascript

    function add() {
        console.log(this.a + this.b);
    }

    var obj = {a: 12, b: 20};

    // doesn't work
    obj.add();

    // works
    add.call(obj);
    add.apply(obj);

```

Difference between `call` and `apply`

```javascript
    function greet(msg) {
        console.log(msg + ' ' + this.name);
    }

    name = 'Jon';
    greet('Hello');     // prints 'Hello Jon';

    obj = { name: 'Smith'};

    greet.call(obj, 'Hello');   // call expects plain arguments
    greet.apply(obj, ['Hello']);    // apply expects array as the second argument which will be unpacked
```
