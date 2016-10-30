# Notes/JavaScript
> examples from Douglas Crockford's "JavaScript: The Good Parts"

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

### Type coercion of `==`

`==` does weird type coercion like

    '' == '0'           // false
    false == 'false'    // false
    " \t\r\n " == 0     // true
    
Always use `===`

### Number

Always use the radix for parseInt function:

    parseInt("12cows", 10);

Since some environment may interpret `"012"` as octal number.

### String 

Strings in js are composed of 16-bit character.

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

### Functions

#### `this` and `arguments`

___

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
    
#### Examples of Function Closures

___


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

##### Constructor pattern:

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
