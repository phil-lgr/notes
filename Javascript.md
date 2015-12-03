#### !!! - Faulty expression statement

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

#### Type coercion of `==`

`==` does weird type coercion like

    '' == '0'           // false
    false == 'false'    // false
    " \t\r\n " == 0     // true
    
Always use `===`

### Number

Always use the radix for parseInt function:

    parseInt("12cows", 10);

### String 

- 16-bit 
- 

### Statements

For looping through Array or Objects, instead of using `for` or `for in` loops, one can use `Object.keys()` and `Array.prototype.every()`. The latter can take a callback function which must return a truthy value otherwhise every() stops.
`Object.keys(obj)` returns an array of the own enumerable properties of the passed object.

    Object.keys(obj)
        .every(function(element, index, array){
            console.log(element);
            console.log(array[index]);
        return true;
    });
    
The main difference between `Object.keys` and `for in` is the former will only loop through *own* *enumerable* properties of the passed object. 
