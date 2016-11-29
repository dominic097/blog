# Symbols

A** primitive data** types that serves as unique identifier for object properties and Hooyah !!! its **Immutable**

---

While ES6 has introduced many features that have been on developer's wish list for quite some time there are some new features and its benefits are less well known, so why wait lets dig-in ...

### Creating New Symbols

As Symbols are primitive data type, they are created via a factory function Symbol as below,

```js

let symbol = Symbol('symbol');

```

Every time when the factory function is called, a new and unique symbol is created. The optional parameter is a descriptive string that is shown when printing the symbol really !!! Ya that's true it did't have no other purpose !!!

```js

let foo = Symbol();

let bar = Symbol();

foo === bar // false

```

By the above code snippet, it is guaranteed that symbols are **always unique** The label does not affect the value of the symbol as I mentioned above, but it is useful for debugging though, and `Symbol()`is shown if the symbol’s `toString()` method is called. However it is possible to create a Symbols with same string, but that would be irrational as it serve no purpose and would probably just lead to confusion.

```js

let symbol1 = Symbol('Hello');
let symbol2= Symbol('World');
symbol1 === symbol2 // false
console.log(symbol2) // <== World

```

### **Usage**

well as you have gessed by now, it can be used as keys of properties which is a best fit for

* To Create Non-Public Properties.

* To keep Meta-level Propertied from clashing with global \/ base level properties.

* To create Immutable data types.


Therefore we can create an unlimited number of unique Symbols to an Object and be guaranteed that there will be no conflict with string Keys ot other unique Symbols

#### Symbols as property keys

```js
var node = {};
const LEFT = Symbol('left');
const RIGHT = Symbol('right');

// const RIGHT = new Symbol('right'); //TypeError 
...
//creating a node for double linked list
node = {LEFT: null, data: d, Right: null};

//using bracket notaion, 
node[LEFT] = null;
node[RIGHT] = null;
```

A method definition can also have a computed key:

```js
const getAttr = Symbols();
const node = {
    [getAttr]() {
        return 'Hello getAttr;'
    }
}

console.log(node[getAttr]()); // <== Hello getAttr;
```

Given the fact that the `Symbols` can be used as property keys, it has re-defined the terminology as `Property Keys are either strings or Symbols` in ES6 hence symbol-valued property keys are called _property symbols_

```js
const node = {
    [Symbol('enumSample')] : null,
    data: {},
    sampleTxt: "Hi There !!! "
}

//Ignores symbol-valued property keys
 Object.getOwnPropertyNames(node) // <== ['data', 'sampleTxt']
 Object.keys(node) // <== ['data', 'sampleTxt']

//Ignores string-values property keys:
Object.getOwnPropertySymbols(obj) // <== [Symbol(enumSample)]

```

### **The Global Registry**

The Es6 specification defines a runtime-wide symbol registry, which enables one can store and retrieve symbols across different contexts, such as between a document and an embedded iframe or service worker.

When the Global Registry is defnied it would have the fallowing data-structure

* **\[\[Keys\]\] **- Array of Keys used to define Symbols
* **\[\[Symbols\]\] **- Array of Symbols registered globally 

#### **Symbol.for\(key\)**

In contrast of `Symbol()`, the `.for(key)` function searches for existing symbols in a runtime-wide symbol registry `[[Symbols]]` with the given key and returns it if found. Otherwise a new symbol is created in the global symbol registry with this key.

```js
'use strict;'

Symbol.far('node.LEFT'); // would create a new global Symbol
Symbol.far('node.LEFT'); // retieve the already available Symbol from Global registry [[Symbols]]

// Global Symbol of same string are alway equal
Symbol.far('node.LEFT') == Symbol.far('node.LEFT'); // true
// however  not the same for local context
Symbol('node.LEFT') == Symbol('node.LEFT'); // false

const stmNodeLeft = Symbol.for("node.LEFT");
symNodeLeft.toString(); //<== "Symbol(node.LEFT)"

```

#### **Symbol.keyFor\(key\)**

The .Keyfor\(\) function retrieves a shared symbols key from the global symbol registry `[[Keys]]` for the given symbols

