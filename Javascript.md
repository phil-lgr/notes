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
