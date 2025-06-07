
# Lecture 9: Block Scope & Shadowing in JS

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [What is a Block?](#-what-is-a-block)
2.  [Block Scope Explained](#-block-scope-explained)
3.  [The Concept of Shadowing](#-the-concept-of-shadowing)
    *   [Shadowing with `var`](#shadowing-with-var)
    *   [Shadowing with `let` and `const`](#shadowing-with-let-and-const)
4.  [Illegal Shadowing](#-illegal-shadowing)
5.  [Lexical Scope in Blocks](#-lexical-scope-in-blocks)
6.  [Summary](#-summary)

---

## 🧱 What is a Block?

In JavaScript, a **block** is defined by a pair of curly braces `{ ... }`. It is used to group multiple statements together.

```js
// This is a block
{
  // This is called a Compound Statement
  var a = 10;
  console.log(a);
}
```

Blocks are necessary where JavaScript expects a single statement, but we need to execute multiple statements. A common example is with `if...else` or loops.

```js
if (true) {
  // This block combines multiple statements into one "compound statement"
  var a = 10;
  console.log(a);
}
```

> 💡 **Note:** If you only need a single statement, the curly braces are not required, but it's still a good practice to use them for clarity.
> `if (true) console.log("Hello");` is valid.

---

## 🌐 Block Scope Explained

**Block scope** refers to the variables and functions that are accessible within a specific block.

*   Variables declared with `let` and `const` are **block-scoped**. This means they are only accessible inside the block where they are defined.

```js
{
  var a = 10;
  let b = 20;
  const c = 30;
  console.log(a); // Output: 10
  console.log(b); // Output: 20
  console.log(c); // Output: 30
}

console.log(a); // Output: 10
console.log(b); // Uncaught ReferenceError: b is not defined
```
*   **`var a`**: `var` is function-scoped (or global-scoped). It is **not** confined to the block. Here, it is attached to the global scope.
*   **`let b` & `const c`**: These are block-scoped. They are stored in a separate memory space reserved for this block. Once the block is executed, they are no longer accessible. Attempting to access `b` outside the block throws a `ReferenceError`.

---

## 👻 The Concept of Shadowing

**Shadowing** occurs when a variable declared within a certain scope (e.g., an inner scope) has the same name as a variable declared in an outer scope. The inner variable "shadows" the outer one.

### Shadowing with `var`

When you shadow a `var` with another `var` inside a block, it behaves differently because `var` is not block-scoped.

```js
var a = 100;
{
  var a = 10; // This 'a' shadows the outer 'a'
  console.log(a); // Output: 10
}
console.log(a); // Output: 10
```
**Explanation:**
*   Both `var a` declarations point to the **same memory location** in the global scope.
*   When `var a = 10` is executed inside the block, it **modifies the value** of the original `a`.
*   Therefore, even outside the block, `a` remains `10`. This can lead to unexpected bugs.

### Shadowing with `let` and `const`

`let` and `const` behave more predictably due to their block-scoped nature.

```js
let b = 100;
{
  let b = 20;     // This 'b' is in the Block Scope
  const c = 30;
  console.log(b); // Output: 20
}
console.log(b); // Output: 100
```
**Explanation:**
*   The outer `let b = 100` is in the `Script` scope.
*   The inner `let b = 20` is in a separate `Block` scope. It is a completely different variable.
*   The inner `b` shadows the outer `b` *only inside the block*.
*   Once execution leaves the block, the inner `b` is gone, and the outer `b` is accessible again. The same principle applies to `const`.

---

## 🚫 Illegal Shadowing

While shadowing is generally permissible, there is a specific rule that makes some shadowing illegal.

> ⚠️ **Rule**: You cannot shadow a `let` variable using a `var` variable within the same scope boundary.

```js
let a = 20;
{
  // var a = 20; // This is ILLEGAL
}
// Uncaught SyntaxError: Identifier 'a' has already been declared
```
**Why is this illegal?**
*   `let a` is block-scoped, but its scope starts from the outer block.
*   `var a` is function-scoped (or global-scoped). The declaration `var a` attempts to cross the boundary of the block and declare itself in the outer scope.
*   Since the outer scope already contains a declaration for `a` (using `let`), this results in a re-declaration error (`SyntaxError`).

✅ **However, the reverse is valid:**
```js
var a = 20;
{
  let a = 20; // This is perfectly valid
  console.log(a); // Output: 20
}
console.log(a); // Output: 20
```
This is valid because the `let a` declaration is contained entirely within the block's scope and does not interfere with the outer `var a`.

---

## ⛓️ Lexical Scope in Blocks

Blocks follow the same lexical scope rules as functions. A variable inside a nested block can be accessed from an even deeper nested block if it's not found locally.

```js
const a = 20;
{
  const a = 100;
  {
    const a = 200;
    console.log(a); // Output: 200
  }
}
```
The engine searches for `a` in the innermost block, finds `200`, and stops. This follows the scope chain.

---

## ✨ Summary

1.  **Block Scope**: `let` and `const` are block-scoped, meaning they exist only within the `{}` block they are defined in. `var` is function-scoped.
2.  **Shadowing**: When an inner variable has the same name as an outer variable.
    *   Shadowing a `var` with `var` modifies the original variable.
    *   Shadowing a `let` with `let` creates a new, separate variable.
3.  **Illegal Shadowing**: It's a `SyntaxError` to shadow a `let` variable with a `var` variable, as `var` declarations try to leak outside their block boundary.
4.  **Lexical Scope**: Blocks follow the principles of the lexical scope chain, resolving variables from inner to outer scopes.
