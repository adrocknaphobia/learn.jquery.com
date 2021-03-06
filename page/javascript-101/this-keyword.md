---
title:        The "this" Keyword
level:        beginner
---

In JavaScript, as in most object-oriented programming languages, `this` is a
special keyword that is used within methods to refer to the object on which a
method is being invoked. The value of `this` is determined using a simple series
of steps:

- If the function is invoked using `Function.call` or `Function.apply`, this will
  be set to the first argument passed to `call`/`apply`. If the first argument
  passed to `call`/`apply` is null or undefined, `this` will refer to the global
  object (which is the `window` object in Web browsers).
- If the function being invoked was created using `Function.bind`, `this` will be
  the first argument that was passed to `bind` at the time the function was
  created.
- If the function is being invoked as a method of an object, `this` will refer to
  that object.
- Otherwise, the function is being invoked as a standalone function not
  attached to any object, and `this` will refer to the global object.

``` js
// A function invoked using Function.call
var myObject = {
  sayHello : function() {
    console.log('Hi! My name is ' + this.myName);
  },

  myName : 'Rebecca'
};

var secondObject = {
  myName : 'Colin'
};

myObject.sayHello();                  // logs 'Hi! My name is Rebecca'
myObject.sayHello.call(secondObject); // logs 'Hi! My name is Colin'
```

``` js
// A function created using Function.bind
var myName = 'the global object',

    sayHello = function () {
      console.log('Hi! My name is ' + this.myName);
    },

    myObject = {
      myName : 'Rebecca'
    };

var myObjectHello = sayHello.bind(myObject);

sayHello();       // logs 'Hi! My name is the global object'
myObjectHello();  // logs 'Hi! My name is Rebecca'
```

``` js
// A function being attached to an object at runtime
    var myName = 'the global object',

        sayHello = function() {
          console.log('Hi! My name is ' + this.myName);
        },

        myObject = {
          myName : 'Rebecca'
        },

        secondObject = {
          myName : 'Colin'
        };

    myObject.sayHello = sayHello;
    secondObject.sayHello = sayHello;

    sayHello();               // logs 'Hi! My name is the global object'
    myObject.sayHello();      // logs 'Hi! My name is Rebecca'
    secondObject.sayHello();  // logs 'Hi! My name is Colin'
```

<div class="note">
When invoking a function deep within a long namespace, it is often tempting to
reduce the amount of code you need to type by storing a reference to the actual
function as a single, shorter variable. It is important not to do this with
instance methods as this will cause the value of `this` within the function to
change, leading to incorrect code operation. For instance:
</div>

``` js
var myNamespace = {
    myObject : {
      sayHello : function() {
        console.log('Hi! My name is ' + this.myName);
      },

      myName : 'Rebecca'
    }
};

var hello = myNamespace.myObject.sayHello;

hello();  // logs 'Hi! My name is undefined'
```


<div class="note">
You can, however, safely reduce everything up to the object on which the method is invoked:
</div>

```
var myNamespace = {
    myObject : {
      sayHello : function() {
        console.log('Hi! My name is ' + this.myName);
      },

      myName : 'Rebecca'
    }
};

var obj = myNamespace.myObject;

obj.sayHello();  // logs 'Hi! My name is Rebecca'
```
