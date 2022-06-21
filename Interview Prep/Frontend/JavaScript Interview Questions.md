<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [JavaScript Interview Questions](#javascript-interview-questions)
  - [How JavaScript Works & Execution Context ?](#how-javascript-works--execution-context-)
    - [How JavaScript code is executed ?](#how-javascript-code-is-executed-)
- [New heading](#new-heading)
  - [Heading 2](#heading-2)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# JavaScript Interview Questions

## How JavaScript Works & Execution Context ?

-   Everything in javaScript happens inside an execution context.

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

-   Take this as an example and its execution steps

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
        -   <img src="JavaScript%20Interview%20Questions.assets/phase1.jpg" alt="Execution Context Phase 1" style="zoom:50%;" />
    -   In the code execution phase , it starts going through the whole code line by line. As it encounters var n = 2, it assigns 2 to 'n'. Until now, the value of 'n' was undefined. For function, there is nothing to execute. As these lines were already dealt with in memory creation phase.



# New heading

## Heading 2

