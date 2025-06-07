# Lecture 16: How JavaScript Works + JS Engine & V8 Architecture

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [JavaScript Runtime Environment (JRE) Recap](#-javascript-runtime-environment-jre-recap)
2.  [What is a JavaScript Engine?](#-what-is-a-javascript-engine)
    *   [Key JS Engines](#key-js-engines)
    *   [The First JS Engine](#the-first-js-engine)
3.  [The Three Stages of JS Code Execution](#-the-three-stages-of-js-code-execution)
    *   [Stage 1: Parsing](#stage-1-parsing)
    *   [Stage 2: Compilation](#stage-2-compilation)
    *   [Stage 3: Execution](#stage-3-execution)
4.  [Interpreter vs. Compiler vs. JIT Compilation](#-interpreter-vs-compiler-vs-jit-compilation)
5.  [A Look Inside Google's V8 Engine](#-a-look-inside-googles-v8-engine)
    *   [The V8 Architecture Flow](#the-v8-architecture-flow)
    *   [Key Components of V8](#key-components-of-v8)
6.  [More Engine Internals](#-more-engine-internals)
    *   [Garbage Collector](#garbage-collector)
    *   [Optimization Techniques](#optimization-techniques)
7.  [Summary](#-summary)

---

## 🌐 JavaScript Runtime Environment (JRE) Recap

JavaScript can run in many different places (browsers, servers, smartwatches) because of the **JavaScript Runtime Environment (JRE)**.

A JRE is a container that has everything needed to run JS code. Its core components are:
*   **JS Engine**: The "heart" of the JRE that actually executes the code.
*   **A set of APIs**: These provide the JS engine with extra functionalities specific to the environment.
    *   **Browser JRE**: Includes `window` object, DOM APIs, `localStorage`, `setTimeout`, etc.
    *   **Node.js JRE**: Includes APIs for file system access (`fs`), server creation (`http`), etc.
*   **Event Loop, Callback Queue, Microtask Queue**: These components work with the JS Engine to handle asynchronous operations.

> 💡 The JS Engine is the central piece, but it's the full JRE that gives JavaScript its power and versatility in different environments.

---

## ⚙️ What is a JavaScript Engine?

A JS Engine is **not** a physical machine. It's a sophisticated program, typically written in a low-level language like C++, that executes JavaScript code. Its primary job is to take JS code and convert it into fast, optimized machine code that a computer's processor can run.

### Key JS Engines
*   **V8**: Google's open-source engine, used in Chrome, Node.js, Deno, and Electron.
*   **SpiderMonkey**: The first-ever JS engine, now powering Firefox.
*   **JavaScriptCore**: Apple's engine for Safari.
*   **Chakra**: Microsoft's original engine for Edge (now Edge uses V8).

All engines must adhere to the **ECMAScript standard**, which ensures that JS code runs consistently across different platforms.

### The First JS Engine
The first JS engine was created in 1995 by **Brendan Eich** (the creator of JavaScript) for the Netscape browser. It was later named **SpiderMonkey**.

---

## 🚀 The Three Stages of JS Code Execution

Every JS Engine performs three main steps to run your code:

### Stage 1: Parsing
In this stage, the human-readable JS code is broken down into a structure that the machine can understand.
1.  **Lexical Analysis**: The code is broken down into small individual units called **tokens**.
2.  **Syntax Parsing**: The stream of tokens is passed to a **Syntax Parser**, which checks the code against the language's syntax rules and generates a data structure called an **Abstract Syntax Tree (AST)**.

> An **AST (Abstract Syntax Tree)** is a tree-like representation of the syntactic structure of your code. Each node in the tree denotes a construct occurring in the code.
> You can visualize this process using the amazing tool [astexplorer.net](https://astexplorer.net/).

```js
// This line of code:
const x = 10;

// Becomes a detailed AST, representing a VariableDeclaration
// with a name 'x' and a value of 10.
```

### Stage 2: Compilation
The AST generated from the parsing phase is now passed to the compilation stage. This is where the magic happens. Modern JS engines use a combination of an interpreter and a compiler.

### Stage 3: Execution
Once the code is compiled into executable instructions, it's ready to be run. The execution phase requires two key components:
*   **Memory Heap**: A large, unstructured block of memory where all the variables, objects, and functions are stored.
*   **Call Stack**: The region in memory that tracks function calls. We've covered this extensively in previous lectures.

---

## 🆚 Interpreter vs. Compiler vs. JIT Compilation

This is a classic interview question: Is JavaScript an interpreted or compiled language?

> ✅ **The Answer:** JavaScript was originally an interpreted language. However, modern JS engines use a combination of both interpretation and compilation, known as **Just-In-Time (JIT) Compilation**, to achieve the best performance.

| Method         | How it Works                                                                                             | Pros                               | Cons                                 |
| -------------- | -------------------------------------------------------------------------------------------------------- | ---------------------------------- | ------------------------------------ |
| **Interpreter**  | Reads and executes code line-by-line, without knowing what comes next.                                   | **Fast start-up time**.            | Inefficient for loops/repeated code. |
| **Compiler**     | Takes the entire codebase, compiles it into an optimized machine code file, and then executes that file. | **Highly efficient execution**.      | Slow start-up due to initial compile time. |
| **JIT Compilation** | Uses an interpreter to start executing code quickly, while a profiler identifies "hot" code to be optimized by a compiler on the fly. | **Best of both worlds**: Fast start-up and highly optimized execution. | More complex internal architecture. |

---

## 🔬 A Look Inside Google's V8 Engine

The V8 engine provides a perfect real-world example of JIT compilation.

### The V8 Architecture Flow
1.  **Parsing**: The JS source code is parsed into an **AST**.
2.  **Interpretation**: The AST is passed to the **Ignition** interpreter, which generates **Bytecode**. Bytecode is an abstract machine code, not specific to any processor.
3.  **Execution & Profiling**: The Bytecode is executed. Simultaneously, a **Profiler** watches the code for "hot spots" (functions that are run frequently).
4.  **Optimization**: The hot code is sent to the **TurboFan** optimizing compiler. TurboFan takes the Bytecode and compiles it into highly-optimized **Machine Code** that is specific to the user's CPU architecture.
5.  **Re-Execution**: The next time the "hot" function is called, the highly-optimized Machine Code is executed instead of the Bytecode, resulting in a significant performance boost.
6.  **De-optimization**: If TurboFan makes an incorrect assumption (e.g., a variable type changes), it discards the optimized code and falls back to executing the original Bytecode.

### Key Components of V8
*   **Parser**: Creates the AST.
*   **Ignition**: The interpreter that produces bytecode.
*   **TurboFan**: The optimizing compiler that produces machine code.
*   **Orinoco**: The main garbage collector project within V8.

---

## 🛠️ More Engine Internals

### Garbage Collector
The Garbage Collector's job is to free up memory in the Memory Heap that is no longer in use, preventing memory leaks.
*   V8's GC uses a famous algorithm called **Mark-and-Sweep**. It periodically "marks" all reachable objects and then "sweeps" away the unreachable ones.

### Optimization Techniques
Compilers like TurboFan use many clever tricks to make code faster:
*   **Inlining**: Replacing a function call with the body of the function itself.
*   **Copy Elision**: Avoiding unnecessary object copies.
*   **Inline Caching**: Caching the results of object property lookups to speed up future access.

---

## ✨ Summary

1.  JavaScript code runs inside a **JS Runtime Environment** (like a browser or Node.js), which contains a **JS Engine** at its core.
2.  The JS Engine is a program that executes code in three stages: **Parsing**, **Compilation**, and **Execution**.
3.  **Parsing** involves converting code into an **Abstract Syntax Tree (AST)**.
4.  Modern JS engines are not purely interpreted or compiled; they use **Just-In-Time (JIT) Compilation** for optimal performance.
5.  The **V8 engine** uses an interpreter (**Ignition**) for a fast start and an optimizing compiler (**TurboFan**) to speed up frequently executed code.
6.  The engine also includes a **Memory Heap** for storage and a **Garbage Collector** to manage memory automatically.
