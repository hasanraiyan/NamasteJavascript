
# Lecture 10: Closures in JS

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [What is a Closure?](#-what-is-a-closure)
    *   [Simple Explanation](#simple-explanation)
    *   [MDN Definition](#mdn-definition)
2.  [Understanding Closures with Code](#-understanding-closures-with-code)
    *   [Example 1: Basic Lexical Scoping](#example-1-basic-lexical-scoping)
    *   [Example 2: Returning a Function (The Classic Closure)](#example-2-returning-a-function-the-classic-closure)
    *   [Example 3: Deeply Nested Closures](#example-3-deeply-nested-closures)
3.  [How Closures Work Under the Hood](#-how-closures-work-under-the-hood)
4.  [Key Interview Questions & Gotchas](#-key-interview-questions--gotchas)
    *   [Does a returned function still have access to its parent's scope?](#does-a-returned-function-still-have-access-to-its-parents-scope)
    *   [What if the parent variable's value changes?](#what-if-the-parent-variables-value-changes)
5.  [Use Cases of Closures](#-use-cases-of-closures)
6.  [Summary](#-summary)

---

## 🤔 What is a Closure?

Closures are one of the most important and often misunderstood concepts in JavaScript.

### Simple Explanation

> A **closure** is a function bundled together with its **lexical environment**. In other words, a closure gives you access to an outer function's scope from an inner function.

Even when a function is executed outside its original lexical scope, it still remembers the variables and scope it was born in. This ability to "remember" is the essence of a closure.

### MDN Definition

> "A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives you access to an outer function's scope from an inner function."

---

## 💻 Understanding Closures with Code

### Example 1: Basic Lexical Scoping

This example demonstrates lexical scoping, which is the foundation for closures.

```js
function x() {
    var a = 7;
    function y() {
        console.log(a); // 'a' is accessed from its parent's lexical scope
    }
    y();
}
x(); // Output: 7
```
*   Function `y` has access to the variables of its lexical parent, function `x`. This is a fundamental concept of scope we've already covered.

### Example 2: Returning a Function (The Classic Closure)

This is where the magic of closures becomes apparent. We return the inner function instead of just calling it.

```js
function x() {
    var a = 7;
    function y() {
        console.log(a);
    }
    return y;
}

var z = x();
console.log(z);
// Output: f y() { console.log(a); }

// Later in the code...
z(); // Output: 7
```

**Execution Analysis:**
1.  `x()` is called. A new execution context for `x` is created. `var a = 7` is defined.
2.  Function `y` is **returned** from `x`.
3.  The execution context for `x` is popped off the call stack and destroyed. `a` should technically be gone.
4.  The returned function (`y`) is assigned to the variable `z`.
5.  When we later call `z()`, it still prints `7`.

> **Why does this work?**
> When a function is returned from another function, it doesn't just return the function code. It returns the **function along with its lexical scope**. This "bundle" is the closure. So, `z` holds a closure which contains the function `y` and a reference to the memory of `a`.

### Example 3: Deeply Nested Closures

Closures work across multiple levels of nesting.
```js
function z() {
    var b = 900;
    function x() {
        var a = 7;
        function y() {
            console.log(a, b); // Accesses both parent and grandparent scopes
        }
        y();
    }
    x();
}
z(); // Output: 7 900
```
Here, function `y` forms a closure that captures the scope of `x` (its parent) and `z` (its grandparent).

---

## ⚙️ How Closures Work Under the Hood

When a function is returned, the JavaScript engine ensures that any variables from its lexical scope that it needs are not garbage-collected. They are preserved.

*   In **Example 2**, when `y` was returned, the engine knew that `y` uses the variable `a`.
*   So, it didn't destroy `a` when `x`'s execution context was popped. It preserved it within the closure.
*   We can see this in the debugger. When paused inside the `z()` call, the **Scope** panel will show:
    *   **Local** scope (of `z`, which is the same as `y`).
    *   **`Closure (x)`**: `a: 7`. This explicitly shows that the closure has captured the lexical environment of `x`.

---

## ⁉️ Key Interview Questions & Gotchas

### Does a returned function still have access to its parent's scope?
**Yes.** This is the core definition of a closure. The function maintains a reference to its parent's lexical environment.

### What if the parent variable's value changes?
The closure captures the **reference** to the variable, not the value at that specific point in time.

```js
function x() {
    var a = 7;
    function y() {
        console.log(a);
    }
    a = 100; // Value of 'a' is changed before returning 'y'
    return y;
}

var z = x();
z(); // Output: 100
```
Because the closure holds a *reference* to the memory location of `a`, when `z()` is finally executed, it will log the *current* value of `a`, which is `100`.

---

## 🚀 Use Cases of Closures

Closures are extremely powerful and are used in many common JavaScript patterns:
*   **Module Design Pattern**: Creating private variables and methods.
*   **Currying**: A technique of evaluating a function with multiple arguments into a sequence of functions with a single argument.
*   **Functions like `once`**: Creating functions that can only be executed a single time.
*   **Memoization**: Caching the results of expensive function calls.
*   **Maintaining state in async world**: Managing state in asynchronous operations like `setTimeout`.
*   **Iterators**
*   **Data Hiding and Encapsulation**

---

## ✨ Summary

1.  **A closure is a function that remembers its outer variables and can access them.**
2.  In JavaScript, functions are first-class citizens. They can be passed around like any other value.
3.  When a function is returned from another function, it maintains its **lexical scope**. It remembers where it was created.
4.  This means it has access to its parent's variables and scope, even after the parent function has finished executing.
5.  The closure captures the **reference** to the variables, not their values at a specific moment.
