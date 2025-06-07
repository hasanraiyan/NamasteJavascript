
# Lecture 8: let & const in JS, Temporal Dead Zone (TDZ)

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Are `let` & `const` Hoisted?](#-are-let--const-hoisted)
2.  [The Temporal Dead Zone (TDZ)](#-the-temporal-dead-zone-tdz)
    *   [Definition](#definition)
    *   [How it Works](#how-it-works)
3.  [Hoisting: `var` vs. `let` & `const`](#-hoisting-var-vs-let--const)
    *   [Global Scope vs. Script Scope](#global-scope-vs-script-scope)
4.  [JavaScript Error Types](#-javascript-error-types)
    *   [`ReferenceError`](#referenceerror)
    *   [`SyntaxError`](#syntaxerror)
    *   [`TypeError`](#typeerror)
5.  [Strictness of `let` and `const`](#-strictness-of-let-and-const)
6.  [Best Practices for Variable Declarations](#-best-practices-for-variable-declarations)
7.  [Summary](#-summary)

---

## 🤔 Are `let` & `const` Hoisted?

This is a very common interview question.

> **The Definitive Answer: YES, `let` and `const` declarations are hoisted.**

However, they are hoisted differently than `var`. While `var` variables are initialized with `undefined` upon hoisting, `let` and `const` variables are hoisted but remain **uninitialized**. They are in a state known as the **Temporal Dead Zone**.

---

## ⏳ The Temporal Dead Zone (TDZ)

### Definition
The **Temporal Dead Zone (TDZ)** is the period of time from when a `let` or `const` variable enters its scope until the line where it is actually declared and initialized.

> 💡 **What does this mean?**
> It's the phase where the variable is in scope, but you cannot access it. The variable is "dead" for that temporal period.

### How it Works
1.  When a `let` or `const` variable is hoisted, memory is allocated, but it is not attached to the global object and is not initialized.
2.  This variable is in a state where it cannot be accessed.
3.  The Temporal Dead Zone ends when the line `let a = 10;` is executed. After this point, the variable is initialized and can be accessed.

#### Code Example:
```js
console.log(a); // Uncaught ReferenceError: Cannot access 'a' before initialization
console.log(b); // Output: undefined

let a = 10;
var b = 20;
```
*   `a` is in the TDZ until line 4 is executed. Accessing it on line 1 throws a `ReferenceError`.
*   `b` is hoisted and initialized with `undefined`. Accessing it on line 2 prints `undefined`.

---

## 🧐 Hoisting: `var` vs. `let` & `const`

Let's use the browser's debugger to see the difference.

```js
// index.js
let a = 10;
var b = 100;
```
If we place a breakpoint on the very first line of the code *before* execution:
*   In the **Scope** panel, you will see two sections:
    *   **`Script`**: Contains the variable `a` with the value `<value unavailable>` or `undefined`. This shows `a` is in a separate memory space and is currently in the TDZ.
    *   **`Global`**: Contains the variable `b` with the value `undefined`. This shows `b` is attached to the global object (`window`).

> **Key Differences in Hoisting:**
> *   `var` declared variables are hoisted and attached to the **global object** (`window`) with an initial value of `undefined`.
> *   `let` and `const` declared variables are hoisted but stored in a **separate memory space** (often called the `Script` scope). They remain uninitialized until their declaration is executed. They are in the TDZ.

### Global Scope vs. Script Scope
*   `let` and `const` are block-scoped and are not attached to the `window` object. You cannot access them using `window.a`.
    ```js
    let a = 10;
    console.log(window.a); // Output: undefined
    ```
*   `var` is function-scoped (or globally-scoped if declared outside a function) and *is* attached to the `window` object.
    ```js
    var b = 100;
    console.log(window.b); // Output: 100
    console.log(this.b);   // Output: 100
    ```

---

## 🚨 JavaScript Error Types

Understanding the different types of errors is crucial for debugging.

### `ReferenceError`
Thrown when trying to access a variable that is not declared or is in the Temporal Dead Zone.
*   **Example 1 (TDZ):** `console.log(a); let a = 10;` -> `ReferenceError: Cannot access 'a' before initialization`.
*   **Example 2 (Not Defined):** `console.log(x);` -> `ReferenceError: x is not defined`.

### `SyntaxError`
Thrown when the code violates the rules of the language. This error occurs during the parsing phase, before any code is executed.
*   **Example 1 (Re-declaration):** `let a = 10; let a = 20;` -> `SyntaxError: Identifier 'a' has already been declared`.
*   **Example 2 (Missing Initializer):** `const b;` -> `SyntaxError: Missing initializer in const declaration`.

### `TypeError`
Thrown when an operation is performed on a value of an incorrect type.
*   **Example (Re-assignment to const):** `const b = 100; b = 200;` -> `TypeError: Assignment to constant variable.`.

---

## ⛓️ Strictness of `let` and `const`

`let` and `const` are stricter than `var`, which helps in writing more robust and less error-prone code.

1.  **Re-declaration**: You cannot re-declare a variable with the same name in the same scope using `let` or `const`.
    ```js
    let a = 10;
    // let a = 20;   // SyntaxError: 'a' has already been declared
    // var a = 20;   // SyntaxError: 'a' has already been declared
    ```
2.  **`const` Declarations**:
    *   They must be initialized at the time of declaration.
    *   The value cannot be re-assigned.

---

## ✅ Best Practices for Variable Declarations

1.  **Minimize the TDZ**: Always declare and initialize your variables at the top of their scope. This shrinks the TDZ window to almost zero, reducing the chance of `ReferenceError`.
2.  **Prefer `const`**: Use `const` by default for all declarations. This ensures immutability where possible.
3.  **Use `let` only when necessary**: If you know a variable's value needs to change, use `let`.
4.  **Avoid `var`**: Try to stop using `var` to prevent unexpected issues with hoisting and scope.

---

## ✨ Summary

1.  `let` and `const` are **hoisted**, but they enter a **Temporal Dead Zone (TDZ)** until they are initialized.
2.  Accessing a variable in the TDZ throws a `ReferenceError`.
3.  `let` and `const` are **block-scoped** and are stored in a separate memory space, not on the `window` object.
4.  They cannot be re-declared in the same scope, which prevents common bugs and throws a `SyntaxError`.
5.  `const` is even stricter: it must be initialized on declaration (`SyntaxError`) and cannot be re-assigned (`TypeError`).
6.  Following best practices (top-of-scope declarations, preferring `const`) leads to cleaner, safer code.
