
# Lecture 4: Functions & Variable Environments

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan


---

## 📋 Table of Contents

1.  [Introduction](#-introduction)
2.  [Code Example for Analysis](#-code-example-for-analysis)
3.  [Detailed Execution Flow](#-detailed-execution-flow)
    *   [Phase 1: Global Memory Creation](#-phase-1-global-memory-creation)
    *   [Phase 2: Global Code Execution](#-phase-2-global-code-execution)
        *   [Invocation of `a()`](#invocation-of-a)
        *   [Invocation of `b()`](#invocation-of-b)
        *   [Final `console.log(x)`](#final-consolelogx)
4.  [The Call Stack in Action](#-the-call-stack-in-action)
5.  [Key Takeaways](#-key-takeaways)
6.  [What's Next](#-whats-next)

---

## 🚀 Introduction

This lecture builds upon our understanding of the **Execution Context** and the **Call Stack**. We will trace a JavaScript program that involves multiple function invocations to see how their individual variable environments are created and managed.

The core principle remains the same:
> "Everything in JavaScript happens inside an Execution Context."

Every time a function is invoked, a **new, sandboxed execution context** is created and stacked on top of the current context in the Call Stack. This new context has its own separate **variable environment**.

---

## 💻 Code Example for Analysis

We will trace the execution of the following program to understand how JavaScript handles multiple functions and their local variables.

```js
var x = 1;
a();
b();
console.log(x);

function a() {
    var x = 10;
    console.log(x);
}

function b() {
🌨️ var x = 100;
    console.log(x);
}
```

### 🤔 What do you think the output will be?
Before proceeding, try to predict the output. This is a great way to test your understanding.

---

## ⚙️ Detailed Execution Flow

Let's trace the code line by line, visualizing the **Global Execution Context (GEC)** and the **Call Stack**.

### Phase 1: Global Memory Creation

First, the JS engine creates the **Global Execution Context (GEC)** and pushes it onto the Call Stack. It then performs the memory allocation phase for the global scope.

1.  `var x = 1;`: A memory space is created for `x` and initialized with `undefined`.
2.  `function a() {...}`: A memory space is created for `a`, and the *entire function code* is stored within it.
3.  `function b() {...}`: A memory space is created for `b`, and its *entire function code* is stored.

> **💡 Initial Memory State (Global):**
>
> ```
> Memory (Global): {
>   x: undefined,
>   a: <function a code>,
>   b: <function b code>
> }
> ```

### Phase 2: Global Code Execution

Now, the code execution phase begins.

1.  `var x = 1;`: The value `1` is assigned to the global variable `x`.
    *   **Memory (Global):** `x` is now `1`.

2.  `a();`: **Function `a` is invoked.**
    *   A **new Function Execution Context** for `a()` is created and pushed onto the Call Stack.
    *   This new context has its own two phases:
        *   **Memory Creation (for `a`):** A new variable `x` is created in the *local memory* of `a` and initialized with `undefined`.
            *   **Memory (Local `a`):** `{ x: undefined }`
        *   **Code Execution (for `a`):**
            *   `var x = 10;`: The value `10` is assigned to the **local** variable `x`.
            *   **Memory (Local `a`):** `{ x: 10 }`
            *   `console.log(x);`: The engine looks for `x` in the **local scope of `a` first**. It finds `x` with the value `10` and logs it to the console.
            *   **Output:** `10`
    *   The function `a` has finished its execution. Its Execution Context is **popped from the Call Stack and completely destroyed**.

3.  `b();`: **Function `b` is invoked.**
    *   A **brand new Function Execution Context** for `b()` is created and pushed onto the Call Stack.
    *   This context also has its own phases:
        *   **Memory Creation (for `b`):** A new variable `x` is created in the *local memory* of `b` and initialized with `undefined`.
            *   **Memory (Local `b`):** `{ x: undefined }`
        *   **Code Execution (for `b`):**
            *   `var x = 100;`: The value `100` is assigned to the **local** variable `x`.
            *   **Memory (Local `b`):** `{ x: 100 }`
            *   `console.log(x);`: The engine finds `x` in the **local scope of `b`** and logs its value, `100`, to the console.
            *   **Output:** `100`
    *   Function `b` finishes, and its Execution Context is **popped from the Call Stack and destroyed**.

4.  `console.log(x);`: This line is executed in the **Global Execution Context**.
    *   The engine looks for `x` in the **global scope**.
    *   It finds the global `x`, whose value is still `1`.
    *   **Output:** `1`

5.  The program finishes. The GEC is popped from the Call Stack.

### ✅ Final Predicted Output:
```
10
100
1
```
This matches the output seen in the browser console, confirming our understanding of the execution flow.

---

## 📚 The Call Stack in Action

Here's a simplified view of how the Call Stack changed during the program's execution:

1.  **Start:** `[ Global EC ]`
2.  **`a()` invoked:** `[ a() EC, Global EC ]`
3.  **`a()` finishes:** `[ Global EC ]`
4.  **`b()` invoked:** `[ b() EC, Global EC ]`
5.  **`b()` finishes:** `[ Global EC ]`
6.  **Program ends:** `[ ]` (Stack is empty)

---

## ✨ Key Takeaways

1.  **Isolated Environments**: Every function invocation creates a new, independent execution context with its own private variable environment.
2.  **Local Scope**: Variables declared inside a function (like `var x` in `a` and `b`) are local to that function's execution context and do not affect variables with the same name in other contexts (including the global one).
3.  **Call Stack Management**: The Call Stack perfectly manages the creation and deletion of these execution contexts in a Last-In, First-Out (LIFO) order.
4.  **Context Destruction**: Once a function completes, its entire execution context (including its local memory) is wiped out.

---

## 🔮 What's Next

The next video will explore the **shortest JavaScript program** and discuss more advanced concepts like the `window` object and what happens when you run an empty JS file.
