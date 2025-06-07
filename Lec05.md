
# Lecture 5: The Shortest JS Program & The Global Space


*   **Video Author**: Akshay Saini  
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan



---

## 📋 Table of Contents

1.  [The Shortest JavaScript Program](#-the-shortest-javascript-program)
2.  [The Global Object: `window`](#-the-global-object-window)
3.  [The Global `this` Keyword](#-the-global-this-keyword)
4.  [The Global Space](#-the-global-space)
    *   [How Global Variables & Functions are Handled](#how-global-variables--functions-are-handled)
    *   [Demonstration of Global Scope](#demonstration-of-global-scope)
5.  [Summary](#-summary)
6.  [What's Next](#-whats-next)

---

## 📜 The Shortest JavaScript Program

The shortest valid JavaScript program is an **empty file**.

> 🤔 **Question:** What happens when you run an empty `.js` file in a browser? Does the JavaScript engine do anything?

Yes, it does! Even with an empty file, the JavaScript engine performs several crucial setup tasks:
1.  It creates a **Global Execution Context (GEC)**.
2.  It pushes the GEC onto the **Call Stack**.
3.  It sets up a **Global Object** and a `this` variable.

We can see this by opening the Chrome DevTools on a page with an empty script and placing a breakpoint on the first line. The **Call Stack** will show the `(anonymous)` global context, and the **Scope** panel will show a `Global` object.

---

## 🌐 The Global Object: `window`

When a JavaScript program runs in a browser, the JS engine creates a large **Global Object**. In browsers, this object is called `window`.

*   The `window` object is created by the JS engine and loaded with many functions and variables that are part of the browser environment (APIs).
*   Examples include `alert()`, `setTimeout()`, `localStorage`, `document`, and more.
*   These can be accessed from anywhere in your JavaScript program.

You can inspect this object by typing `window` in the browser console.

```js
// Open your browser console and type:
window;
```
You will see a large object with hundreds of properties and methods.

---

## 👉 The Global `this` Keyword

Along with the `window` object, the JS engine also creates the `this` keyword.

*   At the global level (i.e., outside of any function), `this` points to the **Global Object**.
*   In browsers, this means `this` points to `window`.

You can verify this by running the following command in your console:
```js
this === window;
// Output: true
```
Therefore, at the global level, `this` and `window` are the same.

---

## 🌌 The Global Space

**Global Space** refers to any part of your code that is **not inside a function**.

```js
// This is the global space
var a = 10; // 'a' is a global variable

function b() {
  // This is the local space of function b
  var x = 10;
}

// This is also the global space
```

### How Global Variables & Functions are Handled

When you create a variable or a function in the global space, it gets attached as a property to the **Global Object** (`window` in browsers).

#### Code Example
Let's analyze this simple program:
```js
var a = 10;

function b() {
  var x = 10;
}

console.log(window.a);   // -> 10
console.log(a);          // -> 10 (assumes window.a)
console.log(this.a);     // -> 10 (this === window at global level)

console.log(x); // -> Uncaught ReferenceError: x is not defined
```

### Demonstration of Global Scope
1.  `var a = 10;`:
    *   `a` is created in the global space.
    *   It becomes a property of the `window` object: `window.a`.

2.  `function b() {...}`:
    *   `b` is also created in the global space.
    *   It becomes a property of the `window` object: `window.b`.

3.  Accessing `a`:
    *   `window.a` directly accesses the property on the `window` object.
    *   `a` works because JavaScript automatically assumes you are referring to `window.a` when the variable is in the global scope.
    *   `this.a` works because at the global level, `this` is `window`, so `this.a` is the same as `window.a`.

4.  Accessing `x`:
    *   `var x = 10;` is declared *inside* the function `b`. It is **local** to `b`'s execution context.
    *   It is **not** in the global space and does **not** get attached to the `window` object.
    *   Therefore, trying to access `x` from the global space results in a `ReferenceError`, because `x` is not defined in the global scope.

---

## ✨ Summary

1.  The shortest JavaScript program is an **empty file**.
2.  When a JS program runs, the engine creates a **Global Execution Context (GEC)**, a **Global Object** (`window` in browsers), and a `this` variable.
3.  **Global Space** is any code written outside of a function.
4.  Variables and functions declared in the global space are attached as properties to the **Global Object** (`window`).
5.  At the global level, `this` is strictly equal to the `window` object (`this === window`).
6.  You can access global variables either directly (`a`), via the window object (`window.a`), or via the `this` keyword (`this.a`).

---

## 🔮 What's Next

The next lecture will take a deeper dive into the concepts of **`undefined`** and **`not defined`**, exploring their differences and how they relate to the execution context and memory allocation.
