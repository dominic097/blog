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


