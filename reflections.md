# Reflections

An non-construct built-in object that provides methods for interceptable JavaScript operations

---

Reflections are all about Reflection through introspection - used to discover very low-level information about your code helping with forwarding default operations from the handler to the target. It goes hand in hand with proxies which is all about Reflection through intercession - wrapping Object and intercepting their behavior through traps.

_**Just Making things clear !!!**_

* Reflect is not a functional Object
* Reflect doesn't have a \[\[Construct\]\] internal method
* It is not possible to use the Reflect Object with the `new`  Operatior
* Reflect Object doesn't have a \[\[Call\]\] internal method
* It is not possible to invoke Reflect Object as a functional method_\_\_\_

---

The fallowing are the static functions that Reflection Object provides, It has the same names as the proxy handler methods where some of them are same as corresponding methods on Object.

#### Reflect.apply `( target, thisArgument, argumentsList )`

In ES5, we would typically use the `Function.prototype.apply` method to call function a function with a given context and arguments provided as Array with the below example

```js
Function.prototype.apply.call(sumFn, null, [1, 2, 3, 4]) === 10;
```

Which is horrible, However we call also do,

```js
Function.prototype.apply.call(sumFn, null, [1, 2, 3, 4]) === 10;
```

But, this is still not a cleaner way. With Reflect.apply which is pretty much like _Function\#**apply** / Function\#**call**_ statement

```js
Reflect.apply(sumFn, null, [1,2,3,4]) === 10;
```

That being said, It is mind blowing! how Reflect.appy is less verbose and most importantly it protect us from the pitfalls of _Function\#**apply** / Function\#**call**_ where any code could trivially change the functionalities of _**call**_ or _**apply**_ method leaving us stuck wiht broken code or horrible workarounds.

#### Reflect.construct `(target, argumentsList[, newTarget])`

A static method which act like the `new` operator as a function. It is equivalent to calling new target\(...args\). This will work with Classes, and sets up the correct object so that Constructors have the right this object with the matching prototype. In ES5 we would use `Object.create()`instead and pass that to _**apply**_ or _**call**_ methods. An Example,

```js
class Auto {
    constructor(model) {
        this.model = model;
    }
    getModelNumber() {
        return "Brand VW: Model: " + this.model;
    }    
}

//ES5 factory  method implementation
let instance = Object.create(Auto.prototype); // creating a factory 
instance.getModelNumber.call({model:"Pollo"}); // would print "Brand VW: Model: Pollo"

//ES6 using Reflect
let instanceByReflect = Reflect.construct(Auto, ["Pollo"]); //creating facory
instanceByReflect.getModelNumber(); // would print "Brand VW: Model: Pollo"

//Applying Reflect to achive sub-classing technique
function someConstructor() {}
var result = Reflect.construct(Array, [], someConstructor);

Reflect.getPrototypeOf(result); // someConstructor.prototype
Array.isArray(result); // true
```

Thus with the help of Reflect.construct we not only attain subclassing inheritance but also a neat code structure which can easily blunt with modern code practice.

#### Reflect.defineProperty `( target, propertyKey, attributes )`

Reflect.defineProperty\(\) method is like Object.defineProperty\(\) which lets you to define metedata about the property. but returns a Boolean.  _NOTE: \_Throws a _**TypeError **\_is if target is not an Object. An Example,

```js
var obj = {};
Reflect.defineProperty(obj, "x", {value: 7}); // true
obj.x; // 7
```

#### Reflect.getOwnPropertyDescriptor `( target, propertyKey )`

This, once again, pretty much replaces `Object.getOwnPropertyDescriptor,` getting the descriptor metadata of a property if it exists on the object, undefined otherwise. An Example,

```js
Reflect.getOwnPropertyDescriptor(obj, "x");
// would return Object {value: 7, writable: false, enumerable: false, configurable: false}
```

The _Reflect.getOwnPropertyDescriptor_ intern calls the _Object.getOwnPropertyDescriptor_ but the difference is,

```js
// would return Uncaught TypeError: Reflect.getOwnPropertyDescriptor called on non-object
Reflect.getOwnPropertyDescriptor('foo', 'f');

Object.getOwnPropertyDescriptor('foo', 'f'); // would return just 'undefined'
```

#### Reflect.has`( target, propertyKey )`

_Reflect.has_ is an interesting one, because it is essentially the same functionality as the in operator \(outside of a loop\). Both use the \[\[**HasProperty**\]\] internal method and returns an Boolean indicating whether or not the target has the property. An Example,

```js
Reflect.has({x: 0}, "x"); // true
Reflect.has({x: 0}, "y"); // false

// returns true for properties in the prototype chain 
Reflect.has({x;0}, "toString"); // true
```

#### Reflect.get`(target, propertyKey[, receiver])`

_Reflect.get_ method works like getting a property from an object \(target\[propertyKey\]\) as a function. An Example,

```js
// Object
var obj = { a: 1, b: 2 };
Reflect.get(obj, "a"); // 1

// Array
Reflect.get(["zero", "one"], 1); // "one"
```

#### Reflect.set`(target, propertyKey, value[, receiver])`

_Reflect.set_ method works like setting a property on an object. An Example,

```js
// Object
var obj = {};
Reflect.set(obj, "prop", "value"); // true
obj.prop; // "value"

// Array
var arr = ["duck", "duck", "duck"];
Reflect.set(arr, 2, "goose"); // true
arr[2]; // "goose"
```

#### Reflect.ownKeys`(target)`

_Reflect.ownKeys_ method wroks similar like Object.Keys\(\) returns an array of the target object's own property keys. An Example,

```js
Reflect.ownKeys({z: 3, y: 2, x: 1}); // [ "z", "y", "x" ]
Reflect.ownKeys([]); // ["length"]

//Working with Symbols 
let left = Symbol.for("LEFT");
let right = Symbol.for("RIGHT");

let obj = {left: 'left Node', right: 'right node, node: 1};
Reflect.ownKeys(obj); // [Symbol(LEFT), Symbol(RIGHT), "node"]
```

#### Reflect.preventExtensions\(\)

The static _Reflect.preventExtensions\(\)_ method prevents new properties from ever being added to an object similar to \_Object.preventExtensions\(\) \_with an diffrence Reflect.preventExtensions can not be used against non-object value which is not the case with Object.preventExtensions. An Example,

```js
var Obj = {x:1};
Reflect.isExtensible(Obj); // === true

// ...but that can be changed.
Reflect.preventExtensions(Obj);
Reflect.isExtensible(Obj); // === false

Obj.y = 1;
console.log(Obj); // would print {x:1}

// The difference
Reflect.preventExtensions(1);// TypeError: 1 is not an object
Object.preventExtensions(1); // would return 1
```

#### Reflect.getPrototypeOf `( target )`

Again, The Static method _Reflect.getPrototypeOf\(\) \_is same as \_Object.getPrototypeOf\(\)_. It returns the prototype \(i.e. the value of the internal \[\[**Prototype**\]\] property\) of the specified object. An Example,

```js
Reflect.getPrototypeOf({}); // Object.prototype

class Auto {}

Reflect.getPrototypeOf(new Auto()) == Auto.prototype; // True
```



