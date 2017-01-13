# Meta Programming

_Enabling self-adapting code_

Meta-Programming \[MP\] an art of writing program with an ability to rewrite itself. `eval` a part of JS since the begin of Time\(ES1\) is a gimmick for MP.

In JS6 MP can be achieved with

* ##### [Symbols](https://dominic097.github.io/blog/symbols.html "Click to know more on Symbols in JavaScript")
* ##### [Proxies](https://dominic097.github.io/blog/proxies.html "Click to know more on Proxies in JavaScript")
* ##### Reflections(https://dominic097.github.io/blog/reflections.html "Click to know more on Proxies in JavaScript")

# [Symbols](https://dominic097.github.io/blog/symbols.html "Click to know more on Symbols in JavaScript")

Symbols are new primitive types just like Number, String, and Boolean. They are tokens that serves as unique ID's for object properties

# [Proxies](https://dominic097.github.io/blog/proxies.html "Click to know more on Proxies in JavaScript")

In a nutshell proxies are used to define custom behavior for fundamental operations

By default, proxies don’t do much – in fact they don’t do anything. If you don’t set any “options”, your proxy will just work as a pass-through to the `target` object

# [Reflection](https://dominic097.github.io/blog/reflections.html "Click to know more on Proxies in JavaScript")

Reflect are build-in non constructable JS-object providing options to self-inspect and manipulate its member attributes and methods

