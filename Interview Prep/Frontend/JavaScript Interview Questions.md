# JavaScript Interview

- [JavaScript Interview](#javascript-interview)
  - [How JavaScript Works & Execution Context ?](#how-javascript-works--execution-context-)
    - [How JavaScript code is executed ?](#how-javascript-code-is-executed-)
  - [Hoisting](#hoisting)
  - [Functions and Variable Environment](#functions-and-variable-environment)
    - [Code flow in terms of execution context](#code-flow-in-terms-of-execution-context)
  - [Shortest JS Program, Window and the this keyword](#shortest-js-program-window-and-the-this-keyword)
  - [Undefined vs Not Defined](#undefined-vs-not-defined)
  - [Scope, Scope Chain and Lexical Environment](#scope-scope-chain-and-lexical-environment)
  - [let & const, Temporal Dead Zone](#let--const-temporal-dead-zone)
  - [Block Scope, Shadowing](#block-scope-shadowing)
  - [Closures](#closures)
    - [Advantages of Closure](#advantages-of-closure)
    - [Disadvantages of Closure](#disadvantages-of-closure)
    - [Closures + setTimeout()](#closures--settimeout)
    - [Closure Output Questions](#closure-output-questions)
  - [Types of functions and First Class Functions](#types-of-functions-and-first-class-functions)
    - [Function Statement/ Declaration](#function-statement-declaration)
    - [Function Expression](#function-expression)
    - [Difference b/w function statement and function expression](#difference-bw-function-statement-and-function-expression)
    - [Anonymous Function](#anonymous-function)
    - [Named function Expression](#named-function-expression)
    - [Parameters vs Arguments](#parameters-vs-arguments)
    - [First Class Functions/Citizens](#first-class-functionscitizens)
  - [Callback Functions](#callback-functions)
    - [Callback with event listeners](#callback-with-event-listeners)
  - [Asynchronous JavaScript and EVENT LOOP](#asynchronous-javascript-and-event-loop)
    - [WebAPIs](#webapis)
    - [Event Loops and Callback Queue](#event-loops-and-callback-queue)
    - [Behaviour of fetch (**Microtask Queue?**)](#behaviour-of-fetch-microtask-queue)
    - [setTimeout Issues](#settimeout-issues)
  - [Higher Order functions](#higher-order-functions)
  - [Call, Apply and Bind](#call-apply-and-bind)
  - [Event Bubbling and event capturing](#event-bubbling-and-event-capturing)
  - [Event Delegation](#event-delegation)
  - [CORS](#cors)
  - [async vs defer](#async-vs-defer)
  - [localStorage & sessionStorage](#localstorage--sessionstorage)
  - [Prototype and Prototypal Inheritance](#prototype-and-prototypal-inheritance)
  - [map, filter, reduce](#map-filter-reduce)
  - [Polyfills](#polyfills)
  - [map vs forEach](#map-vs-foreach)
  - [Currying](#currying)
    - [Real world scenario](#real-world-scenario)
  - [Caching/Memoization Function](#cachingmemoization-function)
  - [Promises](#promises)
    - [Using Promises](#using-promises)
    - [Chaining Promises](#chaining-promises)
  - [Fetch API](#fetch-api)
  - [Async & Await](#async--await)
  - [Deep & Shallow Copy](#deep--shallow-copy)
  - [Arrow functions, Pure functions](#arrow-functions-pure-functions)
  - [Rest vs Spread(...)](#rest-vs-spread)
  - [Array.flat implementation](#arrayflat-implementation)
    - [Custom function](#custom-function)
  - [Debouncing and Throttling](#debouncing-and-throttling)

## How JavaScript Works & Execution Context ?

-   Everything in JavaScript happens inside an execution context.

-   **Execution Context**
    -   <img src="JavaScript%20Interview%20Questions.assets/execution-context.jpg" alt="Execution Context" style="zoom: 67%;" />
    
    -   The first container is the **memory component** also known as the variable environment.
        -   It stores all the variables and function as key value pairs.
    -   The second container is the **code component** which is where all the code gets executed. Also known as the thread of execution.
    
-   JavaScript is a **synchronous & single threaded** language.

    -   **Synchronous :** One command at a time line by line.
    -   **Single Threaded :** In a specific synchronous order. (Can only run the next line when the current line has finished executing)

### How JavaScript code is executed ?

-   Whenever a JS program runs a **Global Execution Context (GEC)** gets created.

-   The GEC gets created in 2 phases

    -   **Memory Creation Phase :** JS will allocate memory to variables and functions.
    -   **Code Execution Phase**

- Take this as an example and its execution steps

    ```javascript
    var n = 2;
    function square(num) {
    	var ans = num * num;
    	return ans;
    }
    
    var square2 = square(n);
    var square4 = square(4);
    ```

    -   In the **memory creation phase** JS goes line by line and allocated memory space for variables and functions.
        -   First it goes to line 1 and allocates memory space for `n`. When allocating memory for `n` it stores the value as `undefined` and not the actual value. Then it goes to line 2 and stores the function along with its code in the variable environment. On line 7 & 8 since square2 and square4 are variables as well it allocates memory space for them as well with the value of undefined.
        -   The GEC will look like this after the MCP is over.
        -   <img src="JavaScript%20Interview%20Questions.assets/phase1.jpg" alt="Execution Context Phase 1" style="zoom: 67%;" />
        
    - In the **code execution phase** , it starts going through the whole code line by line. As it encounters var n = 2, it assigns 2 to 'n'. Until now, the value of 'n' was undefined. For function, there is nothing to execute. As these lines were already dealt with in memory creation phase. 

        -   Coming to line 6 i.e. `var square2 = square(n)`, here **functions are a bit different than any other language. A new execution context is created altogether.** Again in this new execution context, in memory creation phase, we allocate memory to num and ans the two variables. And undefined is placed in them. Now, in code execution phase of this execution context, first 2 is assigned to num. Then var ans = num * num will store 4 in ans. After that, return ans returns the control of program back to where this function was invoked from.

            ![Execution Context Phase 2](JavaScript%20Interview%20Questions.assets/phase2.jpg)

        -   When **return** keyword is encountered, It returns the control to the called line and also **the function execution context is deleted**. Same thing will be repeated for square4 and then after that is finished, the global execution context will be destroyed. So the **final diagram** before deletion would look something like:

            ![Execution Context Phase 2](JavaScript%20Interview%20Questions.assets/final_execution_context.jpg)

- JS manages the code execution context creation adn deletion with the help of the **call stack**

## Hoisting

```javascript
getName(); // Hello
console.log(x); // undefined
var x = 7;
function getName() {
 console.log("Hello");
}
```

In any other language accessing a variable or a function before declaration would be an error. But in JS, We know that in memory creation phase it assigns undefined and puts the content of function to function's memory. And in execution, it then executes whatever is asked. Here, as execution goes line by line and not after compiling, it could only print undefined and nothing else. This phenomenon, is not an error. However, if we remove var x = 7; then it gives error. Uncaught ReferenceError: x is not defined

- **Hoisting : **is a concept which enables us to extract values of variables and functions even before initialising/assigning value without getting error and this is happening due to the 1st phase (memory creation phase) of the Execution Context.

```javascript
getName(); // Uncaught TypeError: getName is not a function
console.log(getName);
var getName = function () {
    console.log("H");
}
```

- In this example getName() will have the value of undefined in the first phase and in the second phase since JS runs synchrounously it will still have the value of undefined.

## Functions and Variable Environment

```javascript
var x = 1;
a();
b(); // we are calling the functions before defining them. This will work properly, as seen in Hoisting.
console.log(x);

function a() {
  var x = 10; // local scope because of separate execution context
  console.log(x);
}

function b() {
  var x = 100;
  console.log(x);
}
```

Outputs : 

> 10
>
> 100
>
> 1

### Code flow in terms of execution context

- The Global Execution Context (GEC) is created (the big box with Memory and Code subparts). Also GEC is pushed into Call Stack

> Call Stack : GEC

- In first phase of GEC (memory phase), variable x:undefined and a and b have their entire function code as value initialized
- In second phase of GEC (execution phase), when the function is called, a new local Execution Context is created. After x = 1 assigned to GEC x, a() is called. So local EC for a is made inside code part of GEC.

> Call Stack: [GEC, a()]

- For local EC, a totally different x variable assigned undefined(x inside a()) in phase 1 , and in phase 2 it is assigned 10 and printed in console log. After printing, no more commands to run, so a() local EC is removed from both GEC and from Call stack

> Call Stack: GEC

- Cursor goes back to b() function call. Same steps repeat.

> Call Stack :[GEC, b()] -> GEC (after printing yet another totally different x value as 100 in console log)

- Finally GEC is deleted and also removed from call stack. Program ends.

<img src="JavaScript%20Interview%20Questions.assets/function.jpg" alt="Execution Context Phase 1" style="zoom:67%;" />

## Shortest JS Program, Window and the this keyword

- The shortest JS program is empty file. Because even then, JS engine does a lot of things. As always, even in this case, it creates the GEC which has memory space and the execution context.
- JS engine creates something known as '**window**'. It is an object, which is created in the global space. It contains lots of functions and variables. These functions and variables can be accessed from anywhere in the program. JS engine also creates a **this** keyword, which points to the **window object** at the global level. So, in summary, **along with GEC, a global object (window) and a this variable are created.**
- In different engines, the name of global object changes. Window in browsers, but in nodeJS it is called something else. **At global level, `this === window`**
- If we create any variable in the global scope, then the variables get attached to the global object.

```javascript
var x = 10;
console.log(x); // 10
console.log(this.x); // 10
console.log(window.x); // 10
```

## Undefined vs Not Defined

- In first phase (memory allocation) JS assigns each variable a placeholder called **undefined**.
- **undefined** is when memory is allocated for the variable, but no value is assigned yet.
- If an object/variable is not even declared/found in memory allocation phase, and tried to access it then it is **Not defined**
- Not Defined !== Undefined

> *When variable is declared but not assigned value, its current value is **undefined**. But when the variable itself is not declared but called in code, then it is **not defined**.*

```javascript
console.log(x); // undefined
var x = 25;
console.log(x); // 25
console.log(a); // Uncaught ReferenceError: a is not defined
```

## Scope, Scope Chain and Lexical Environment

Observe the below examples

```javascript
// CASE 1
function a() {
    console.log(b); // 10
    // Instead of printing undefined it prints 10, So somehow this a function could access the variable b outside the function scope. 
}
var b = 10;
a();

// CASE 2
function a() {
    c();
    function c() {
        console.log(b); // 10
    }
}
var b = 10;
a();

// CASE 3
function a() {
    c();
    function c() {
        var b = 100;
        console.log(b); // 100
    }
}
var b = 10;
a();

// CASE 4
function a() {
    var b = 10;
    c();
    function c() {
        console.log(b); // 10
    }
}
a();
console.log(b); // Error, Not Defined
```

- In **case 1**: function a is able to access variable b from Global scope.
- In **case 2**: 10 is printed. It means that within nested function too, the global scope variable can be accessed.
- In **case 3**: 100 is printed meaning local variable of the same name took precedence over a global variable.
- In **case 4**: A function can access a global variable, but the global execution context can't access any local variable.

```
To summarize the above points in terms of execution context:
call_stack = [GEC, a(), c()]
Now lets also assign the memory sections of each execution context in call_stack.
c() = [[lexical environment pointer pointing to a()]]
a() = [b:10, c:{}, [lexical environment pointer pointing to GEC]]
GEC =  [a:{},[lexical_environment pointer pointing to null]]
```

Refer this ->

<img src="JavaScript%20Interview%20Questions.assets/lexical.jpg" alt="Lexical Scope Explaination" style="zoom:67%;" />

- **Lexical Environment :** Local memory + lexical env of its parent. Hence, Lexical Environement is the local memory along with the lexical environment of its parent
    - Lexical : Heirachy
- **The process of going one by one to parent and checking for values is called scope chain or Lexcial environment chain.**

```javascript
function a() {
    function c() {
        // logic here
    }
    c(); // c is lexically inside a
} // a is lexically inside global execution
```

- **TLDR**; An inner function can access variables which are in outer functions even if inner function is nested deep. In any other case, a function can't access variables not in its scope.

## let & const, Temporal Dead Zone

- let and const declarations are hoisted. But its different from **var**

```javascript
console.log(a); // ReferenceError: Cannot access 'a' before initialization
console.log(b); // prints undefined as expected
let a = 10;
console.log(a); // 10
var b = 15;
console.log(window.a); // undefined
console.log(window.b); // 15
```

**It looks like let isn't hoisted, but it is**.

- Both a and b are actually initialized as *undefined* in hoisting stage. But var **b** is inside the storage space of GLOBAL, and **a** is in a separate memory object called script, where it can be accessed only after assigning some value to it first ie. one can access 'a' only if it is assigned. Thus, it throws error.

- **Temporal Dead Zone** : Time since when the let variable was hoisted until it is initialized some value.

    - So any line till before "let a = 10" is the TDZ for a
    - Since a is not accessible on global, its not accessible in *window/this* also. window.b or this.b -> 15; But window.a or this.a ->undefined, just like window.x->undefined (x isn't declared anywhere)

- **Reference Error** are thrown when variables are in temporal dead zone.

- **Syntax Error** doesn't even let us run single line of code.

    ```javascript
    let a = 10;
    let a = 100;  //this code is rejected upfront as SyntaxError. (duplicate declaration)
    ------------------
    let a = 10;
    var a = 100; // this code also rejected upfront as SyntaxError. (can't use same name in same scope)
    ------------------
    var a = 10;
    var a = 100;
    // can do this however
    ```

- **Let** is a stricter version of **var**. Now, **const** is even more stricter than **let**.

```javascript
let a;
a = 10;
console.log(a) // 10. Note declaration and assigning of a is in different lines.
------------------
const b;
b = 10;
console.log(b); // SyntaxError: Missing initializer in const declaration. (This type of declaration won't work with const. const b = 10 only will work)
------------------
const b = 100;
b = 1000; //this gives us TypeError: Assignment to constant variable. 
```

## Block Scope, Shadowing

- **Block?**

    - Block aka *compound statement* is used to group JS statements together into 1 group. We group them within {...}

        ```javascript
        {
            var a = 10;
            let b = 20;
            const c = 30;
            // Here let and const are hoisted in Block scope,
            // While, var is hoisted in Global scope.
        }
        ```

        - Ex.

        ```javascript
        {
            var a = 10;
            let b = 20;
            const c = 30;
        }
        console.log(a); // 10
        console.log(b); // Uncaught ReferenceError: b is not defined
        // b was inside the block scope and not accessible to the window
        ```

        - **Reason** ?    
            * In the BLOCK SCOPE; we get b and c inside it initialized as *undefined* as a part of hoisting (in a seperate memory space called **block**)
                * While, a is stored inside a GLOBAL scope. 
                * Thus we say, *let* and *const* are BLOCK SCOPED. They are stored in a separate mem space which is reserved for this block. Also, they can't be accessed outside this block. But var a can be accessed anywhere as it is in global scope. Thus, we can't access them outside the Block.

- What is **Shadowing?**

    ```javascript
    var a = 100;
    {
        var a = 10; // same name as global var
        let b = 20;
        const c = 30;
        console.log(a); // 10
        console.log(b); // 20
        console.log(c); // 30 
    }
    console.log(a); // 10, instead of the 100 we were expecting. So block "a" modified val of global "a" as well. In console, only b and c are in block space. a initially is in global space(a = 100), and when a = 10 line is run, a is not created in block space, but replaces 100 with 10 in global space itself. 
    ```

    - So, If one has same named variable outside the block, the variable inside the block *shadows* the outside variable. **This happens only for var**

    - Let's observe the behaviour in case of let and const and understand it's reason.

        ```javascript
        let b = 100;
        {
            var a = 10;
            let b = 20;
            const c = 30;
            console.log(b); // 20
        }
        console.log(b); // 100, Both b's are in separate spaces (one in Block(20) and one in Script(another arbitrary mem space)(100)). Same is also true for *const* declarations.
        ```

        ![Block Scope Explaination](JavaScript%20Interview%20Questions.assets/scope.jpg)

- What is **Illegal Shadowing?**

    ```javascript
    let a = 20;
    {
        var a = 20;
    }
    // Uncaught SyntaxError: Identifier 'a' has already been declared
    ```

    - We can shadow var with var since both of them will be in global scope and let with let since one let will be in script memory and the other one will be in block scope and also a var with a let.

## Closures

- **Function bundled along with it's lexical scope is closure.**

- JavaScript has a lexcial scope environment. If a function needs to access a variable, it first goes to its local memory. When it does not find it there, it goes to the memory of its lexical parent. See Below code, Over here function **y** along with its lexical scope i.e. (function x) would be called a closure. y forms a closure with x.

    ```javascript
    function x() {
        var a = 7;
        function y() {
            console.log(a);
        }
        return y;
    }
    var z = x();
    console.log(z);  // value of z is entire code of function y.
    ```

    - In above code, When y is returned, not only is the function returned but the entire closure (fun y + its lexical scope) is returned and put inside z. So when z is used somewhere else in program, it still remembers var a inside x()

<img src="JavaScript%20Interview%20Questions.assets/closure.jpg" alt="Closure Explaination" style="zoom:67%;" />

### Advantages of Closure

- Data Hiding 
- Memoization
- Currying

### Disadvantages of Closure

- Over consumption of memory and memory leak since the closed over variables are not garbage collected till the program expires.

### Closures + setTimeout()

```javascript
function x() {
    var i = 1;
    setTimeout(function() {
        console.log(i);
    }, 3000);
    console.log("Namaste Javascript");
}
x();
// Output:
// Namaste Javascript
// 1 // after waiting 3 seconds
```

- We expect JS to wait 3 sec, print 1 and then go down and print the string. But JS prints string immediately, waits 3 sec and then prints 1.
- The function inside setTimeout forms a closure (remembers reference to i). So wherever function goes it carries this ref along with it.
- setTimeout takes this callback function & attaches timer of 3000ms and stores it. Goes to next line without waiting and prints string.
- After 3000ms runs out, JS takes function, puts it into call stack and runs it.

**Q: Print 1 after 1 sec, 2 after 2 sec till 5 : Tricky interview question**

Simple approach

```javascript
function x() {
for(var i = 1; i<=5; i++){
    setTimeout(function() {
    	console.log(i);
    }, i*1000);
    }
    console.log("Namaste Javascript");
}
x();
```

```
OUTPUT : 
// Namaste Javascript
// 6
// 6
// 6
// 6
// 6
```

- This is not the expected output. Reason ? 

    - This happens because of closures. When setTimeout stores the function somewhere and attaches timer to it, the function remembers its reference to i, **not value of i**. All 5 copies of function point to same reference of i. JS stores these 5 functions, prints string and then comes back to the functions. By then the timer has run fully. And due to looping, the i value became 6. And when the callback fun runs the variable i = 6. So same 6 is printed in each log
    - To avoid this, we can use **let** instead of **var** as let has Block scope. For each iteration, the i is a new variable altogether(new copy of i). Everytime setTimeout is run, the inside function forms closure with new variable i.

- But if need to implement using var ?

    ```javascript
    function x() {
        for(var i = 1; i<=5; i++){
        	function close(i) {
            	setTimeout(function() {
            	console.log(i);
            }, i*1000);
            // put the setT function inside new function close()
        }
        close(i); // everytime you call close(i) it creates new copy of i.
        }
        console.log("Namaste Javascript");
    }
    x();
    ```

### Closure Output Questions

**Q1: Will the below code still forms a closure?**

```javascript
function outer() {
    function inner() {
        console.log(a);
    }
    var a = 10;
    return inner;
}
outer()(); // 10
```

Ans : Yes, because inner function forms a closure with its outer environment so sequence doesn't matter.

**Q2: Changing var to let, will it make any difference?**

```javascript
function outer() {
    let a = 10;
    function inner() {
        console.log(a);
    }
    return inner;
}
outer()(); // 10
```

**Ans**: It will still behave the same way.

**Q3: Will inner function have the access to outer function argument?**

```javascript
function outer(str) {
    let a = 10;
    function inner() {
        console.log(a, str);
    }
    return inner;
}
outer("Hello There")(); // 10 "Hello There"
```

**Ans**: Inner function will now form closure and will have access to both a and b.

**Q4: In below code, will inner form closure with **outest**?**

```javascript
function outest() {
    var c = 20;
    function outer(str) {
        let a = 10;
        function inner() {
            console.log(a, c, str);
        }
        return inner;
    }
    return outer;
}
outest()("Hello There")(); // 10 20 "Hello There"
```

**Ans**: Yes, inner will have access to all its outer environment.

**Q5: Output of below code and explaination?**

```javascript
function outest() {
    var c = 20;
    function outer(str) {
        let a = 10;
        function inner() {
            console.log(a, c, str);
        }
        return inner;
    }
    return outer;
}
let a = 100;
outest()("Hello There")(); // 10 20 "Hello There"
```

**Ans**: Still the same output, the inner function will have reference to inner a, so conflicting name won't matter here. If it wouldn't have find a inside outer function then it would have went more outer to find a and thus have printed 100. So, it try to resolve variable in scope chain and if a wouldn't have been found it would have given reference error.

**Q6: Discuss more on data hiding advantage**

```javascript
// without closures
var count = 0;
function increment(){
  count++;
}
// in the above code, anyone can access count and change it. 

------------------------------------------------------------------

// (with closures) -> put everything into a function
function counter() {
  var count = 0;
  function increment(){
    count++;
  }
}
console.log(count); // this will give referenceError as count can't be accessed. So now we are able to achieve hiding of data

------------------------------------------------------------------

//(increment with function using closure) true function
function counter() {
  var count = 0;
  return function increment(){
    count++;
    console.log(count);
  }
}
var counter1 = counter(); //counter function has closure with count var. 
counter1(); // increments counter

var counter2 = counter();
counter2(); // here counter2 is whole new copy of counter function and it wont impack the output of counter1

*************************

// Above code is not good and scalable for say, when you plan to implement decrement counter at a later stage. 
// To address this issue, we use *constructors*

// Adding decrement counter and refactoring code:
function Counter() {
//constructor function. Good coding would be to capitalize first letter of constructor function. 
  var count = 0;
  this.incrementCounter = function() { //anonymous function
    count++;
    console.log(count);
  }
   this.decrementCounter = function() {
    count--;
    console.log(count);
  }
}

var counter1 = new Counter();  // new keyword for constructor fun
counter1.incrementCounter();
counter1.incrementCounter();
counter1.decrementCounter();
// returns 1 2 1
```

## Types of functions and First Class Functions

### Function Statement/ Declaration

```javascript
function a() {
    console.log("Hello")
}
a();
```

### Function Expression

```javascript
var b = function() {
  console.log("Hello");
}
b();
```

### Difference b/w function statement and function expression

- The major difference between these two lies in **Hoisting**

```javascript
a(); // "Hello A"
b(); // TypeError
function a() {
  console.log("Hello A");
}
var b = function() {
  console.log("Hello B");
}
// Why? During mem creation phase a is created in memory and function assigned to a. But b is created like a variable (b:undefined) and until code reaches the function()  part, it is still undefined. So it cannot be called.
```

### Anonymous Function

- A function without a name

    ```javascript
    function () {
        
    } // this is going to throw Syntax Error - function Statement requires function name
    ```

### Named function Expression

```javascript
var b = function xyz() {
  console.log("b called");
}
b(); // "b called"
xyz(); // Throws ReferenceError:xyz is not defined.
// xyz function is not created in global scope. So it can't be called.
```

### Parameters vs Arguments

```javascript
var b = function(param1, param2) { // labels/identifiers are parameters
  console.log("b called");
}
b(arg1, arg2); // arguments - values passed inside function call
```

### First Class Functions/Citizens

In JS functions are treated as first class citizens which means they can be used as values like they can be passed to a function or assigned to a variable.

```javascript
var b = function(param1) {
  console.log(param1); // prints " f() {} "
}
b(function(){});

// Other way of doing the same thing:
var b = function(param1) {
  console.log(param1);
}
function xyz(){
}
b(xyz); // same thing as prev code

// we can return a function from a function:
var b = function(param1) {
  return function() {
  }  
}
console.log(b()); //we log the entire fun within b. 
```

## Callback Functions

- A function passed to a function to be called at a later time is known as a callback function.

- Callback functions introduce us to the asynchronous world of JS.

    ```javascript
    setTimeout(function () {
        console.log("Timer");
    }, 1000) // first argument is callback function and second is timer.
    ```

- JS is a synchronous and single threaded language. But due to callbacks, we can do async things in JS.

```javascript
setTimeout(function () {
	console.log("timer");
}, 5000);
function x(y) {
	console.log("x");
	y();
}
x(function y() {
	console.log("y");
});
// x y timer
```

- In the call stack, first x and y are present. After code execution, they go away and stack is empty. Then after 5 seconds (from beginning) anonymous suddenly appear up in stack ie. setTimeout
- All 3 functions are executed through call stack. If any operation blocks the call stack, its called blocking the main thread.
- Say if x() takes 30 sec to run, then JS has to wait for it to finish as it has only 1 call stack/1 main thread. Never block main thread.
- Always use **async** for functions that take time eg. setTimeout

```javascript
// Another Example of callback
function printStr(str, cb) {
    setTimeout(() => {
        console.log(str);
        cb();
    }, Math.floor(Math.random() * 100) + 1)
}
function printAll() {
    printStr("A", () => {
        printStr("B", () => {
            printStr("C", () => {})
        })
    })
}
printAll() // A B C // in order
```

### Callback with event listeners

- We will create a button in html and attach event to it.

```javascript
// index.html
<button id="clickMe">Click Me!</button>

// in index.js
document.getElementById("clickMe").addEventListener("click", function xyz(){ //when event click occurs, this callback function (xyz) is called into callstack
    console.log("Button clicked");
});
```

- Lets implement a increment counter button.

    - Using global variable (not good as anyone can change it)

        ```javascript
        let count = 0;
        document.getElementById("clickMe").addEventListener("click", function xyz(){ 
            console.log("Button clicked", ++count);
        });
        // can just change the value of count here only
        ```

    - Use closures for data abstraction

        ```javascript
        function attachEventList() {  //creating new function for closure
            let count = 0;
            document.getElementById("clickMe").addEventListener("click", function xyz(){ 
            console.log("Button clicked", ++count);  //now callback function forms closure with outer scope(count)
        });
        }
        attachEventList();
        ```

<img src="JavaScript%20Interview%20Questions.assets/event.jpg" alt="Event Listerner Demo" style="zoom: 80%;" />

- Event listeners take a lot of memory. So even when call stack is empty, EventListener won't free up memory allocated to count as it doesn't know when it may need count again. So we remove event listeners when we don't need them (garbage collected) onClick, onHover, onScroll all in a page can slow it down heavily.

## Asynchronous JavaScript and EVENT LOOP

- Browser has JS Engine which has Call Stack which has Global execution context, local execution context etc.

    - But browser has many other superpowers - Local storage space, Timer, place to enter URL, Bluetooth access, Geolocation access and so on.

    - Now JS needs some way to connect the callstack with all these superpowers. This is done using Web APIs.

         ![Event Loop 1 Demo](JavaScript%20Interview%20Questions.assets/eventloop1.jpg)

### WebAPIs

None of the below are part of Javascript! These are extra superpowers that browser has. Browser gives access to JS callstack to use these powers.

​                                             ![Event Loop 2 Demo](JavaScript%20Interview%20Questions.assets/eventloop2.jpg)

- setTimeout(), DOM APIs, fetch(), localstorage, console (yes, even console.log is not JS!!), location and so many more.

    - setTimeout() : Timer function
    - DOM APIs : eg.Document.xxxx ; Used to access HTML DOM tree. (Document Object Manipulation)
    - fetch() : Used to make connection with external servers eg. Netflix servers etc.

- We get all these inside call stack through global object ie. window

    - Use window keyword like : window.setTimeout(), window.localstorage, window.console.log() to log something inside console.
    - As window is global obj, and all the above functions are present in global object, we don't explicity write window but it is implied.

- Let's undertand the below code image and its explaination: 

    ![Event Loop 3 Demo](JavaScript%20Interview%20Questions.assets/eventloop3.jpg)

    - ```javascript
        console.log("start");
        setTimeout(function cb() {
            console.log("timer");
        }, 5000);
        console.log("end");
        // start end timer
        ```

    - First a GEC is created and put inside call stack.

    - console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.

    - setTimeout(function cb() { //this calls the setTimeout web api which gives access to timer feature. It stores the callback cb() and starts timer. console.log("Callback");}, 5000);

    - console.log("End"); // calls console api and logs in console window. After this GEC pops from call stack.

    - While all this is happening, the timer is constantly ticking. After it becomes 0, the callback cb() has to run.

    - Now we need this cb to go into call stack. Only then will it be executed. For this we need **event loop** and **Callback queue**

### Event Loops and Callback Queue

Q: How after 5 secs?

- cb() cannot simply directly go to callstack to be execeuted. It goes through the callback queue when timer expires.

- Event loop keep checking the callback queue, and see if it has any element to puts it into call stack. It is like a gate keeper.

- Once cb() is in callback queue, eventloop pushes it to callstack to run. Console API is used and log printed

    ![Event Loop 4 Demo](JavaScript%20Interview%20Questions.assets/eventloop4.jpg)

Q: Another example to understand Eventloop & Callback Queue.

See the below Image and code and try to understand the reason:

 ![Event Loop 5 Demo](JavaScript%20Interview%20Questions.assets/eventloop5.jpg) 

Explanation?

```javascript
console.log("Start"); 
document. getElementById("btn").addEventListener("click", function cb() { 
  // cb() registered inside webapi environment and event(click) attached to it. i.e. REGISTERING CALLBACK AND ATTACHING EVENT TO IT. 
  console.log("Callback");
});
console.log("End"); // calls console api and logs in console window. After this GEC get removed from call stack.
// In above code, even after console prints "Start" and "End" and pops GEC out, the eventListener stays in webapi env(with hope that user may click it some day) until explicitly removed, or the browser is closed.
```

- Eventloop has just one job to keep checking callback queue and if found something push it to call stack and delete from callback queue.

**Q: Need of callback queue?**

**Ans**: Suppose user clciks button x6 times. So 6 cb() are put inside callback queue. Event loop sees if call stack is empty/has space and whether callback queue is not empty(6 elements here). Elements of callback queue popped off, put in callstack, executed and then popped off from call stack.

### Behaviour of fetch (**Microtask Queue?**)

Let's observe the code below and try to understand

```javascript
console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console. 
setTimeout(function cbT() { 
  console.log("CB Timeout");
}, 5000);
fetch("https://api.netflix.com").then(function cbF() {
    console.log("CB Netflix");
}); // take 2 seconds to bring response
// millions lines of code
console.log("End"); 
```

**Code Explanation:**

* Same steps for everything before fetch() in above code.
* fetch registers cbF into webapi environment along with existing cbT.
* cbT is waiting for 5000ms to end so that it can be put inside callback queue. cbF is waiting for data to be returned from Netflix servers gonna take 2 seconds.
* After this millions of lines of code is running, by the time millions line of code will execute, 5 seconds has finished and now the timer has expired and response from Netflix server is ready.
* Data back from cbF ready to be executed gets stored into something called a Microtask Queue.
* Also after expiration of timer, cbT is ready to execute in Callback Queue.
* Microtask Queue is exactly same as Callback Queue, but it has higher priority. Functions in Microtask Queue are executed earlier than Callback Queue.
* In console, first Start and End are printed in console. First cbF goes in callstack and "CB Netflix" is printed. cbF popped from callstack. Next cbT is removed from callback Queue, put in Call Stack, "CB Timeout" is printed, and cbT removed from callstack.
* See below Image for more understanding

​                                   ![Event Loop 6 Demo](JavaScript%20Interview%20Questions.assets/eventloop6.jpg) 

### setTimeout Issues

- setTimeout with timer of 5 secs sometimes does not exactly guarantees that the callback function will execute exactly after 5s.

- Let's observe the below code and it's explaination

    ```javascript
    console.log("Start");
    setTimeout(function cb() {
    console.log("Callback");
    }, 5000);
    console.log("End");
    // Millions of lines of code to execute
    
    // o/p: Over here setTimeout exactly doesn't guarantee that the callback function will be called exactly after 5s. Maybe 6,7 or even 10! It all depends on callstack. Why?
    ```

    - Reason?
        - First GEC is created and pushed in callstack.
        - Start is printed in console
        - When setTimeout is seen, callback function is registered into webapi's env. And timer is attached to it and started. callback waits for its turn to be execeuted once timer expires. But JS waits for none. Goes to next line.
        - End is printed in console.
        - After "End", we have 1 million lines of code that takes 10 sec(say) to finish execution. So GEC won't pop out of stack. It runs all the code for 10 sec.
        - But in the background, the timer runs for 5s. While callstack runs the 1M line of code, this timer has already expired and callback fun has been pushed to Callback queue and waiting to pushed to callstack to get executed.
        - Event loop keeps checking if callstack is empty or not. But here GEC is still in stack so cb can't be popped from callback Queue and pushed to CallStack. **Though setTimeout is only for 5s, it waits for 10s until callstack is empty before it can execute** (When GEC popped after 10sec, callstack() is pushed into call stack and immediately executed (Whatever is pushed to callstack is executed instantly).
        - This is called as the **[Concurrency model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)** of JS. This is the logic behind setTimeout's trust issues.

- The First rule of JavaScript: Do not **block the main thread** (as JS is a single threaded(only 1 callstack) language).

- setTimeout guarantees that it will take at least the given timer to execute the code.

- What if **timeout = 0sec**?

    ```javascript
    console.log("Start");
    setTimeout(function cb() {
    console.log("Callback");
    }, 0);
    console.log("End");
    // Even though timer = 0s, the cb() has to go through the queue. Registers calback in webapi's env , moves to callback queue, and execute once callstack is empty.
    // O/p - Start End Callback
    // This method of putting timer = 0, can be used to defer a less imp function by a little so the more important function(here printing "End") can take place
    ```

## Higher Order functions

**Higher Order Function : ** A function which takes in a function as a argument or returns a function as their result.

```javascript
function x() {
    console.log("Hi)";
};
function y(x) {
    x();
};
y(); // Hi
// y is a higher order function
// x is a callback function
```

Example usecase for a HOF

Say we have to create a function which takes in a radius array and then returns the area of all the radius in the form of a array.

```javascript
const radiusArr = [1,2,3,4];

const calculateArea = (radius) => {
    const output = [];
    for(let i = 0;i < radius.length;i++){
        output.push(radius * radius * Math.PI);
    }
    
    return output;
}
```

Lets say now we have to calculate the circumference as well

```javascript
const calculateCircumference = (radius) => {
    const output = [];
    for(let i = 0;i < radius.length;i++){
        output.push(2 * radius * Math.PI);
    }
    
    return output;
}
```

Both of these functions are pretty similar and just the logic differs b/w them. If we had say also need to calculate the diameter that would also have the same structure of the function.

A better way to do this would be

```javascript
const area = radius => radius * radius * Math.PI;
const circumference = radius => 2 * Math.PI * radius;
const diameter = radius => 2 * radius;

const calculate = (radiusArr,operation) => {
    const output = [];
    for(let i = 0;i < radiusArr.length;i++){
        output.push(operation(radiusArr[i]));
    }
    
    return output;
}
```

## Call, Apply and Bind

- **call :** what call does is calls a function with the given this context.
- **apply : ** apply is the same as call but it takes external arguments in an array.
- **bind : ** bind does not call the function but returns a function which can be called later

```javascript
const name = {
    firstName : "Chirag",
    lastName : "Jain"
}

const printFullName = function(first,second) {
    console.log(`${this.firstName} ${this.lastName}`);
    console.log(first,second);
}

// CALL
printFullName.call(name,"hello","there"); // Chirag Jain Hello There

// APPLY
printFullName.apply(name,["hello","there"]); // Chirag Jain Hello There

// BIND
let fullName = printFullName.bind(name);
fullName(); // can be called at any later time
```

## Event Bubbling and event capturing

```html
<div id="grandparent">
    grandparent
    <div id="parent">
        parent
        <div id="child">
            child
        </div>
    </div>
</div>
```

```css
#grandparent {
	width: 400px;
	height: 300px;
	border: 1px solid black;
}

#parent {
	width: 300px;
	height: 200px;
	border: 1px solid black;
	margin: 40px;
}

#child {
	width: 200px;
	height: 100px;
	border: 1px solid black;
	margin: 40px;
}
```

```javascript
const grandparent = document.querySelector("#grandparent");
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

grandparent.addEventListener("click", function () {
	console.log("grandparent");
});

parent.addEventListener("click", function () {
	console.log("parent");
});

child.addEventListener("click", function () {
	console.log("child");
});
```

<img src="JavaScript%20Interview%20Questions.assets/image-20220628191932242.png" alt="image-20220628191932242" style="zoom:67%;" />

Say we have such a structure now if we click on child then the event is going to bubble up to the parent element as well which is the grandparent. On clicking the child div we will get the output of

> child
>
> parent
>
> grandparent

- In bubbling we go from down to up ( default behaviour )
- In trickling we go from up to down ( will happen if we pass true to the second argument of addEventListener)
- First trickling happens and then bubbling
- If we pass true as the second argument then ievent will trickle/ capture and then the output on clicking child div will be 

> grandparent
>
> parent
>
> child

- Ex.

    ```javascript
    // if this was the case
    grandparent.addEventListener(
    	"click",
    	function () {
    		console.log("grandparent");
    	},
    	true
    );
    
    parent.addEventListener(
    	"click",
    	function () {
    		console.log("parent");
    	},
    	false
    );
    
    child.addEventListener(
    	"click",
    	function () {
    		console.log("child");
    	},
    	true
    );
    ```

    Output will be

    > grandparent (since trickle is true)
    >
    > child ( first trickle hota h and parent bubble ke liey set h)
    >
    > parent ( child tak trickle hone ke baad fir bubbling phase chalu hoga)

## Event Delegation

*If we have a list of items which all needs to be clickable but we are not going to add event Listeners to all the items in the list since onClick event handlers are heavy* 

***We provide event listener to the parent and then acess the child elements using that***

ex.

```html
<div id="products">
	<li id="shoes"></li>
	<li id="table"></li>
	<li id="chair"></li>
</div>
```

```javascript
// only the parent element has the event listener
document.querySelector("#products").addEventListener('click',(e) => {
	console.log(e);
	if(e.target.tagName === 'LI'){
		window.location.href += "#" + event.target.id;
	}
})
```

## CORS

**CORS : Cross origin request sharing**

- CORS is a mechanism which uses additional http headers to tell the browser wheter a specific web app can share the resourse with another web app when both the web apps should have diff origin
- How it Works?
    - suppose there are 2 origins a and b what happens is first a preflight/options request is made which contains some http headers which will verify if the request can be made or not
    - if can be made b will send some additional headers after which the actual call is made
    - Additional Headers :
        - access-control-allow-origin
        - access-control-allow-methods etc.

## async vs defer

Bina async aur defer ke jaise hi encounter karti h vaise hi fetch karegi aur fir run kardeti h aur uske baad html chalta h

With async attribute it is fetched aynchrounously in parallel with the html parsing and as soon as it gets it it runs and then continues with the html parsing

With defer attribute script it is fetched aynchrounously in parallel with the html parsing but is only executed when the html is full parsed

## localStorage & sessionStorage

- Stored in key value pairs of string inside the browser
    - **Session Storage :  Stored only for that particular session until we close the browser window**
    - **Local Storage -> Does not have an expiry**

```javascript
localStorage.setItem(key,value)
localStorage.getItem(key)
localStorage.removeItem(key)
localStorage.clear()

-------------------------------
    
Generally we store objects in value
	To do that use (key,JSON.stringify(value))
	and to get use JSON.parse
```

## Prototype and Prototypal Inheritance

- **A prototype is a blueprint for an object**

- Basically prototypal inheritance se baaki languages ki tarah inheritance use kar sakte h

- In JavaScript, objects have a special hidden property [[Prototype]] (as named in the specification), that is either null or references another object. That object is called “a prototype”:

- When we read a property from object, and it’s missing, JavaScript automatically takes it from the prototype. In programming, this is called “prototypal inheritance”.

- Ex.

    ```javascript
    let animal = {
    	eats : true
    }
    
    let rabbit = {
    	jumps : true
    }
    
    rabbit.__proto__ = animal; // rabbit ka prototype is animal now
    clg(rabbit.eats); // true eats is not in rabbit so it will follow the prototypal chain and will go to animal(its prototype) and finds eats there
    
    
    --------------------------
    let animal = {
      eats: true,
      walk() {
        alert("Animal walk");
      }
    };
    
    let rabbit = {
      jumps: true,
      __proto__: animal // can also do thiss
    };
    
    // walk is taken from the prototype
    rabbit.walk(); // Animal walk
    ```

## map, filter, reduce

- **map : ** creates a new array from the existing one by applying a function on all the elements of the original array.
- **filter : **takes each element in an array and applies a conditional statement if it passes then goes into the output array or not
- **reduce : ** reduces the array into just one value

ex.

```javascript
const nums = [1,2,3,4];
const multiplyThree = nums.map((num,idx,arr) =>{
	// arr is the original array
	return num * 3;
}) // 3,6,9,12

------------
const moreThanTwo = nums.filter((num,idx,arr) => {
	return num > 2;
})
// [3,4]

-----------
const sum = nums.reduce((accumulator,currentValue,i,arr) => {
	// accumulator is the previousValue/ value till now
	return accumulator + currentValue;
},0) // the initial value
// if no initital value then the first element of the array
// 10
```

## Polyfills

```javascript
// map
Array.prototype.myMap = function(cb){
	let temp = [];
	for(let i = 0; i < this.length;i++){
		temp.push(cb(this[i],i,this));
	}

	return temp;
}

// filter 
Array.prototype.myFilter = function(cb){
	let temp = [];
	for(let i = 0; i < this.length;i++){
		if(cb(this[i],i,this)) temp.push(this[i]);
	}

	return temp;
}

// reduce
Array.prototype.myReduce = function(cb,initialValue){
	var accumulator = initialValue;

	for(let i = 0;i < this.length;i++){
		// if no initialValue is given then it should be the first value
		accumulator = accumulator ? cb(accumulator,this[i],i,this) : this[i];
	}

	return accumulator;
}

```

## map vs forEach

map returns a new array by calling the callback function on the elements

forEach just loops over the array for each element

## Currying

Currying is the ability through which we can convert a function of n arguments to a function that returns n function.

ex. `add(3,4) -> add(3)(4)`

ex.

```javascript
function f(a){
	return function (b) {
		return `${a} ${b}`
	}
}

clg(f(3)(4)); // 3 4
```

**Q : sum(2)(6)(1)**

```javascript
function sum(a){
	return function (b) {
		return function (c){
			return a + b + c;
		}
	}
}
```

**Q -> infinite currying sum(1)(2)(3)...(n)()**

```javascript
function sum(a){
	return function (b){
		if(b) return a + b; // if b has a value
		return a;
	};
}
```

### Real world scenario

```javascript
function updateElementText(id) {
	return function(content){
		document.querySelector("#" + id).textContent = content;
	}
}

const updateHeader = updateElementText("heading");
// we can now call updateHeader again and again and it will update the content of the heading
// we can now call updateHeader(content) or updateElementText(id)(content)
```

## Caching/Memoization Function

Say we have a function that takes a lot of time to run and is basically blocking the main thread. What we can do is to write a caching function that is going to store the value of already run arguments and only run the function if we dont have the value.

```javascript
const clumsyProduct = (num1,num2) => {
	for(let i = 1;i <= 1000000000;i++){}

	return num1 * num2;
}

console.time("First Call");
console.log(clumsySquare(567,876));
console.timeEnd("End call");

console.time("First Call");
console.log(clumsySquare(567,876));
console.timeEnd("End call");

// Both these calls are going to take the same time even if we have the same arguments

function myMemoize(fn,context){
	const cache = {};
	return function (...args){
		var argsCache = JSON.stringify(args);
		// check if already present in our cache storage
		// if not present then calculate the answer
		if(!cache[argsCache]){
			res[argsCache] = fn.call(context || this,...args);
		}
		// then return the answer if already there
		return res[argsCache];
	}
}
```

## Promises

Promises are used to handle asynchronous tasks in JS. Before promises callbacks were used to do async things but using a lot of callbacks lead to unmanageable code and would lead to **callback hell( Callback within a callback ...)** 

```javascript
const getSomething = (resource, callback) => {
	const request = new XMLHttpRequest();
	request.addEventListener("readystatechange", () => {
		if (request.readyState === 4 && request.status === 200) {
			callback(undefined, request.responseText);
		} else if (request.readyState === 4) {
			callback("could not get the data", undefined);
		}
	});
	request.open("GET", resource);
	request.send();
};

// WORKS -> WILL ONLY CALL THE INNER ONE AFTER OUTER IS COMPLETED
// EVEN THOUGH THIS WORKS THIS CAN GET MESSY REALLY FAST and wil form this tree like structure
getSomething("someEndpoint", (err, data) => {
	console.log(data);
	getSomething("someEndpoint", (err, data) => {
		console.log(data);
		getSomething("someEndpoint", (err, data) => {
			console.log(data);
		});
	});
});
```

### Using Promises

```javascript
const doSomething = () => {
	// resolve and reject are built into Promise API and will recieve as parameters
	return new Promise((resolve, reject) => {
		// fetch some data
		resolve(data);
		// reject(error);
	});
};

// then will take another callback method and will fire it when promise gets resolved or rejected
// first one will be fired when resolved
// second one will be fired when rejected
doSomething().then(
	(data) => {
		console.log(data);
	},
	(error) => {
		console.log(error);
	}
);

----------------------------------------------------------------------------
// BETTER WAY
doSomething()
	.then((data) => {
		console.log(data);
	})
	.catch((err) => {
		console.log(err);
	});
```

### Chaining Promises

```javascript
getSomething("promise1Endpoint")
	.then((data) => {
		console.log("promise 1", data);
		// this returns a promise so we can chain another then to this function
		return getSomething("promise2Endpoint");
	})
	.then((data) => {
		console.log("promise 2", data);
		return getSomething("promise3Endpoint");
	})
	.then((data) => {
		console.log("promise 2", data);
	})
	.catch((err) => {
		// single catch wiil work for all of them
		console.log("error", error);
	});
```

## Fetch API

-   Easier and newer way to make requests as compared to `XMLHttpRequest`.
-   Uses the promise API
-   However only rejects when there is a network error
-   Use status codes to check if succes or not

```javascript
fetch("someEndpoint")
	.then((response) => {
		console.log("resolved", response);
		// will return a promise and parse the json itself
		return response.json();
	})
	.then((data) => {
		console.log(data);
	})
	.catch((error) => {
		console.log("rejected", error);
	});
```

## Async & Await

-   Async function will always return a promise
-   What await does is to stop JS ( function is already async so will not stop the normal calls ) and return value only when the promise gets resolved
-   **Non blocking code**
-   Syntatical sugar for promises

```javascript
const getSomething = async () => {
	const response = await fetch("someEndpoint");
	// response.json() also returns a promise
	const data = await response.json();

	return data;
};

// BUT WILL STILL NEED TO USE A SINGLE .then() when the function gets called since async functions return promises

getSomething().then((data) => {
	console.log("resolved", data);
});
```

## Deep & Shallow Copy

**Shallow copy is when only the values gets copied but the reference remains the same.**

```javascript
const a = {
    name : "Hello",
    age : 34
}

const b = a; // this will store the reference
b.name = "There";
clg(a.name); // There
```

**Deep copy when it is a completely new copy with no references to the original object**

- Ways to create deep copy

    ```javascript
    // Using JSON methods
    // doesnot copy functions
    const objCopy = JSON.parse(JSON.stringify(obj));
    
    // using spread
    // doesnot clone nested objects
    const objCopy = {...obj};
    ```

## Arrow functions, Pure functions

- Arrow function difference in syntax return keyword no arguments keyword in an object the arrow function refers to this as the window object

```javascript
let user = {
	name : "hello",
	rc1 : () => {
		clg(this.name); // undefined
	}
	rc2 () {
		clg(this.name); // hello
	}
}

user.rc1(); // undefined
user.rc2(); // hello
```

- Pure function : for same input always get the same output.

## Rest vs Spread(...)

- **Rest** : Enclose the values passed to a function in an array

    ```javascript
    function hello(a,b,...args){
        // args will be an array which will contain all the extra arguments passed
    }
    let a = "there"
    let b = "hello"
    hello(a,b,"val1","val2","val3");
    ```

- **Spread** : Expands the value into individual arguments.

    ```javascript
    function myBio(firstName, lastName, company) { 
      return `${firstName} ${lastName} runs ${company}`;
    }
    
    myBio(...["Oluwatobi", "Sofela", "CodeSweetly"]);
    ```

## Array.flat implementation

```javascript
// inbuilt method
const arr = [
	[1,2],
	[3,4],
	[5,6],
	[
		[7,8],
		9,
		10
	]
]
arr.flat(2); // 2 is the depth 
```

### Custom function

```javascript
function customFlat(arr,depth = 1){
	let result = [];
	arr.forEach((ar) => {
		if(Array.isArray(ar) && depth > 0){
			result.push(...customFlat(ar,depth - 1))
		}
		else result.push(ar);
	})

	return result;
}
```

## Debouncing and Throttling

[Refer This](*https://medium.com/nerd-for-tech/debouncing-throttling-in-javascript-d36ace200cea#:~:text=Debouncing%20is%20a%20technique%20where,irrespective%20of%20continuous%20user%20actions*)

