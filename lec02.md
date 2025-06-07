
# Lecture 2: How JavaScript Code is Executed & Call Stack

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan
---

## 📋 Table of Contents

1.  [Recap: The Execution Context](#-recap-the-execution-context)
2.  [The Two Phases of Execution](#-the-two-phases-of-execution)
3.  [Code Example for Analysis](#-code-example-for-analysis)
4.  [Step-by-Step Execution Walkthrough](#-step-by-step-execution-walkthrough)
    *   [Phase 1: Memory Creation](#-phase-1-memory-creation)
    *   [Phase 2: Code Execution](#-phase-2-code-execution)
5.  [Introducing the Call Stack](#-introducing-the-call-stack)
6.  [Summary](#-summary)

---

## 🔄 Recap: The Execution Context

> "Everything in JavaScript happens inside an **Execution Context**."

When any JavaScript code is run, a **Global Execution Context (GEC)** is created. This context has two components:
1.  **Memory Component (Variable Environment)**: Stores variables and functions.
2.  **Code Component (Thread of Execution)**: Executes code line by line.

---

## 🔢 The Two Phases of Execution

The creation and execution of an Execution Context happens in two distinct phases:

1.  **Memory Creation Phase**: The JavaScript engine scans the code and allocates memory to all variables and functions.
2.  **Code Execution Phase**: The JavaScript engine executes the code line by line.

---

## 💻 Code Example for Analysis

We will use the following simple JavaScript program to trace its execution step by step.

```js
var n = 2;

function square(num) {
  var ans = num * num;
  return ans;
}

var square2 = square(n);
var square4 = square(4);
```

---

## 🚀 Step-by-Step Execution Walkthrough

When we run this code, the **Global Execution Context (GEC)** is created and pushed to the Call Stack.

### Phase 1: Memory Creation

The JS engine scans the entire code from top to bottom (lines 1 to 8) and allocates memory.

1.  **Line 1 (`var n = 2;`)**: It finds a variable `n`. Memory is allocated for `n`, and a special placeholder value `undefined` is stored.
2.  **Line 3 (`function square...`)**: It finds a function declaration `square`. Memory is allocated for `square`, and the *entire code of the function* is placed in its memory space.
3.  **Line 7 (`var square2 = ...`)**: It finds a variable `square2`. Memory is allocated, and it's initialized with `undefined`.
4.  **Line 8 (`var square4 = ...`)**: It finds a variable `square4`. Memory is allocated, and it's also initialized with `undefined`.

> **💡 Initial Memory State (After Phase 1):**
>
> ```
> Memory: {
>   n: undefined,
>   square: <function code>,
>   square2: undefined,
>   square4: undefined
> }
> ```

### Phase 2: Code Execution

Now, the JS engine executes the code line by line, updating the values in memory.

1.  **Line 1 (`var n = 2;`)**: The value `2` is placed into `n`.
    *   **Memory State**: `n` is now `2`.

2.  **Lines 3-6 (Function Definition)**: Nothing to execute here, as function definitions are handled in the memory creation phase. The engine skips these lines.

3.  **Line 7 (`var square2 = square(n);`)**: This line involves a **function invocation**.
    *   A **new Execution Context** is created for the `square(n)` call. It is a completely new, sandboxed environment.
    *   This new EC is pushed onto the **Call Stack** on top of the GEC.
    *   This new EC goes through its own two phases:
        *   **Memory Creation (for `square(n)`):**
            *   Parameter `num`: `undefined`
            *   Variable `ans`: `undefined`
        *   **Code Execution (for `square(n)`):**
            *   The argument `n` (which is `2`) is passed to the parameter `num`.
            *   **Memory**: `num` becomes `2`.
            *   `var ans = num * num;` is executed. `2 * 2` is `4`.
            *   **Memory**: `ans` becomes `4`.
            *   `return ans;` is executed. The function returns the value of `ans` (`4`) to where it was called.
            *   As soon as the `return` statement is executed, the entire execution context for `square(n)` is **deleted** and popped from the Call Stack.
    *   Control returns to line 7. The value `4` (returned from the function) is assigned to `square2`.
    *   **Memory State**: `square2` is now `4`.

4.  **Line 8 (`var square4 = square(4);`)**: Another function invocation.
    *   The process repeats: a brand new Execution Context for `square(4)` is created and pushed to the Call Stack.
    *   **Memory Creation (for `square(4)`):**
        *   Parameter `num`: `undefined`
        *   Variable `ans`: `undefined`
    *   **Code Execution (for `square(4)`):**
        *   The argument `4` is passed to the parameter `num`.
        *   **Memory**: `num` becomes `4`.
        *   `var ans = num * num;` is executed. `4 * 4` is `16`.
        *   **Memory**: `ans` becomes `16`.
        *   `return ans;` is executed. The function returns `16`.
        *   The execution context for `square(4)` is **deleted** and popped from the Call Stack.
    *   Control returns to line 8. The value `16` is assigned to `square4`.
    *   **Memory State**: `square4` is now `16`.

5.  **End of Program**: After line 8, there's no more code to run. The Global Execution Context is also popped from the Call Stack, and the program finishes.

---

## 📚 Introducing the Call Stack

The **Call Stack** is a fundamental mechanism in JavaScript used to manage execution contexts.

*   **Purpose**: To keep track of which function is currently being run.
*   **Behavior**: It operates on a Last-In, First-Out (LIFO) principle.
    *   When a script is loaded, the **Global Execution Context (GEC)** is pushed to the bottom of the stack.
    *   Whenever a function is invoked, a **new execution context** for that function is created and **pushed** on top of the stack.
    *   When the function finishes (hits a `return` or the end of its code), its execution context is **popped** off the stack.
    *   The program finishes when the stack is empty (the GEC is popped).

> ✅ **Other Names for the Call Stack:**
> The Call Stack is also known by many other names, which all refer to the same concept:
> *   Execution Context Stack
> *   Program Stack
> *   Control Stack
> *   Runtime Stack
> *   Machine Stack

---

## ✨ Summary

1.  **Global Execution Context (GEC)** is created and pushed to the Call Stack.
2.  **Memory Creation Phase**: Memory is allocated for all variables (`undefined`) and functions (full code).
3.  **Code Execution Phase**: Code is executed line by line.
    *   Variables are assigned their actual values.
    *   When a function is **invoked**:
        *   A new, separate **Function Execution Context** is created.
        *   This new context is pushed onto the **Call Stack**.
        *   It runs through its own memory and code execution phases.
        *   Once the function returns, its execution context is **popped** from the stack.
4.  The entire program is finished once the GEC is popped from the Call Stack.