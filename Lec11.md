
# Lecture 11: setTimeout + Closures Interview Question

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [The Famous Interview Question](#-the-famous-interview-question)
    *   [Problem Statement](#problem-statement)
    *   [The Unexpected Output](#the-unexpected-output)
2.  [Why Does It Behave This Way? (The `var` Problem)](#-why-does-it-behave-this-way-the-var-problem)
3.  [Solution 1: Using `let`](#-solution-1-using-let)
4.  [Solution 2: Using Closures with `var`](#-solution-2-using-closures-with-var)
5.  [Summary: Key Takeaways](#-summary-key-takeaways)

---

## ❓ The Famous Interview Question

This is one of the most popular and tricky JavaScript interview questions, designed to test a deep understanding of closures, the event loop, and variable scope.

### Problem Statement
The task is to write a script that prints the numbers 1, 2, 3, 4, and 5 to the console, with each number appearing after a delay corresponding to its value in seconds (i.e., 1 after 1 second, 2 after 2 seconds, etc.).

A common but incorrect first attempt looks like this:

```js
function x() {
    for (var i = 1; i <= 5; i++) {
        setTimeout(function() {
            console.log(i);
        }, i * 1000);
    }
    console.log("Namaste JavaScript");
}
x();
```

### The Unexpected Output

Instead of printing `1, 2, 3, 4, 5`, the code prints:
*   "Namaste JavaScript" (immediately)
*   `6` (after 1 second)
*   `6` (after 2 seconds)
*   `6` (after 3 seconds)
*   `6` (after 4 seconds)
*   `6` (after 5 seconds)

---

## 🤔 Why Does It Behave This Way? (The `var` Problem)

The root of the problem lies in the combination of `setTimeout` callbacks, closures, and the function-scoped nature of `var`.

1.  **Closures and Shared Reference**: The callback function passed to `setTimeout` forms a closure. This closure captures a **reference** to the variable `i`, not its value at the time the loop iterates. Because `i` is declared with `var`, all five callback functions are sharing a reference to the *same* `i` in the parent scope.

2.  **Loop Execution Speed**: The `for` loop executes very quickly. It doesn't pause for the `setTimeout` timers. It iterates five times, scheduling five callbacks to run in the future, and then the loop terminates.

3.  **Final Value of `i`**: By the time the loop finishes, the value of `i` has become `6` (the condition `i <= 5` is no longer true).

4.  **Callback Execution**: Much later, when the `setTimeout` timers finally expire (after 1s, 2s, 3s, etc.), the callback functions are executed. When they look up the value of `i` via their closure, they all see the *current* value of `i` in memory, which is `6`.

> ✅ **Key Concept:** The closure remembers the **reference** to the variable `i`, not its value at each iteration. All callbacks point to the same memory location for `i`.

---

## ✅ Solution 1: Using `let`

The simplest and most modern way to fix this problem is to replace `var` with `let`.

```js
function x() {
    for (let i = 1; i <= 5; i++) { // Using 'let' instead of 'var'
        setTimeout(function() {
            console.log(i);
        }, i * 1000);
    }
    console.log("Namaste JavaScript");
}
x();
```

**Correct Output:**
*   "Namaste JavaScript"
*   `1` (after 1s)
*   `2` (after 2s)
*   ...and so on.

### Why `let` Works
*   **Block Scope**: The `let` keyword is **block-scoped**. This means that for every single iteration of the `for` loop, a **new, separate copy** of `i` is created.
*   **Unique Closures**: Each `setTimeout` callback now forms a closure over a *different* instance of `i`.
    *   The first callback closes over an `i` whose value is `1`.
    *   The second callback closes over a completely separate `i` whose value is `2`.
    *   ...and so on.
*   Because each callback has a unique, block-scoped variable in its closure, it prints the correct value when executed.

---

## ✅ Solution 2: Using Closures with `var`

If the interviewer insists on solving the problem using `var`, you can use the power of closures explicitly. This demonstrates a deeper understanding of how closures work.

The solution is to wrap the `setTimeout` in another function.

```js
function x() {
    for (var i = 1; i <= 5; i++) {
        function close(x) { // Create a new function scope
            setTimeout(function() {
                console.log(x);
            }, x * 1000);
        }
 приятелi); // Call the function on each iteration
    }
    console.log("Namaste JavaScript");
}
x();
```

### Why This Works
1.  On each iteration of the loop, we call the `close()` function.
2.  We pass the current value of `i` into `close()`.
3.  When `close(i)` is called, a **new execution context** is created. The value of `i` is passed into the local variable `x` of the `close` function.
4.  Crucially, a **new copy of `x`** is created for every single iteration.
5.  The `setTimeout` callback now forms a closure over this new, separate variable `x`.
6.  Each callback has its own unique value of `x` preserved in its closure, leading to the correct output.

> 💡 **Note:** This manual closure technique effectively achieves what `let` does automatically with block scoping.

---

## ✨ Summary: Key Takeaways

1.  **The Problem**: When using `var` in a loop with `setTimeout`, all callbacks share a closure over the **same variable reference**. By the time they execute, the loop has finished, and they all see the variable's final value.
2.  **The `let` Solution**: `let` is block-scoped. It creates a new variable for each loop iteration, so each callback's closure captures a unique variable with the correct value. This is the modern, preferred solution.
3.  **The `var` + Closure Solution**: By wrapping the `setTimeout` in another function and passing `i` as an argument, we manually create a new scope for each iteration, effectively giving each callback its own unique variable to close over. This demonstrates a deep understanding of closure mechanics.
