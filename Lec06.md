
# Lecture 6: Undefined vs Not Defined & Loosely Typed Languages

*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Core Concept: `undefined` vs. `not defined`](#-core-concept-undefined-vs-not-defined)
2.  [Revisiting the Memory Creation Phase](#-revisiting-the-memory-creation-phase)
3.  [What is `undefined`?](#-what-is-undefined)
    *   [Example 1: Before Execution](#example-1-before-execution)
    *   [Example 2: Logging before Declaration](#example-2-logging-before-declaration)
4.  [What is `not defined`?](#-what-is-not-defined)
5.  [Key Differences Summarized](#-key-differences-summarized)
6.  [JavaScript as a Loosely Typed Language](#-javascript-as-a-loosely-typed-language)
7.  [⚠️ A Pro-Tip: Avoid Assigning `undefined`](#️-a-pro-tip-avoid-assigning-undefined)
8.  [Summary](#-summary)

---

## 🤔 Core Concept: `undefined` vs. `not defined`

In JavaScript, `undefined` and `not defined` are two distinct concepts that often cause confusion. Understanding their difference is fundamental to grasping how JavaScript manages memory and executes code.

*   `undefined`: A variable has been declared, but no value has been assigned to it. JavaScript's engine assigns `undefined` as a placeholder.
*   `not defined`: A variable has **not** been declared in the memory at all. Trying to access it results in a `ReferenceError`.

---

## 🔄 Revisiting the Memory Creation Phase

As we've learned, JavaScript code execution happens in two phases. The first phase is **Memory Creation**.

*   During this phase, the JavaScript engine scans the code and allocates memory for all variables and functions.
*   For variables declared with `var`, it assigns a special placeholder value: `undefined`.
*   This process happens *before* a single line of code is executed.

This initial placeholder assignment is the root cause of the `undefined` behavior.

---

## 🤷‍♂️ What is `undefined`?

`undefined` is a special keyword and a primitive type in JavaScript. It signifies that a variable has been allocated memory but has not yet been assigned a value.

### Example 1: Before Execution

Let's use the debugger to see this in action.
```js
var a = 7;
```
If we place a breakpoint on this line and inspect the **Global Scope** *before* it executes, we see that memory has already been allocated for `a`.
*   **Memory State (before execution):** `a: undefined`
*   **Memory State (after execution):** `a: 7`

### Example 2: Logging before Declaration

This is a classic hoisting example that demonstrates the concept clearly.
```js
console.log(a); // Output: undefined
var a = 7;
console.log(a); // Output: 7
```
*   **Why `undefined`?**: In the memory creation phase, `a` is given memory and the value `undefined`. When `console.log(a)` runs, it prints the value currently stored in `a`'s memory, which is `undefined`.
*   **Why `7`?**: After the assignment `a = 7` is executed, the value in memory is updated, so the second `console.log(a)` prints `7`.

---

## 🚫 What is `not defined`?

A variable is `not defined` when it has not been allocated memory in the current scope. Attempting to access such a variable will throw a `ReferenceError`.

```js
console.log(a); // Output: undefined
var a = 7;

console.log(x); // Throws -> Uncaught ReferenceError: x is not defined
```
In this example:
*   `a` is declared, so it exists in memory (initially as `undefined`).
*   `x` was **never declared** in the program. The JS engine cannot find it in memory, resulting in a `ReferenceError`.

---

## ✨ Key Differences Summarized

| Concept        | Meaning                                                                 | Example                                               | Result                               |
| -------------- | ----------------------------------------------------------------------- | ----------------------------------------------------- | ------------------------------------ |
| **`undefined`**  | The variable is declared, but no value is assigned yet. It's a placeholder. | `console.log(a);` <br> `var a;`                        | Prints `undefined`                   |
| **`not defined`**| The variable has not been declared at all. No memory is allocated.      | `console.log(b);` <br> (where `b` is never declared) | Throws `ReferenceError: b is not defined` |

---

## 🌐 JavaScript as a Loosely Typed Language

JavaScript is a **loosely typed** (or weakly typed) language. This means you do not have to explicitly declare the data type of a variable. A single variable can hold different data types throughout its lifecycle.

```js
var a;
console.log(a); // Output: undefined

a = 10;
console.log(a); // Output: 10

a = "hello world";
console.log(a); // Output: hello world
```
1.  Initially, `a` is `undefined`.
2.  Then, `a` holds a `number` (`10`).
3.  Later, `a` holds a `string` (`"hello world"`).

This flexibility is a core feature of JavaScript and contrasts with strictly-typed languages (like Java or C++) where a variable's type is fixed upon declaration.

---

## ⚠️ A Pro-Tip: Avoid Assigning `undefined`

While you *can* manually assign `undefined` to a variable, it is considered **bad practice**.

```js
var a = undefined; // Please avoid doing this!
```
*   **Why is it bad?** `undefined` is primarily meant to be a default value assigned by the JavaScript engine during the memory creation phase. It acts as a signal that a variable has not yet been initialized.
*   By assigning `undefined` yourself, you are overwriting its natural meaning. This can lead to confusion and make debugging harder, as you can no longer be sure if a variable is `undefined` because it's in its initial state or because you explicitly set it that way.

> ✅ **Best Practice:** If you need to indicate that a variable is intentionally empty, assign `null` to it instead. This preserves the special meaning of `undefined`.

---

## 📝 Summary

1.  **`undefined`** is a value automatically assigned by JavaScript to variables that have been declared but not yet initialized.
2.  **`not defined`** is a `ReferenceError` that occurs when you try to access a variable that has never been declared.
3.  JavaScript is a **loosely typed** language, allowing a variable to hold values of any data type without explicit type declaration.
4.  **Best Practice**: Avoid setting variables to `undefined` manually. Use `null` for cases where you want to explicitly denote an empty or non-existent value.
