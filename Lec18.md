
# Lecture 18: Higher-Order Functions ft. Functional Programming

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Introduction to Functional Programming](#-introduction-to-functional-programming)
2.  [What is a Higher-Order Function?](#-what-is-a-higher-order-function)
    *   [Definition](#definition)
    *   [Callback Functions](#callback-functions)
3.  [A Practical Example: The Problem](#-a-practical-example-the-problem)
    *   [The Naive, Repetitive Approach (The Wrong Way)](#the-naive-repetitive-approach-the-wrong-way)
4.  [The Functional Programming Solution (The Right Way)](#-the-functional-programming-solution-the-right-way)
    *   [Step 1: Abstract the Logic](#step-1-abstract-the-logic)
    *   [Step 2: Create a Higher-Order Function](#step-2-create-a-higher-order-function)
    *   [Step 3: Combine and Reuse](#step-3-combine-and-reuse)
5.  [The Beauty of Functional Programming](#-the-beauty-of-functional-programming)
6.  [Polyfill for `map()`: A Deeper Dive](#-polyfill-for-map-a-deeper-dive)
7.  [Key Takeaways for Interviews](#-key-takeaways-for-interviews)

---

## 🚀 Introduction to Functional Programming

**Functional Programming (FP)** is a programming paradigm focused on building software by composing **pure, modular, and reusable functions**. It emphasizes treating functions as first-class citizens, meaning they can be handled like any other variable.

> ✅ **Core Idea:** Functional programming is a way of thinking about code in terms of smaller, reusable functions that can be combined to build more complex logic. This leads to cleaner and more maintainable code.

---

## 🧐 What is a Higher-Order Function?

### Definition
A **Higher-Order Function (HOF)** is a function that does at least one of the following:
*   Takes one or more functions as an argument.
*   Returns a function as its result.

This is a cornerstone concept of functional programming in JavaScript.

### Callback Functions
A function that is passed as an argument to another function is known as a **callback function**. The higher-order function is responsible for "calling it back" at the appropriate time.

```js
// `y` is the Higher-Order Function because it takes a function as an argument.
function y(callback) {
    console.log("Doing my job...");
    callback(); // Calling the callback function
}

// `x` is the Callback Function.
function x() {
    console.log("I am the callback!");
}

y(x); // Passing function x to function y
```

---

## 💻 A Practical Example: The Problem

Let's consider a common programming task. We have an array of radii for four circles, and we need to calculate their **area**, **circumference**, and **diameter**.

```js
const radius = [3, 1, 2, 4];
```

### The Naive, Repetitive Approach (The Wrong Way)
A beginner or someone in a hurry might approach this by writing separate, repetitive functions for each calculation.

```js
// Function to calculate the areas
const calculateArea = function (radiusArr) {
    const output = [];
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(Math.PI * radiusArr[i] * radiusArr[i]);
    }
    return output;
};
console.log(calculateArea(radius));

// Function to calculate the circumferences
const calculateCircumference = function (radiusArr) {
    const output = [];
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(2 * Math.PI * radiusArr[i]);
    }
    return output;
};
console.log(calculateCircumference(radius));
```
> ⚠️ **The Problem:** This code works, but it's not good. It violates the **DRY (Don't Repeat Yourself)** principle. The logic for looping, creating an output array, and returning it is repeated in every function. Only the core formula changes. This code is difficult to maintain and scale.

---

## ✨ The Functional Programming Solution (The Right Way)

We can make this code better, more modular, and reusable by following functional programming principles.

### Step 1: Abstract the Logic
First, let's extract the core calculation logic into its own small, single-purpose functions.

```js
// Logic for calculating the area of a single circle
const area = function (radius) {
    return Math.PI * radius * radius;
};

// Logic for calculating the circumference
const circumference = function (radius) {
    return 2 * Math.PI * radius;
};

// Logic for calculating the diameter
const diameter = function (radius) {
    return 2 * radius;
};
```

### Step 2: Create a Higher-Order Function
Next, we create a single, generic `calculate` function that handles the common parts (looping and returning an array). This function will take the array and the specific calculation `logic` (our callback function) as arguments.

```js
const calculate = function (arr, logic) {
    const output = [];
    for (let i = 0; i < arr.length; i++) {
        // Pass each item to the logic function and push the result
        output.push(logic(arr[i]));
    }
    return output;
};
```
This `calculate` function is now a powerful, reusable, higher-order function.

### Step 3: Combine and Reuse
Now, we can use our HOF with our logic functions to get the results cleanly and without repetition.
```js
const radius = [3, 1, 2, 4];

console.log(calculate(radius, area));
console.log(calculate(radius, circumference));
console.log(calculate(radius, diameter));
```

---

## 🌟 The Beauty of Functional Programming

By abstracting the logic, our code is now:
*   **Modular:** Each function has a single, clear responsibility.
*   **Reusable:** The `calculate` function can be used for any kind of calculation on an array, not just for circles.
*   **Clean and Readable:** The intent of the code is much clearer.
*   **Follows the DRY Principle:** We have eliminated all repeated code.

This functional approach is exactly what the built-in `Array.prototype.map` method does!
```js
// The same result can be achieved with the built-in map HOF
console.log(radius.map(area));
```

---

## 🛠️ Polyfill for `map()`: A Deeper Dive

To make our `calculate` function behave *exactly* like the built-in `map` method, we can attach it to the `Array.prototype`. This is essentially how a polyfill for `map` would be created.

```js
// Attaching our custom map-like function to the Array's prototype
Array.prototype.calculate = function (logic) {
    const output = [];
    // 'this' inside a prototype function refers to the array it is called on (e.g., 'radius')
    for (let i = 0; i < this.length; i++) {
        output.push(logic(this[i]));
    }
    return output;
};

const radius = [3, 1, 2, 4];
console.log(radius.calculate(area)); // Now it works just like map!
```

---

## 💡 Key Takeaways for Interviews

1.  **Code Quality**: Writing code in a functional style demonstrates a mature understanding of software engineering principles. It shows you think about reusability, modularity, and maintainability.
2.  **DRY Principle**: **Don't Repeat Yourself.** If you find yourself writing similar code over and over, abstract that logic into a reusable function. This is a huge green flag for interviewers.
3.  **Higher-Order Functions**: Understand and use HOFs like `map`. Being able to explain *how* they work (like by writing a polyfill) shows a much deeper understanding than just using them.
4.  **Problem Solving**: Instead of a brute-force approach, break down problems into smaller, logical, and functional units. This is the essence of good programming.
