# Proxies

Proxies allow us to define JS objects in a way that were impossible before.

---

Proxies are lower level functionality introduced as a part of **ES6, **By that They alter the default behavior of Javascript by a wrapper function called **Traps** aka _Proxy handlers_. These handlers have an opportunity to perform extra logic on top of native function before forwarding to the original target/wrapped object.

Again as I described above, Proxies a bility to modify the default behavior of Javascript within itself is one of the ingredients that amuses me toward Javascript.

Let's dig in with a popular example, getter \[\[get\]\] and setter \[\[set\]\]. which allows us to intercept get & set operations with custom behavior.

```js
let targetObj = {a:1, b:2, c:3},
    handler = {
        get: (target, key, context) => {
            return target[key] + ' from Proxy';
        },
        set: (target, key, value) => {
            console.log("setting " + value + " @" + key + " By Proxy set handler");
            target[key] = value;
        }        
    },
    proxyObj = new Proxy(targetObj, handler);

    // Accessing targetObj Properties
    targetObj.a // will return '1'
    targetObj.b // will return '2'

    /** NOTE: by accessing targetObj will not return the postfix text we added, 
    *   since we're acessing the non-proxied object. however by accessing
    *   the proxyObj the o/p will be different as shown below 
    */

    proxyObj.a // will return `1 from Proxy`
    proxyObj.d = 4 // will console `setting 4 @d By Proxy set handler`
```

By the above example,

