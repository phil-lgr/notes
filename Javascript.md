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
