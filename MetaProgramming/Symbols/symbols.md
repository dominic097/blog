# Symbols

A primitive data types that serves as unique identifier for object properties.

While ES6 has introduced many features that have been on developer's wish list for quite some time there are some new features and its benefits are less well known, so why wait lets dig-in ...

### Creating New Symbols

As Symbols  are primitive data type, they are created via a factory function Symbol as below,

```js
const mySymbol = Symbol('mySymbol');
```

Every time when the factory function is called, a new and unique symbol is created. The optional parameter is a descriptive string that is shown when printing the symbol really !!! Ya that's true it did't have no other purpose !!!

```js
let foo = Symbol();
let bar = Symbol();
foo === bar // false
```

By the above code snippet it is guaranteed that symbols are always unique  The label does not affect the value of the symbol as I mentioned above, but it is useful for debugging though, and is shown if the symbol’s `toString()` method is called. However it is possible to create a Symbols with same string, but that would be irrational as it serve no purpose and would probably just lead to confusion.