* The proxy handlers _\(get & set\)_ each intercept the operation when a respective meta-programming task is performed on the targetObj.
* The `new Proxy()` invoke constructor of type intrinsic object _%FunctionPrototype%_ meaning, proxy is an exotic objects which do not have a \[\[Prototype\]\] internal slot that requires initialization.
* Being said that proxy do not have prototype chain, proxyObj do have below intrinsic object, 
  * \[\[Handler\]\]    -&gt; Handler obj -&gt; has get & set trap function.
  * \[\[Target\]\]       -&gt; targetObj -&gt; has a,b,c properties.
  * \[\[IsRevoked\]  -&gt; By debaut false.

**Proxy's Internal Method's and Internal Handler's**

As we know every proxy object is an exotic object whose essential internal methods are partially implemented using ECMAScript. Every proxy objects has an internal slot called \[\[ProxyHandler\]\] aka trap / handler hook function callbacks. The value of \[\[**ProxyHandler**\]\] is an object, called the proxy’s handler object or **null **by default. Every proxy object also has an internal slot called \[\[**ProxyTarget**\]\] whose value is either an object or **null **value. This object is called the proxy’s target object as in our case it's _targetObj_ as per above example.

The below table \[_Proxy Handler Methods_\] shows the internal method's and it's respective Handler Method's where some of them will be implemented in upcoming examples,

| Internal Method | Handler Method |
| :---: | :---: |
| \[\[GetPrototypeOf\]\] | getPrototypeOf |
| \[\[SetPrototypeOf\]\] | setPrototypeOf |
| \[\[IsExtensible\]\] | isExtensible |
| \[\[PreventExtensions\]\] | getOwnPropertyDescriptor |
| \[\[HasProperty\]\] | has |
| \[\[Get\]\] | get |
| \[\[Set\]\] | set |
| \[\[Delete\]\] | deleteProperty |
| \[\[OwnPropertyKeys\]\] | ownKeys |
| \[\[Call\]\] | apply |
| \[\[Construct\]\] | construct |
| \[\[DefineOwnProperty\]\] | defineProperty |
| \[\[Enumerate\]\] | enumerate |
| \[\[GetOwnProperty\]\] | getOwnPropertyDescriptor |

### Creating a Revocable Proxies

Besides creating common Proxies, we can also create temporary revocable proxies, which can be dismantle any time we want. To create a revocable Proxy, we need to use `Proxy.revocable(target, handler)` \(_instead of new Proxy\(target, handler\)_\), and instead of returning the Proxy directly, it’ll return an Object that would looks like `{ proxy, revoke()<Function> },` An Example,

```js
let targetObj = {a:1, b:2, c:3},
    handler = {
        get: (target, key, context) => {
            return target[key] + ' from Proxy';
        },
        set: (target, key, value) => {
            console.log("setting " + value + " @" + key + " By Proxy set handler");
            target[key] = value;
        }        
    },
    {proxy, revoke} = Proxy.revocable(targetObj, handler);

    proxy.a; // console / return '1 from Proxy'

    revoke(); // Revoking or Disabling proxy 

    proxy.a // Uncaught TypeError: Cannot perform 'get' on a proxy that has been revoked at <anonymous>:1:6
```

#### Proxies as prototypes {#proxies-as-prototypes}

As we know Proxies don't have a prototype but it doesn't mean it can be used as a prototype,  By extending the above example with `targetObj` and `handler`

```js
let obj = Object.create(proxyObj);

obj.a // console "1 from Proxy"
```

By the above example, we have now extended the object's Prototype using proxy object via [_Explicit Prototype_](https://msdn.microsoft.com/en-us/library/hh924508(v=vs.94).

**Using Proxies to hide all Properties **

We can use Proxies to make every property in an object completely hidden, except for when getting the value. All ways we can find out if a property exists on an Object in JavaScript:

1. _Reflect.has_,_Object.hasOwnProperty_,_Object.prototype.hasOwnProperty_, and the _in_ operator all use \[\[**HasProperty**\]\]. Proxy can trap this with `has`.
2. _Object.keys_/_Object.getOwnPropertyNames_, which uses \[\[**OwnPropertyKeys**\]\]. Proxy can trap this with `ownKeys`.
3. _Object.entries _\(an upcoming ES2017 feature\), also uses \[\[**OwnPropertyKeys**\]\] - again - trapped by ownKeys.
4. _Object.getOwnPropertyDescriptor_ which uses \[\[**GetOwnProperty**\]\]. Proxy can trap this with, surprise surprise, getOwnPropertyDescriptor.

that being said, with this kind of implementation, we can totally hide the properties of an Object. An Example

```js
let propertiesToHide = ['a', 'c']; // To hide properties `a` and `c
let targetObj = {a:1, b:2, c:3},
    handler = {
      has(target, property) {
        if (propertiesToHide.indexOf(property) != -1) {
          return false;
        }
        return target[property]; // Best Method use Reflect.has(target, property);
      },
      ownKeys(target) {
        return Object.ownKeys(target).filter( // Best Method use Reflect.ownKeys..
          (property) => propertiesToHide.indexOf(property) == -1
        );
      },
      getOwnPropertyDescriptor(target, property) {
        if (propertiesToHide.indexOf(property) != -1) {
          return undefined;
        }
        return Object.getOwnPropertyDescriptor(target, property);// Best Method use Reflect.getOwnPropertyDe..
      }
    },
    proxyObj = new Proxy(targetObj, handler);

    Object.keys(proxyObj) // console ['b'];
    proxyObj.hasOwnProperty('a') // would return false;
    proxyObj.hasOwnProperty('b') // would return true;
    Object.getOwnPropertyNames(proxyObj) // would return ['b'];

    proxyObj.a; // would return 1;
```

NOTE: _By the above implementation, we can only hide the properties of an Object that it holds, its value is still accessible._

_**Binggo **_**!!!**  **some of the use cases for proxies**

1. Tracing property accesses:  
   Consider a naive example, where we wanna trace out all the properties of an Object that are accessed/modified. To demonstrate how that works, let’s create a class and trace accesses to the properties of an instance.

   ```js
   let targetObj = {a:1, b:2, c:3};  
    tracePropAccessFn = (target, props) => {  
    let propKeySet = new Set(...props);  
        return new Proxy(target, {  
            get(target, propKey, receiver) {  
                if (propKeySet.has(propKey)) {  
                    console.log('GET '+propKey);  
                }  
                return Reflect.get(target, propKey, receiver); // If not Reflect can use target[propKey]  
            },  
            set(target, propKey, value, receiver) {  
                if (propKeySet.has(propKey)) {  
                    console.log('SET '+propKey+' = '+value);  
                }  
                return Reflect.set(target, propKey, value, receiver);// If not Reflect can use target[propKey]

            },
        });
    },
    proxyObj = tracePropAccessFn(targetObj, Object.keys(targetObj));

    proxyObj.a // would console `GET` a & return value '1'

    proxyObj.a = 100; // would console `SET a = 100` & return '100'
   ```

2. Tackling unknown properties:



