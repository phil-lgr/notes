# JavaScript

### Number

Always use the radix for parseInt function:

    parseInt("12cows", 10);

Since some environment may interpret `"012"` as octal number.

___

### String 

Strings in js are composed of 16-bit character.

___

### Statements

For looping through Array or Objects, instead of using `for` or `for in` loops, one can use `Object.keys()` and `Array.prototype.every()`. The latter can take a callback function which must return a truthy value otherwhise every() stops.
`Object.keys(obj)` returns an array of the own enumerable properties of the passed object.

    Object.keys(obj)
        .every(function(element, index, array) {
            (obj[element]); // return the object property "element"
            (array[index]);
        return true;
    });
    
The main difference between `Object.keys` and `for in` is the former will only loop through *own* *enumerable* properties of the passed object. 

___

### Scope and Closures

#### Scope

_Where to look for things_

Example of how the JS engine parses variable and function declaration on the first pass (compilation):

    var foo = "bar"; // does foo exist in the scope ? let's create it.
    
    function bar() { // does bar exist in the scope ? let's create it.
        console.log(foo); // not a declaration
        var foo = "baz"; // another LHS (left handed assignment)
        console.log(foo); // not a declaration
    }
    
    function baz(foo /* foo is created as formal variable inside (declaration) of baz */) { // create baz 
        foo = "bam"; // not a declaration
        bam = "yay"; // can't imply anything about bam
    }
    
On the second pass (execution):

    var foo = "bar"; // does foo exist ? ok, assign it the "bar" value.
    
    function bar() { 
        console.log(foo); // what is the value of foo ? undefined
        var foo = "baz"; // ever heard of foo ? ok, assign it the "baz" value
        console.log(foo); // what is the value of foo ? "baz"
    }
    
    function baz(foo) {
        foo = "bam"; // know foo ? yes, assign it the "bam" value 
        bam = "yay"; // know bam ? no, go up one level to find out, creates it on the global in non-strict mode
    }
    
#### Closures

##### Singleton pattern returns one instance of an object containing 2 public methods:

    var singleton = (function () {
        var privateVariable;
        function privateFunction(x) {
            // private variable stuff
        }
        return {
            firstMethod: function(a, b) {
                // private variable
            },
            secondMethod: function(x) {
                // private function...
            }
        };
    }())

This immediately invoked function will return the 2 methods which will have access to private var and private function. This will be true as long as the returned object lives. Immediately invoked function also prevent the pollution of the GLOBAL variable.

##### Constructor pattern

    function constructor(spec) {
        var privateValue;
        // here we can call another constructor to inherit from
        var service = otherConstructor(spec),
            instance,
            method = function() {
                // spec, instance, method, privateValue
            };
        service.method = method;
        return service;
    }

The returned object have access to private *(close over...)* variables or private methods. Returned functions are pushed on the memory heap and *because of closure* the inner variables and methods that were linked to returned functions in the constructor *won't* get garbage collected. They will live as long as the returned object lives.

___

### Functions

#### `this` and `arguments`

In a function, `arguments` contains a pseudo array (only has a few methods) which contains all arguments that were passed in:

    function sum() {
        var total = 0, i, len = arguments.length;
        for(i = 0; i < len; i++) {
            total += Number(arguments[i]);
        }
        return total;
    }
    
`this` parameter contains a reference to the object of invocation. It allows a method to know what object it is concerned with.

    function() {
        (this); // if called in the global scope for example, this is the window object
    }
    
An example of lexical `this` with the `Function.Prototype.bind()` method
    
    function mod(){
        this.text = 'world';
        function helloUndefined() {
            return this.text;
        }
        function hello(){
          this.text = 'hello'
          return this.text;
        }
        function helloWithBind(){
            return this.text;
        }
        return {
            helloUndefined: helloUndefined,
            hello: hello,
            helloWithBind: helloWithBind.bind(this)
            }
    }
    
    var service = mod();
    console.log(service.helloUndefined()); // underfined
    console.log(service.hello()); // 'hello'
    console.log(service.helloWithBind()); // 'world'
   

