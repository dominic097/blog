# Reflections

An non-construct built-in object that provides methods for interceptable JavaScript operations

---

Reflections are all about Reflection through introspection - used to discover very low-level information about your code helping with forwarding default operations from the handler to the target. It goes hand in hand with proxies which is all about Reflection through intercession - wrapping Object and intercepting their behavior through traps.

_**Just Making things clear !!!**_

* Reflect is not a functional Object
* Reflect doesn't have a \[\[Construct\]\] internal method
* It is not possible to use the Reflect Object with the `new`  Operatior
* Reflect Object doesn't have a \[\[Call\]\] internal method
* It is not possible to invoke Reflect Object as a functional method____
____
The fallowing are the static functions that Reflection Object provides, It has the same names as the proxy handler methods where some of them are same as corresponding methods on Object.

#### Reflect.apply \( target, thisArgument, argumentsList \)

In ES5, we would typically use the `Function.prototype.apply` method to call function a function with a given context and arguments provided as Array with the below example 

```js
Function.prototype.apply.call(sumFn, null, [1, 2, 3, 4]) === 10;
```
Which is horrible, However we call also do,
```js
Function.prototype.apply.call(sumFn, null, [1, 2, 3, 4]) === 10;
```
But, this is still not a cleaner way. With Reflect.apply which is pretty much like _Function#**apply** / Function#**call**_ statement
```js
Reflect.apply(sumFn, null, [1,2,3,4]) === 10;
```
That beign said, It is mind blowing! how Reflect.appy can protect us from pitfall of _Function#**apply** / Function#**call**_ where any code could trivially change the functionalities of **_call_** or **_apply_** method leaving us stuck wiht broken code or horrible workarounds.

________

