# Lecture 19: Map, Filter & Reduce

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Introduction to `map`, `filter`, & `reduce`](#-introduction-to-map-filter--reduce)
2.  [Array.prototype.map()](#-arrayprototypemap)
    *   [Definition & Use Case](#definition--use-case)
    *   [Code Examples](#code-examples)
3.  [Array.prototype.filter()](#-arrayprototypefilter)
    *   [Definition & Use Case](#definition--use-case-1)
    *   [Code Examples](#code-examples-1)
4.  [Array.prototype.reduce()](#-arrayprototypereduce)
    *   [Definition & Use Case](#definition--use-case-2)
    *   [Code Examples: Sum and Max](#code-examples-sum-and-max)
5.  [Real-World Examples with an Array of Objects](#-real-world-examples-with-an-array-of-objects)
    *   [Example 1: Using `map` to get full names](#example-1-using-map-to-get-full-names)
    *   [Example 2: Using `reduce` to group by age](#example-2-using-reduce-to-group-by-age)
6.  [Method Chaining](#-method-chaining)
    *   [Example: Filter and Map](#example-filter-and-map)
    *   [Challenge: Re-implementing with `reduce`](#challenge-re-implementing-with-reduce)
7.  [Summary](#-summary)

---

## 🚀 Introduction to `map`, `filter`, & `reduce`

`map`, `filter`, and `reduce` are three of the most popular and powerful higher-order functions in JavaScript for working with arrays. Understanding them is crucial for writing clean, modern, and efficient code.

---

## 🗺️ Array.prototype.map()

### Definition & Use Case
The `map` method is used to **transform** an array. It creates a **new array** populated with the results of calling a provided function on every element in the calling array.

> 💡 **When to use `map`:** When you want to transform each element of an array into something else. The new array will always be the same size as the original array.

### Code Examples

Let's start with a simple array of numbers.
```js
const arr = [5, 1, 3, 2, 6];
```

#### Example 1: Doubling Each Value
The transformation logic is to multiply each element by 2.
```js
function double(x) {
    return x * 2;
}

const output = arr.map(double);
console.log(output); // Output: [10, 2, 6, 4, 12]
```

#### Example 2: Converting to Binary
The transformation logic is to convert each number to its binary string representation.
```js
function binary(x) {
    return x.toString(2);
}

const output = arr.map(binary);
console.log(output); // Output: ["101", "1", "11", "10", "110"]
```

✅ **Modern Syntax:** You can also write the logic directly inside the map call using an arrow function for more concise code.
```js
const output = arr.map(x => x * 2);
console.log(output); // Output: [10, 2, 6, 4, 12]
```

---

## 🔍 Array.prototype.filter()

### Definition & Use Case
The `filter` method is used to **filter** elements from an array. It creates a **new array** with all elements that pass the test implemented by the provided callback function.

> 💡 **When to use `filter`:** When you want to select a subset of elements from an array based on a certain condition.

The callback function must return a `boolean` (`true` or `false`). If it returns `true`, the element is included in the new array.

### Code Examples
```js
const arr = [5, 1, 3, 2, 6];
```

#### Example 1: Filtering Odd Numbers
The logic is to check if a number is odd.
```js
function isOdd(x) {
    return x % 2 !== 0; // or simply x % 2
}

const output = arr.filter(isOdd);
console.log(output); // Output: [5, 1, 3]
```

#### Example 2: Filtering Values Greater Than 4
```js
const output = arr.filter(x => x > 4);
console.log(output); // Output: [5, 6]
```
---

## ⚗️ Array.prototype.reduce()

### Definition & Use Case
The `reduce` method is the most versatile of the three. It is used when you need to take all the elements of an array and come up with a **single value** out of them. This single value could be a number, string, object, or another array.

> 💡 **When to use `reduce`:** When you want to iterate over an array to accumulate a single result. Common use cases are finding the sum, max, or min value.

### Code Examples: Sum and Max

#### Example 1: Finding the Sum of Array Elements
Let's find the sum of all numbers in the array. First, let's look at the traditional, non-functional way.
```js
// Traditional Way
function findSum(arr) {
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        sum = sum + arr[i];
    }
    return sum;
}
console.log(findSum(arr)); // Output: 17
```

Now, let's do the same with `reduce`.
```js
const output = arr.reduce(function(accumulator, current) {
    // accumulator: accumulates the result (like 'sum' in the traditional loop)
    // current: the current element of the array being processed
    accumulator = accumulator + current;
    return accumulator;
}, 0); // 0 is the initial value for the accumulator

console.log(output); // Output: 17
```

#### Example 2: Finding the Maximum Value in an Array
```js
const output = arr.reduce(function(max, current) {
    if (current > max) {
        max = current;
    }
    return max;
}, 0); // initial value of max is 0

console.log(output); // Output: 6
```
---

## 🏢 Real-World Examples with an Array of Objects

Let's use a more complex data structure, which is common in real applications.
```js
const users = [
  { firstName: "akshay", lastName: "saini", age: 26 },
  { firstName: "donald", lastName: "trump", age: 75 },
  { firstName: "elon", lastName: "musk", age: 50 },
  { firstName: "deepika", lastName: "padukone", age: 26 },
];
```

### Example 1: Using `map` to get full names
**Task:** Get an array of full names like `["akshay saini", "donald trump", ...]`.
```js
const output = users.map(user => user.firstName + " " + user.lastName);

console.log(output);
// Output: ["akshay saini", "donald trump", "elon musk", "deepika padukone"]
```

### Example 2: Using `reduce` to group by age
**Task:** Get an object showing how many users have a particular age. e.g., `{ 26: 2, 75: 1, 50: 1 }`. This is a classic and very useful interview question.

```js
const output = users.reduce(function(acc, curr) {
    // If the age already exists as a key in our accumulator object, increment it
    if (acc[curr.age]) {
        acc[curr.age]++;
    }
    // Otherwise, initialize it to 1
    else {
        acc[curr.age] = 1;
    }
    return acc; // Always return the accumulator
}, {}); // The initial value is an empty object

console.log(output); // Output: { 26: 2, 50: 1, 75: 1 }
```

---

## 🔗 Method Chaining

One of the most powerful features of these methods is that they can be **chained** together. The output of one method can be the input for the next.

### Example: Filter and Map
**Task:** Get the first name of all users whose age is less than 30.

```js
const output = users
  .filter(user => user.age < 30) // First, filter users with age < 30
  .map(user => user.firstName); // Then, map the result to get just the first name

console.log(output); // Output: ["akshay", "deepika"]
```
This is a very clean and readable way to write complex data transformations.

### Challenge: Re-implementing with `reduce`
You can achieve the same result as the chaining example above using only `reduce`. This is a great exercise to test your understanding.

> 📝 **Homework:** Try to write the code for the task above (first name of users with age < 30) using only the `reduce` method. Post your solution in the comments!

---

## ✨ Summary
*   **`map`**: Use it to **transform** every element in an array into something new.
*   **`filter`**: Use it to **select** a subset of elements based on a condition.
*   **`reduce`**: Use it to take an entire array and **reduce it** to a single output (a number, object, etc.). It is the most powerful of the three.
*   **Chaining**: Combine these methods to perform complex data manipulations in a clean and declarative way.