```js
let node_left = Symbol.for("node.LEFT");
Symbol.keyFor(node_left); // <== "node.LEFT"

let localSym = Symbol();
Symbol.keyFor(localSym); // <== undefined
```

### _`@@iterator`_ to make the object Iterable

Symbol.iterator probably a well-known symbol specifies the default iterator for an object. It allows to define how the object should be iterated using for...of statement or consumed by ... spread operator.

Many built-in types like strings, arrays, maps, sets are iterables, i.e. they have an _ @@iterator_ function whose behaviour can be tamed by using Symbols

_**@@iterator**_ method is called with no arguments, and the returned iterator is used to obtain the values to be iterated.

Some built-in types have a default iteration behavior, while other types \(such as Object\) do not. The built-in types with a _@@iterator_ method are:

* Array.prototype

* TypedArray.prototype

* String.prototype

* Map.prototype

* Set.prototype


### **Usage**

The following example creates an iterable object of a Linked list

```js
class linkedList {

    node(d, prev, next) {
        return {[PREV]: prev, 'data': d, [NEXT]: next};
    }

    [Symbol.iterator]() { 
        var i = 0,
         ...
         return { 
            next: () => { 
                if (i++ > 0) 
                    _nodeToReturn = (_nodeToReturn[NEXT] ? _nodeToReturn[NEXT] : null); 
                return { 
                    value: <return value>, done: <Boolean>
                   }
             }
           };
     }

...

}

// returns an Iteratable object
var obj = new linkedList();

for (var o of obj) {
   length++;
}

```

As per the above example _**@@iterator**_ property also accepts a generator function, which makes it even more valuable. The generator function returns a generator object, which conforms to iterator protocol.

If the primitive type or object has an @@iterator method, then it can be applied in the following constructs:

* Iterate over the elements in **for...of** loop
* Create an array of elements using spread operator **\[...iterableObject\]**
* Create an array of elements using **Array.from\(iterableObject\)**
* In **yield\* **expression to delegate to another generator
* In constructors for **Map**\(iterableObject\), **WeakMap**\(iterableObject\), **Set**\(iterableObject\), **WeakSet**\(iterableObject\)
* In promise static methods **Promise.all\(iterableObject\)**,** Promise.race\(iterableObject\)**

### _`@@hasInstance`_ to customize _`instanceof`_
By default the _instanceof_ operator verifies if the prototype chain of an Object contains that instance, For Example: 

```js
function Constructor() {  
  // constructor code
}
let obj = new Constructor();  
let objProto = Object.getPrototypeOf(obj);  
objProto === Constructor.prototype; // => true  
obj instanceof Constructor;         // => true  
obj instanceof Object;              // => true  
```

As per the above Ex, the obj _`instanceof`_ evaluates to true as the prototype of the Object equals _`Constructor.prototype`_

Unfortunatly Often an application does not deal with prototypes and requires a more specific instance verification thus comes the _`Symbol.hasInstance`_ a well-known symbol which can be used to determine if a constructor object recognizes an object as its instance. For Ex,

```js
class MyArray { 
    static [Symbol.hasInstance](instance) { 
        return Array.isArray(instance); 
    }
}

console.log([] instanceof MyArray); // <== true

```

### _`@@isConcatSpreadable`_ to convert an object to flat array elements
As the name suggest it is a Boolean valued property indicating if an Object can be flattened to an Array Elements by _`Array.prototype.concat()`_ function 

```js
// Array of names 
const name = ["Jhone", "Dominic", "Sam", "Venkat"];

// Array of Age corresponding to the name array
const Age = [48, 24, 40, 24];

name.concat([], Age) // <== ["Jhone", "Dominic", "Sam", "Venkat", 48, 24, 40, 24]

Age[Symbol.isConcatSpreadable] = false;

name.concat([], Age); // <== ["Jhone", "Dominic", "Sam", "Venkat", [48, 24, 40, 24]]

```

As per the above example, by setting `_isConcatSpreadable_` as _false_ keeps the array _Age_ intact in the concatenation result `["Jhone", "Dominic", "Sam", "Venkat", [48, 24, 40, 24]]`

