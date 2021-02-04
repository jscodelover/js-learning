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


