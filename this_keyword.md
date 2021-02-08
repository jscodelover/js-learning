# this keyword
- `this` keyword is created by JS engine which points to the `window` object. `window` object is also created by JS engine into the global space.

- The value of `this` is determined by how a function is called (runtime binding). 
  It can't be set by assignment during execution, and it may be different each time the function is called. 
  
- It behave differently in non-strict and strict mode.
  In strict mode, `this` keyword refers to a value.
  In non strict mode, `this` keyword refers to a object.
  
  For example: in simple Function Call: 
  
  `this` is the `global object` in non-strict mode, and `undefined` in strict mode.
  
  NON-STRICT
  ```
    function ghost() {
      console.log(this.boo);
    }

    ghost(); // undefined

    const boo = '👻';

    ghost(); // 👻
  ```   
  
  STRICT
  ```
    'use strict';

    function ghost() {
      console.log(this.boo);
    }

    ghost(); // TypeError: this is undefined

    // the rest is not executed
    var boo = '👻';

    ghost();
  ```
  
#### Note: As a side note, variables declared with `let` or `const` at the global level are not stored in the global object, but instead in an inaccessible declarative environment record. 
    ```
      function ghost() {
        console.log(this.boo);
      }

      ghost(); // undefined

      let boo = '👻';

      ghost(); // undefined

      window.boo = '👻';

      ghost(); // 👻
    ```
    
  - ## Implicit or Default Binding of `this`
    
      - If we are in strict mode then the default value of `this` keyword is `undefined` otherwise `this` keyword act as `global` object, it’s called default binding of `this` keyword.
      
      - `this` points to the object on which the function is called (what’s to the left of the period when the function is called).
      ```
        function ghost() {
          console.log(this.boo);
        }

        let myGhost = {
          name: 'Casper',
          boo: '👻 Boo!!',
          ghost
        }

        myGhost.ghost(); // 👻 Boo!!
      ```
      ##### Note : It’s important to know how, when and from where the function is called, does not matter where function is declared.
   
  - ## Explicit and Fixed Binding of `this`
  
      - We can explicitly tell the JavaScript engine to set this to point to a certain value using `call`, `apply` or `bind`.


  - ## `new` Binding
      - The `new` keyword in front of any function turns the function call into constructor call and below things occurred when new keyword put in front of   function
      
        - A brand new empty object gets created
        - new empty object gets linked to prototype property of that function
        - same new empty object gets bound as this keyword for execution context of that function call
        - if that function does not return anything then it implicit returns this object.

        ```
        function bike() {
          var name = "Ninja";
          this.maker = "Kawasaki";
          console.log(this.name + " " + maker);  // undefined Bajaj
        }

        var name = "Pulsar";
        var maker = "Bajaj";

        obj = new bike();
        console.log(obj.maker);                  // "Kawasaki"
        ```
      
        - In the above code snippet, "bike" function is get called with `new` keyword in front of it. So, it creates a new object then that new object gets linked to  prototype chain of function "bike", after that the created new object bound to `this` object and function returns `this` object. That’s how the returned `this` object assigned to "obj" and `console.log(obj.maker)` prints “Kawasaki” .
      
        - In the above code snippet, "this.name" inside function "bike()" does not print “Ninja” or “Pulsar” instead it prints `undefined` because the name variable declared inside the function "bike()" and "this.name" are totally 2 different things. Same way "this.maker" and "maker" are different inside function "bike()" .

- ### Precedence of `this` keyword bindings
   - First it checks whether the function is called with new keyword.
   - Second it checks whether the function is called with call() or apply() method means explicit binding.
   - Third it checks if the function called via context object (implicit binding).
   - Default global object (undefined in case of strict mode).
