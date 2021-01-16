# JS-learning
Common JS interview topics 

# Call, Bind and Apply
- The `this` keyword in JavaScript behaves differently compared to other programming languages. <br>
In other object-oriented programming languages, the `this` keyword always refers to the current instance of the class. Whereas in JavaScript, the value of `this` depends on how a function is called.

- We use `call`, `bind` and `apply` methods to set the `this` keyword independent of how the function is called. This is especially useful for the callbacks.
  ```
  const counter = {
   count: 0,
   incrementCounter: function() {
     console.log(this);
     this.count++;
   }
  }
 
  document.querySelector('.btn').addEventListener('click', counter.incrementCounter);
  ```
  In the above(callback) snippet, the `this` keyword refers to the DOM element where the event happened, not the counter object.
 
 - We know that functions are a special kind of objects in JavaScript. So they have access to some methods and properties. `Call`, `bind`, and `apply` are some of the methods that every function inherits.
 
- ### Bind( )
  - The bind method creates a new function and sets the `this` keyword to the specified object. 
  
    Syntax -
    ```
    function.bind(thisArg, optionalArguments)
    ```
  
    For Example - 
    ```
    const john = {
      name: 'John',
      age: 24, 
    };
    
    const jane = {
      name: 'Jane',
      age: 22,
    };
    
    function greeting() {
      console.log(`Hi, I am ${this.name} and I am ${this.age} years old`);
    }
    
    
    const greetingJohn = greeting.bind(john);
    
    // Hi, I am John and I am 24 years old
    greetingJohn();
    
    const greetingJane = greeting.bind(jane);
    
    // Hi, I am Jane and I am 22 years old
    greetingJane();
    ```
  
    Here `greeting.bind(john)` creates a new function with `this` set to `john` object, which we then assign to `greetingJohn` variable. Similarly for        `greetingJane`.
  
  - We can also use `bind` in case of callbacks and event handlers. 
    For example:
    ```
      const counter = {
        count: 0,
        incrementCounter: function() {
          console.log(this);
          this.count++;
        }
      }
      
      document.querySelector('.btn').addEventListener('click', counter.incrementCounter.bind(counter));
    ```
    
  - We can also pass extra arguments to the `bind` method. The general syntax for this is `function.bind(this, arg1, arg2, ...)`. For example:  
      ```
      function greeting(lang) {
         console.log(`${lang}: I am ${this.name}`);
      }
      
      const john = {
        name: 'John'
      };
      
      const jane = {
        name: 'Jane'
      };
      
      const greetingJohn = greeting.bind(john, 'en');
      // en: I am John
      greetingJohn();
      
      const greetingJane = greeting.bind(jane, 'es');
      // es: I am Jane
      greetingJane();
      ```
      
- ### Call ( )
  - The `call` method sets the `this` inside the function and immediately executes that function.
  
    Syntax -
    ```
    function.call(thisArg, arg1, agr2, ...)
    ```
  
    For Example - 
    ```
    function greeting() {
       console.log(`Hi, I am ${this.name} and I am ${this.age} years old`);
    }
    
    const john = {
       name: 'John',
       age: 24,
    };
    
    const jane = {
       name: 'Jane',
       age: 22,
    };
    
    // Hi, I am John and I am 24 years old
    greeting.call(john);
    
    // Hi, I am Jane and I am 22 years old
    greeting.call(jane);
    ```
    
    Above example is similar to the bind() example except that call() does not create a new function. 
    
- ### Apply ( )
  - The `apply()` method is similar to `call()`. The difference is that the `apply()` method accepts an array of arguments instead of comma separated values.
  
    Syntax -
    ```
    function.apply(thisArg, [argumentsArr])
    ```
  
    For Example - 
    ```
    function greet(greeting, lang) {
       console.log(lang);
       console.log(`${greeting}, I am ${this.name} and I am ${this.age} years old`);
    }
    
    const john = {
       name: 'John',
       age: 24,
    };
    
    const jane = {
       name: 'Jane',
       age: 22,
    };
    
    //en
    // Hi, I am John and I am 24 years old
    greet.apply(john, ['Hi', 'en']);
    
    // es
    // Hola, I am Jane and I am 22 years old
    greet.apply(jane, ['Hola', 'es']);
    ```  
 
- #### Note :  
     The `bind()` method creates a copy of the function and sets the `this` keyword, while the `call()` and `apply()` methods sets the `this` keyword and calls the function immediately. The `bind()` and `call()` methods accepts comma separated values whereas `apply()` method accepts array of arguments. 
     [For more detailed example.](https://ui.dev/this-keyword-call-apply-bind-javascript/)

- ### Polyfill for Bind
  ```
  let name = { firstName: 'Manisha', lastName: 'Basra'};

  let printName = function(arg, name){
    console.log(`${this.firstName} ${this.lastName} ${arg} ${name}`);
  }


  let printMyName = printName.bind(name, 'in Love');
  printMyName(['with javascript']);

  Function.prototype.myBind =  function(...args){
    const myThis = this;
    return function(arg){
      if(typeof arg === 'string' )
        myThis.apply(args[0], [...args.slice(1), arg]);
      else
        myThis.apply(args[0], [...args.slice(1), ...arg]);
    }
  }

  let printMyNameAgain = printName.myBind(name, 'in Love');
  printMyNameAgain(['with javascript from my bind method']);

  ```
  
 - ### Polyfill for Call
    ```
    let name = {
     firstName: 'Manisha',
     lastName: "Basra"
    }

    let printName= function(arg1, arg2){
      console.log(`${this.firstName} ${this.lastName}, ${arg1}, ${arg2}`);
    }

    Function.prototype.myCall = function(context, ...arg){
      // context with object reference
      console.log(context) 

      context.fn = this;
       
     // context with printName func as property so that `this` can refer to its current object
      console.log(context) 
      
      context.fn(...arg);  // to pass all the string arguments
    }

    printName.myCall( name, 'Hello', 'World!!');

    ```
      
 - ### Polyfill for Apply
    ```
    let name = {
     firstName: 'Manisha',
     lastName: "Basra"
    }

    let printName= function(arg){
     console.log(`${this.firstName} ${this.lastName} is going to learn ${arg.toString()}`);
    }

    Function.prototype.myCall = function(context, ...arg){
      // context with object reference
      console.log(context) 

      context.fn = this;
       
     // context with printName func as property so that `this` can refer to its current object
      console.log(context) 
      
      context.fn(arg);  // to pass single array argument
    }

    printName.myCall( name, ['React Native', 'Js concept']);

    ```