#### `new` keyword aka constructor call

    function bar() {
        console.log('bar');
    }
    var foo = new bar(); // four things happen:
    // 1. brand new object is created
    // 2. newly created object is linked to another object (bar)
    // 3. newly created object is passed as `this` in the function call
    // 4. return this (the object)
    
Another example with `Date()`

    var date = Date()
    undefined
    var dateObject = new Date()
    undefined
    date.constructor
    ƒ String() { [native code] }
    dateObject.constructor
    ƒ Date() { [native code] }
    date.__proto__
    String {[[PrimitiveValue]]: "", length: 0, constructor: ƒ, charAt: ƒ, charCodeAt: ƒ, codePointAt: ƒ, …} // has Date as proto
    dateObject.__proto__
    {constructor: ƒ, toString: ƒ, toDateString: ƒ, toTimeString: ƒ, toISOString: ƒ, …} has String as proto
    
    
___

### Prototypes & Inheritance (ES5)

#### Class and Subclass with Object.create()

    function Car() {
        this.age = 12;
        this.grow = function grow() {
            this.age += 1;
        }
    }
    
    function Honda() {
        Car.call(this); // to inherit props
        this.age = 14;
    }
    
    Honda.prototype = Object.create(Car.prototype); // sets proto, but overrides the constructor;
    Honda.prototype.constructor = Honda; // this is probably what we want
    
    var honda1 = new Honda();
    honda1.age = 14;
    honda1.grow();
    console.log(honda1.age); // 15;
    
#### Constructor

A constructor makes an object linked to its own prototype.

#### Example of Protoype with Shadowing (skipping proto chain)

![image](https://user-images.githubusercontent.com/9644867/33003876-88df8de0-cd8b-11e7-8e0f-2ec78945c40e.png)

    // create function Foo with a prop called .prototype
    // the prototype's constructor is the function Foo
    // there is also a relationship between Foo's proto and Object.prototype
    function Foo(who) {
        this.me = who;
    }
    Foo.prototype.identify = function() {
        return "I am " + this.me;
    }

    // create a new object
    // link new object to Foo's proto
    // pass the object as this in Foo
    // return the object (same for a2)
    var a1 = new Foo("a1");
    var a2 = new Foo("a2");

    // assignment to the a2 object (not through the prototype chain)
    a2.speak = function() {
        alert("Hello, " + this.identify() + ".");
    }

    // a1 doesn't have a proto, but higher in the chain,
    // Foo.prototype is the first one up
    a1.constructor === Foo;
    a1.constructor === a2.constructor;

    a1.__proto__ === Foo.constructor; // access __proto__ goes to Foo.prototype
    Object.getPrototypeOf(a1); // IE 6-11 __proto__ is not available
    a1.__proto__ === a2.__proto__;

    // shadowing
    a1.identify = function() {
        Foo.prototype.identify.call(this); // <-- skip the prototype chain and call the 
        // prototype directly
    }

#### Object Linked To Object Example (OLOO)

![image](https://user-images.githubusercontent.com/9644867/33156910-3447f82e-cfcc-11e7-9bd8-3405ed354725.png)

    var Foo = {
        init: function (who) {
            this.me = who;
        },
        identify: function() {
            return "I am " + this.me;
        }
    };

    var Bar = Object.create(Foo);

    Bar.speak = function() {
        alert("hello, " + this.identify());
    };

    var b1 = Object.create(Bar);
    b1.init("b1");
    b1.speak(); // alerts: "hello, I am b1";
    
___

### Types

Native types: `number`, `boolean`, `object`, `undefined`, `string`

`function` is not a primitive type since it's based on object, but is callable.

#### NaN

    "a" / 2         // gives NaN
    isNaN()         // built-in method to check for NaN value (convert to number first)
    Number.isNaN()  // more robust method
    
___

### !!! - Faulty expression statement

    return
    
    {
        ok: true
    };

Because of automatic semicolons insertion, above code is equivalent to an expression statement :

    return;
    
    { 
        ok:true 
    };

Always write

    return {
        ok: true 
    };

___

### Type coercion of `==`

`==` does weird type coercion like

    '' == '0'           // false
    false == 'false'    // false
    " \t\r\n " == 0     // true
    
Always use `===`

___
