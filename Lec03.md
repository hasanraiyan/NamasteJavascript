
# Lecture 3: Hoisting in JavaScript

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [The Hoisting Phenomenon](#-the-hoisting-phenomenon)
2.  [Code Examples](#-code-examples)
    *   [Example 1: Demonstrating Hoisting](#example-1-demonstrating-hoisting)
    *   [Example 2: Hoisting with Arrow Functions](#example-2-hoisting-with-arrow-functions)
3.  [How Hoisting Works: The Execution Context](#-how-hoisting-works-the-execution-context)
    *   [Phase 1: Memory Creation & Hoisting](#phase-1-memory-creation--hoisting)
    *   [Phase 2: Code Execution](#phase-2-code-execution)
4.  [Visualizing Hoisting with Chrome DevTools](#-visualizing-hoisting-with-chrome-devtools)
5.  [Undefined vs. Not Defined](#-undefined-vs-not-defined)
6.  [Hoisting with Function Expressions & Arrow Functions](#-hoisting-with-function-expressions--arrow-functions)
7.  [Summary & Key Takeaways](#-summary--key-takeaways)

---

## 🪄 The Hoisting Phenomenon

**Hoisting** is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

> 💡 **Conceptual Definition:**
> Hoisting is a phenomenon in JavaScript by which you can access variables and functions even before you have initialized them without getting an error.

This behavior is a direct result of how JavaScript's **Execution Context** is created in two phases.

---

## 💻 Code Examples

### Example 1: Demonstrating Hoisting

Consider the following code snippet where we call a function and log a variable *before* their declarations.

```js
// Invoke the function and log the variable BEFORE declaration
getName();
console.log(x);

var x = 7;

function getName() {
  console.log("Namaste JavaScript");
}
```

**Expected Output:**

```
Namaste JavaScript
undefined
```
*   In most other programming languages, this would throw an error.
*   In JavaScript, `getName()` executes correctly, but `console.log(x)` prints `undefined`. This is hoisting in action.

### Example 2: Hoisting with Arrow Functions

Now, let's see what happens if `getName` is an arrow function.

```js
getName(); // This will now throw a TypeError
console.log(x);

var x = 7;

var getName = () => {
  console.log("Namaste JavaScript");
}
```
In this case, the code will throw a `TypeError`. We will explore why later in these notes.

---

## 🤔 How Hoisting Works: The Execution Context

Hoisting is not a magical process where code is physically moved. It's a direct consequence of the two-phase execution process.

### Phase 1: Memory Creation & Hoisting

Before any code is executed, the JavaScript engine scans the code and allocates memory in the **Global Execution Context (GEC)**.

*   **For `var` variables**:
    *   The engine allocates memory for the variable (e.g., `x`).
    *   It then stores a placeholder value of `undefined` in it.
    *   `var x = 7;` -> `x: undefined`

*   **For `function` declarations**:
    *   The engine allocates memory for the function (e.g., `getName`).
    *   It then stores the **entire function code** in the memory block.
    *   `function getName() {...}` -> `getName: <function code>`

> ✅ **This is the core of hoisting!** Because the function's code and the variable's memory space exist *before* the code execution starts, we can access them from the first line.

### Phase 2: Code Execution

After memory allocation, the code is executed line by line.

1.  `getName();`: The engine looks up `getName` in memory, finds the function code, creates a new function execution context, and executes it. This prints "Namaste JavaScript".
2.  `console.log(x);`: The engine looks up `x` in memory, finds its current value which is `undefined`, and prints it.
3.  `var x = 7;`: The engine now assigns the value `7` to `x`. If we were to `console.log(x)` after this line, it would print `7`.

---

## 🔬 Visualizing Hoisting with Chrome DevTools

We can observe this behavior by placing a breakpoint on the very first line of our code.

*   In the **Sources** tab, add a breakpoint at line 1.
*   Refresh the page. The code will pause before executing anything.
*   In the **Scope** panel, you can inspect the `Global` memory space. You will see:
    *   `getName: f getName()` (the function is already stored)
    *   `x: undefined` (the variable is initialized with `undefined`)
*   The **Call Stack** will show the `(anonymous)` global execution context.

This proves that memory is allocated before code execution begins.

---

## 🚫 Undefined vs. Not Defined

These two concepts are fundamentally different and are a common source of confusion.

*   **`undefined`**: This is a special value that means a variable has been declared and allocated memory, but no value has been assigned to it yet.
    ```js
    console.log(a); // Output: undefined
    var a = 10;
    ```
*   **`not defined`**: This means the variable has **not been declared** or allocated memory in the current scope. Accessing it will throw a `ReferenceError`.
    ```js
    console.log(b); // Throws ReferenceError: b is not defined
    ```

---

## ⚠️ Hoisting with Function Expressions & Arrow Functions

Function Expressions and Arrow Functions behave differently from Function Declarations during hoisting.

```js
// console.log(getName); // This would log 'undefined'
getName(); // This will throw TypeError

var getName = function () {
  console.log("Namaste JavaScript");
}
```
**Why does this cause a `TypeError`?**

1.  In the **Memory Creation Phase**, `getName` is treated as a regular variable because of `var getName`.
2.  Therefore, memory is allocated for `getName`, and it is initialized with `undefined`.
3.  In the **Code Execution Phase**, when the engine tries to execute `getName()`, it finds that `getName` holds the value `undefined`, not a function.
4.  Trying to invoke `undefined` as a function results in `Uncaught TypeError: getName is not a function`.

> **💡 Note:** The same behavior applies to Arrow Functions assigned to a `var`. They are treated as variables during the hoisting process.

---

## ✨ Summary & Key Takeaways

1.  **Hoisting** is the behavior where JavaScript seems to lift variable and function declarations to the top of their scope.
2.  It's a direct result of the **Memory Creation Phase** of the Execution Context.
3.  Variables declared with `var` are hoisted but are initialized with `undefined`.
4.  Function declarations are hoisted with their **full function body**, allowing them to be called before their definition in the code.
5.  Function expressions and arrow functions behave like variables: they are hoisted, but their value is `undefined` until the assignment line is executed, which leads to a `TypeError` if invoked early.
6.  `undefined` means a variable exists but has no value. `not defined` means the variable does not exist in memory at all, leading to a `ReferenceError`.
