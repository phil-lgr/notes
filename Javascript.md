# Some gotchas.. in JS (ES5)

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

#### Fun with functions

##### Some simple unary functions:

    function add(a, b) {
      return a + b;
    }
    
    function sub(a, b) {
      return a - b;
    }
    
    function mul(a, b) {
      return a * b;
    }

##### A function that returns a function that returns the argument:

    function identityf(number) {
      return function() {
        return number;
      }
    }

##### A function that adds from two invocations:

    function addf(outer) {
      return function(inner) {
        return outer + inner;
      }
    }
    addf(3)(4);
    
##### A function that takes a binary function and makes it callable with two invocations
*currying*: decomposing multiple arguments functions into multiple functions that takes a single argument

    function liftf(bin) {
      return function(first) {
        return function(second) {
          return bin(first, second);
        }
      }
    }
    var addf = liftf(add);
    addf(3)(4);
    liftf(mul)(5)(6);
    
##### A function that takes a binary function and an argument and can pass another argument

    function curry(bin, first) {
      return liftf(bin)(first);
    }

    var add3 = curry(add, 3);
    add3(4)
    curry(mul, 5)(6);

*In ES6* we can use ellipsis:

    function curryES6(func, ...first) {
    // ellipsis ... takes all the args put them in an array and bind array to parameters
      return function(...second) {
        return func(...first, ...second);
      }
    }
    
##### Functions that add 1 to arguments:

    var inc = liftf(add)(1);
    var inc2 = curry(add, 1);
    var inc3 = addf(1);
    inc3(5);
    inc3(inc(5));
    
##### A function that takes a binary function that returns a unary function that passes its argument twice in the binary function:
    
    function twice(bin) {
      return function(number) {
        return bin(number, number);
      }
    }
    var doubl = twice(add);
    var square = twice(mul);
    doubl(11); 
    square(11);

##### A function that reverse the order of the arguments of a binary function:

    function reverse(bin) {
      return function(...args) {
        return bin(...args.reverse());
      }
    }

    var bus = reverse(sub);
    bus(3,2);
    
##### A function that takes two unary functions that returns a unary function that calls them both:

    function composeu(unary1, unary2) {
      return function(number) {
        return unary2(unary1(number));
      }
    }
    
    composeu(doubl, square)(5); // 100
    
##### A function that takes two binary functions that returns a function that calls them both:

    function composeb(bin1, bin2) {
      return function(a, b, c) {
        return bin2(bin1(a, b), c);
      }
    }
    
    composeb(add, mul)(2, 3, 7); // 35
    
##### A function that limits the number of times a binary function can be invoked:

    function limit(bin, limitN) {
      var counter = limitN || 1;
      return function(a, b) {
        if(counter) {
          counter -= 1;
          return bin(a, b);
        } else {
          return undefined;
        }
      }
    }
    
    var add_limited = limit(add, 2);
    add_limited(1, 2); // 3
    add_limited(1, 2); // 3
    add_limited(1, 2); // undefined
    
##### A generator function that produces a series of values:

    function from(number) {
      return function() {
        return number++; // function closes over the outer function parameter
      }
    }
    
    var index = from(0);
    index(); // 0
    index(); // 1
    
##### A function that takes a generator and a end value that produces value up to that end value:

    function to(gen, end) {
      return function() {
        var value = gen();
        if (value < end) {
          return value;
        }
      }
    }
    
    var index = to(from(1), 3);
    index(); // 1
    index(); // 2
    index(); // undefined
    
##### A function that takes a start and end value and produces a generator that will produce values in a range:

    function fromTo(begin, end) {
      return to(from(begin), end);
    }
    
    var index = fromTo(0, 3);
    index(); // 0
    index(); // 1
    index(); // 2
    index(); // undefined
    
##### A function that takes an array and a generator that returns a generator that will produce elements from the array:

    function element(array, fromToGen) {
      return function() {
        var index = fromToGen();
        if (index !== undefined) { // protects the function from the array containing the value *undefined*
          return array[index];
        }
      }
    }
    
    var ele = element(['a', 'b', 'c', 'd'], fromTo(1, 3));
    ele(); // b
    ele(); // c
    ele(); // undefined
    
With the generator optional:

    function element(array, fromToGen) {
      return function() {
        var value;
        // making the generator optional
        // if (fromToGen === undefined) {
        //   fromToGen = fromTo(0, array.length);
        // }
        // better, since we check if the passed argument is explicitely a function
        if (typeof(fromToGen) !== "function") {
          fromToGen = fromTo(0, array.length);
        }
        value = array[fromToGen()];
    
        if (value !== undefined) {
          return value;
        }
      }
    }
    
    var ele = element(['a', 'b', 'c', 'd']);
    ele(); // b
    ele(); // c
    
#### A function that takes a generator and an array and will produces a function that collects the result in the array:

    function collect(gen, array){
      return function(){
        var value;
        if(typeof(gen) !== "function"){
          throw "gen must be a generator";
        } else {
          value = gen();
          if(value !== undefined){
            array.push(value);
          }
          return value;
        }
      }
    }
    
    var array = [];
    var col = collect(fromTo(0, 2), array);
    col(); // 0
    col(); // 1
    col(); // undefined
    array; // [0, 1]
    
#### A filter function that takes a generation and a predicate (function that returns bool) and produces a generator that produces only the values approuved by the predicate:

    function filter(gen, predicate) {
      return function() {
        var value = gen();
        while (value !== undefined) {
          if (predicate(value)) {
            return value;
          }
          value = gen();
        }
      }
    }
    
    var fil = filter(fromTo(0, 5),
      function third(value) {
        return (value % 3) === 0;
      });
    
    fil(); // 0
    fil(); // 3
    fil(); // undefined
    
#### A concat function that takes two generators and produce a generator that put them in sequence:

    function concat(gen1, gen2) {
      return function() {
        var value;
    
        value = gen1();
    
        if (value !== undefined) {
          return value;
        }
    
        value = gen2();
    
        if (value !== undefined) {
          return value;
        }
    
      }
    }
    
    var con = concat(fromTo(0, 3), fromTo(0, 2));
    con(); // 0
    con(); // 1
    con(); // 2
    con(); // 0
    con(); // 1 
    con(); // undefined

#### A function that makes a function that takes a character string and generates unique symbols:

    function gensymf(char){
      var index = 1;
      return function(){
        return char+index++;
      }
    }
    
    var geng = gensymf("G"), genh = gensymf("H");
    
    geng(); // G1
    geng(); // G2
    genh(); // H1
    genh(); // H2
