# Lecture 13: Functions in JS - Diving Deep

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Core Function Concepts](#-core-function-concepts)
    *   [Function Statement (aka Function Declaration)](#function-statement-aka-function-declaration)
    *   [Function Expression](#function-expression)
    *   [Anonymous Functions](#anonymous-functions)
    *   [Named Function Expression](#named-function-expression)
2.  [Key Differences: Statement vs. Expression](#-key-differences-statement-vs-expression)
3.  [Parameters vs. Arguments](#-parameters-vs-arguments)
4.  [First-Class Functions (First-Class Citizens)](#-first-class-functions-first-class-citizens)
5.  [Arrow Functions](#-arrow-functions)
6.  [Summary](#-summary)

---

##  फंक्शन Core Function Concepts

This lecture dives deep into the different ways functions can be defined and used in JavaScript, clarifying common points of confusion.

### Function Statement (aka Function Declaration)

This is the most common way to create a function. The terms "Function Statement" and "Function Declaration" are interchangeable.

```js
// This is a Function Statement or Function Declaration
function a() {
    console.log("a called");
}

a(); // Invoking the function
```

### Function Expression

A function expression is when you assign a function to a variable. The function itself can be anonymous.

```js
// This is a Function Expression
var b = function() {
    console.log("b called");
}

b(); // Invoking the function
```
Here, the function acts like a value that is assigned to the variable `b`.

### Anonymous Functions

An anonymous function is a function that does not have a name.

```js
// This is an Anonymous Function
function () {
    // ...
}
```

> ⚠️ **Important:** Anonymous functions cannot be created as Function Statements. They must be used in a context where a function is expected as a value, such as in a Function Expression. Using an anonymous function on its own will result in a `SyntaxError`.
>
> ```js
> function () { } // Uncaught SyntaxError: Function statements require a function name
> ```

### Named Function Expression

This is a variation of a function expression where the function itself has a name.

```js
var b = function xyz() {
    console.log("b called");
    console.log(xyz); // The named function is accessible inside its own scope
}

b();
// xyz(); // Uncaught ReferenceError: xyz is not defined
```
*   In this case, the function `xyz` is assigned to the variable `b`.
*   You can still only call it using `b()`. The name `xyz` is **not created in the outer (global) scope**.
*   The primary use case for a named function expression is for debugging (the name appears in call stacks) or for recursive calls within the function itself.

---

## 🆚 Key Differences: Statement vs. Expression

The main difference between a Function Statement and a Function Expression lies in **hoisting**.

*   **Function Statement (`function a() {}`)**: The entire function is hoisted to the top of the scope. This means you can call the function *before* its declaration in the code.

    ```js
    a(); // Works fine, prints "a called"

    function a() {
        console.log("a called");
    }
    ```

*   **Function Expression (`var b = function() {}`)**: The declaration `var b` is hoisted and initialized with `undefined`, but the function assignment does not happen until the code execution reaches that line.

    ```js
    b(); // Uncaught TypeError: b is not a function

    var b = function() {
        console.log("b called");
    }
    ```
    *   This throws a `TypeError` because at the time of the call, `b` exists but its value is `undefined`, which cannot be invoked as a function.

---

## 🤷‍♂️ Parameters vs. Arguments

These two terms are often used interchangeably, but they have distinct meanings.

*   **Parameters**: The identifiers or labels used in a function's definition to represent the inputs it receives. They are local variables inside the function.

    ```js
    function b(param1, param2) { // param1 and param2 are parameters
        console.log(param1, param2);
    }
    ```

*   **Arguments**: The actual values that are passed to the function when it is invoked.

    ```js
    b(1, "hello"); // 1 and "hello" are arguments
    ```

---

## 🥇 First-Class Functions (First-Class Citizens)

In JavaScript, functions are "First-Class Citizens." This is a powerful concept that means functions can be treated like any other variable or value.

> The ability of functions to be used as values is known as **First-Class Functions**.

This means you can:
1.  **Assign them to variables** (Function Expressions).
2.  **Pass them as arguments** to other functions.
3.  **Return them** from other functions.

This ability is the foundation for higher-order functions and many powerful functional programming patterns in JavaScript.

```js
// Example of passing a function as an argument
function greet(callback) {
    console.log("Hello!");
    callback();
}

greet(function() {
    console.log("The callback was invoked.");
});

// Example of returning a function
function createGreeter() {
    return function() {
        console.log("I was returned from a function!");
    }
}

const myGreeter = createGreeter();
myGreeter();
```

---

## ➡️ Arrow Functions

Arrow functions are a more concise syntax for writing function expressions, introduced in ES6. We will cover them in more detail in a future video, but they are an important part of modern JavaScript function syntax.

---

## ✨ Summary

1.  **Function Statement (Declaration)**: The standard `function name() {}` syntax. They are fully hoisted.
2.  **Function Expression**: Assigning a function to a variable, e.g., `var b = function() {}`. They are not fully hoisted and behave like variables.
3.  **Anonymous Function**: A function without a name. They are used in places where functions are treated as values, like in expressions or as callbacks.
4.  **First-Class Functions**: The core concept that functions in JavaScript can be treated like any other value (assigned, passed as arguments, returned). This makes them "First-Class Citizens."
5.  **Parameters vs. Arguments**: Parameters are the labels in the function definition, while arguments are the actual values passed during invocation.
