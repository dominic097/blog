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

A static method which act like the _`new`_ operator as a function. It is equivalent to calling new target\(...args\). This will work with Classes, and sets up the correct object so that Constructors have the right this object with the matching prototype. In ES5 we would use `Object.create() `instead and pass that to _**apply**_ or _**call**_ methods. An Example,

```js
class Auto {
    constructor(model) {
        this.model = model;
    }
    getModelNumber() {
        return "Brand VW: Model: " + this.model;
    }    
}
```



