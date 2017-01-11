# Proxies

Proxies allow us to define JS objects in a way that were impossible before.

---

Proxies are lower level functionality introduced as a part of **ES6, **By that They alter the default behavior of Javascript by a wrapper function called **Traps** aka _Proxy handlers_. These handlers have an opportunity to perform extra logic on top of native function before forwarding to the original target/wrapped object.

Again as I described above, \_Proxies a\_bility to modify the default behavior of Javascript within itself is one of the ingredients that amuses me toward Javascript.

Let's dig in with a popular example, getter \[\[get\]\] and setter \[\[set\]\]. which allows us to intercept get & set operations with custom behavior.

```js
let targetObj = {a:1, b:2, c:3},
    handler = {
        get: (target, key, context) => {
            return target[key] + ' from Proxy';
        },
        set: (target, key, value) => {
            console.log("setting " + value + " @ " + key + " By Proxy set handler");
            target[key] = value;
        }        
    },
    proxyObj = new Proxy(targetObj, handler);

    // Accessing targetObj Properties 
    
    targetObj.a // will return '1' 
    targetObj.b // will return '2'
    
    NOTE: by a since we're acessing the non-proxified object
```



