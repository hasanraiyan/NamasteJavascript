# Lecture 7: Scope, Lexical Environment & The Scope Chain

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Introduction: The Link Between Scope and Lexical Environment](#-introduction-the-link-between-scope-and-lexical-environment)
2.  [What is Scope?](#-what-is-scope)
3.  [Core Concepts Explained](#-core-concepts-explained)
    *   [What is a Lexical Environment?](#-what-is-a-lexical-environment)
    *   [What does "Lexical" Mean?](#-what-does-lexical-mean)
    *   [What is the Scope Chain?](#-what-is-the-scope-chain)
4.  [Code Examples and Execution Flow](#-code-examples-and-execution-flow)
    *   [Example 1: Basic Scope Chain](#example-1-basic-scope-chain)
    *   [Example 2: Nested Scope Chain](#example-2-nested-scope-chain)
    *   [Example 3: Scope Chain with Shadowing](#example-3-scope-chain-with-shadowing)
5.  [Visualizing the Call Stack and Lexical Environments](#-visualizing-the-call-stack-and-lexical-environments)
6.  [Summary & Key Takeaways](#-summary--key-takeaways)
7.  [What's Next?](#-whats-next)

---

## 🔗 Introduction: The Link Between Scope and Lexical Environment

Scope in JavaScript is directly related to the concept of the **Lexical Environment**. Understanding the Lexical Environment is crucial as it forms the basis for understanding **Scope**, the **Scope Chain**, and even more advanced topics like **Closures**.

---

## 🤔 What is Scope?

In simple terms, **scope** determines the accessibility of variables and functions in the code. It defines where a specific variable or function can be accessed and where it cannot.

*   When a line of code `console.log(b)` is executed, the JavaScript engine needs to know what `b` is and where to find it. This "where to find it" is determined by scope.

---

## 🧠 Core Concepts Explained

### What is a Lexical Environment?

Whenever a JavaScript **Execution Context** is created, a **Lexical Environment** is also created.

> A Lexical Environment is a combination of two things:
> 1.  **Local Memory:** The memory space of the current context, which includes its variables and functions.
> 2.  **A Reference to the Lexical Environment of its Parent.**

This "reference to the parent" is the key to understanding how the scope chain works.

### What does "Lexical" Mean?

The term **lexical** means "in order" or "in hierarchy." In the context of JavaScript, it refers to the position of a function or variable in the source code.

*   A function `c` written inside another function `a` is said to be *lexically inside* `a`.
*   This physical placement in the code determines the function's "lexical parent."

### ⛓️ What is the Scope Chain?

The **Scope Chain** is the chain of all the lexical environments and their parent references.

*   When the JS engine tries to resolve a variable, it first looks in the **local memory** of the current execution context.
*   If it doesn't find the variable there, it follows the **reference to the lexical environment of the parent**.
*   It then searches for the variable in the parent's memory.
*   This process continues, moving up the chain from one lexical parent to the next, until it reaches the **Global Execution Context**.
*   The lexical parent of the Global Execution Context is `null`. If the variable isn't found anywhere in the chain, the JS engine throws a `ReferenceError`.

---

## 💻 Code Examples and Execution Flow

### Example 1: Basic Scope Chain

```js
function a() {
  console.log(b); // Output: 10
}
var b = 10;
a();
```

**Execution Analysis:**
1.  Function `a` is called. A new Execution Context (EC) for `a` is created.
2.  The `console.log(b)` line is executed.
3.  JS Engine searches for `b` in the **local memory of `a`**. It's not found.
4.  It follows the reference to the **lexical parent**, which is the **Global Scope**.
5.  It finds `b` in the Global Scope with a value of `10`.
6.  `10` is printed to the console.

### Example 2: Nested Scope Chain

```js
function a() {
  function c() {
    console.log(b); // Output: 10
  }
  c();
}
var b = 10;
a();
```

**Execution Analysis:**
1.  `a()` is called. Its EC is created.
2.  `c()` is called. Its EC is created.
3.  `console.log(b)` is executed.
4.  JS Engine searches for `b` in the **local memory of `c`**. Not found.
5.  It follows the reference to `c`'s lexical parent: **the scope of `a`**.
6.  It searches for `b` in the **local memory of `a`**. Still not found.
7.  It follows the reference to `a`'s lexical parent: **the Global Scope**.
8.  It finds `b` in the Global Scope. `10` is printed.

### Example 3: Scope Chain with Shadowing

```js
function a() {
  var b = 100;
  function c() {
    console.log(b); // Output: 100
  }
  c();
}
var b = 10;
a();
```
**Execution Analysis:**
1.  `c()` is called. It tries to find `b`.
2.  Searches local memory of `c`: Not found.
3.  Follows reference to lexical parent `a`.
4.  Searches local memory of `a`: **Found!** The value is `100`.
5.  The search stops here. `100` is printed. The global `var b = 10` is "shadowed" and never reached.

---

## 🖼️ Visualizing the Call Stack and Lexical Environments

When you use a debugger in Chrome, the **Scope** panel gives you a direct view of the scope chain.

Consider the code from **Example 3**:
1.  Place a breakpoint on the `console.log(b)` line inside function `c`.
2.  Run the code.
3.  **Call Stack:** You will see the stack of Execution Contexts: `c`, `a`, `(anonymous)`.
4.  **Scope Panel:** You will see:
    *   **Local**: The local scope of `c` (which is empty).
    *   **Closure (a)**: This is the **Lexical Environment of the parent function `a`**. You will see `b: 100` here. This is a crucial insight: the debugger calls the parent's lexical scope a "Closure".
    *   **Global**: The global scope, which contains `a: function`, `b: 10` (the shadowed variable), etc.

This directly shows the chain: **Local Scope of `c` -> Lexical Env of `a` -> Global Scope**.

---

## ✨ Summary & Key Takeaways

1.  **Scope**: Determines the accessibility of variables.
2.  **Lexical Environment**: Created with every Execution Context. It's the **local memory** plus a **reference to the lexical parent's environment**.
3.  **Lexical**: Refers to the physical placement of code. A function's lexical parent is where it was defined, not where it was called.
4.  **Scope Chain**: The chain of lexical environments. The JS engine traverses this chain to find variables.
5.  The process is always to search the local scope first, and if not found, move up the chain one parent at a time until the global scope is reached.

---

## 🔮 What's Next?

The next video will explore `let` and `const` in-depth, including how they are hoisted, their scope (block scope), and the "temporal dead zone."
