# Reflections

An non-construct built-in object that provides methods for interceptable JavaScript operations

---

Reflections are all about Reflection through introspection - used to discover very low-level information about your code helping with forwarding default operations from the handler to the target. It goes hand in hand with proxies which is all about Reflection through intercession - wrapping Object and intercepting their behavior through traps.

_**Just Making things clear !!!**_

* Reflect is not a functional Object
* Reflect doesn't have a \[\[Construct\]\] internal method
* It is not possible to use the Reflect Object with the `new`  Operatior
* Reflect Object doesn't have a \[\[Call\]\] internal method
* It is not possible to invoke Reflect Object as a functional method

The fallowing are the static functions that Reflection Object provides, It has the same names as the proxy handler methods where some of them are same as corresponding methods on Object.

#### Reflect.apply \( target, thisArgument, argumentsList \)



