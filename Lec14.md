# Lecture 14: Asynchronous JavaScript & EVENT LOOP from scratch

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Recap: JavaScript's Synchronous Nature](#-recap-javascripts-synchronous-nature)
2.  [Introducing Asynchronous Operations](#-introducing-asynchronous-operations)
3.  [The JavaScript Runtime Environment](#-the-javascript-runtime-environment)
    *   [JS Engine (V8, etc.)](#js-engine-v8-etc)
    *   [Web APIs](#web-apis)
    *   [Callback Queue (Task Queue)](#callback-queue-task-queue)
    *   [Microtask Queue](#microtask-queue)
    *   [The Event Loop](#the-event-loop)
4.  [Execution Flow of Asynchronous Code](#-execution-flow-of-asynchronous-code)
    *   [Code Example](#code-example)
    *   [Step-by-Step Walkthrough](#step-by-step-walkthrough)
5.  [The Role of the Event Loop](#-the-role-of-the-event-loop)
6.  [Other Asynchronous Web APIs](#-other-asynchronous-web-apis)
7.  [Summary](#-summary)

---

## 🔄 Recap: JavaScript's Synchronous Nature

As established in earlier lectures, the core of JavaScript is **synchronous** and **single-threaded**.
*   This means the JavaScript engine, with its single **Call Stack**, can only execute one command at a time.
*   If a long-running operation blocks the Call Stack, it freezes the entire browser page, leading to a poor user experience. This is known as **blocking the main thread**.

---

## ⚡ Introducing Asynchronous Operations

To solve the problem of blocking the main thread, we use **asynchronous operations**. These are tasks that can be started now and finished later, without stopping the rest of the program from running.

The `setTimeout` function is a classic example.

```js
console.log("Start");

setTimeout(function cb() {
  console.log("Callback");
}, 5000);

console.log("End");
```

**Expected Output:**
```
Start
End
// ... (after 5 seconds) ...
Callback
```

This output proves that JavaScript did not wait for the 5-second timer to finish. But if JavaScript is synchronous, how is this possible? The answer lies in the full **JavaScript Runtime Environment**.

---

## 🌐 The JavaScript Runtime Environment

JavaScript itself doesn't have the built-in capability for asynchronous operations. These capabilities are provided by the environment it runs in, such as a web browser.

The main components are:
1.  **JS Engine** (containing the Call Stack)
2.  **Web APIs** (provided by the browser)
3.  **Callback Queue**
4.  **Microtask Queue** (for high-priority callbacks, discussed later)
5.  **Event Loop**



### JS Engine (V8, etc.)
*   This is the heart of JavaScript, containing the **Call Stack** where code is executed line by line.

### Web APIs
*   These are APIs provided by the browser environment, not the JS engine itself. They handle asynchronous tasks.
*   Examples include:
    *   `setTimeout()`
    *   `DOM APIs` (like `addEventListener`)
    *   `fetch()`
    *   `localStorage`
    *   `console` (Yes, `console` is also part of the browser APIs!)

### Callback Queue (Task Queue)
*   This is a queue that holds the callback functions that are ready to be executed.
*   When an asynchronous operation (like a `setTimeout` timer) completes, its callback function is placed in this queue.
*   It follows a First-In, First-Out (FIFO) principle.

### Microtask Queue
*   Similar to the Callback Queue but has a **higher priority**.
*   Callbacks from Promises (`.then`, `.catch`) and MutationObserver go into this queue.
*   The Event Loop will always empty the Microtask Queue completely before looking at the Callback Queue. This will be covered in detail in a later video.

### The Event Loop
*   The Event Loop is the conductor of this whole orchestra. It has one simple job.

> 💡 **The Job of the Event Loop:**
> To continuously monitor the **Call Stack** and the **Callback Queue**. If the Call Stack is empty, it takes the first function from the Callback Queue and pushes it onto the Call Stack for execution.

---

## 🚀 Execution Flow of Asynchronous Code

Let's trace our `setTimeout` example to see how all these components work together.

### Code Example

```js
console.log("Start");

setTimeout(function cb() {
    console.log("Callback");
}, 5000);

console.log("End");
```

### Step-by-Step Walkthrough

1.  **GEC Creation:** A Global Execution Context is created and pushed onto the **Call Stack**.
2.  **`console.log("Start")`:** This is executed by the Call Stack. "Start" is printed to the console. The function is popped from the stack.
3.  **`setTimeout(...)`:** The Call Stack executes the `setTimeout` call.
    *   Instead of handling the timer itself, the JS Engine hands the callback function `cb` and the timer (5000ms) to the **Web API** environment.
    *   The Web API starts a 5-second timer.
    *   `setTimeout` finishes its job in the Call Stack and is popped off. The JS Engine moves on.
4.  **`console.log("End")`:** This is executed by the Call Stack. "End" is printed to the console. The function is popped.
5.  **GEC Popped:** All synchronous code is done. The GEC is popped from the Call Stack. The Call Stack is now **empty**.
6.  **Timer Finishes:** After 5 seconds, the `setTimeout` timer in the Web API environment completes.
7.  **Callback to Queue:** The Web API pushes the callback function `cb` into the **Callback Queue**.
8.  **Event Loop in Action:** The Event Loop sees that the **Call Stack is empty** and there is a task in the **Callback Queue**.
9.  **Callback to Stack:** The Event Loop takes `cb` from the queue and pushes it onto the **Call Stack**.
10. **Callback Execution:** The Call Stack executes `cb`. `console.log("Callback")` is run, and "Callback" is printed to the console.
11. **Finish:** `cb` is popped from the stack. The program is complete.

---

## 🔄 The Role of the Event Loop

The Event Loop is the fundamental piece that enables asynchronous behavior in a synchronous language.
*   It acts as a gatekeeper between the Callback Queue and the Call Stack.
*   It ensures that asynchronous callbacks do not interrupt the main thread of execution. They have to wait their turn until the Call Stack is free.

> ⚠️ **`setTimeout(..., 0)` does not mean instant execution!**
> Even with a timeout of 0ms, the callback function still goes through the Web API and into the Callback Queue. It will only be executed after all the synchronous code in the main script has finished and the Call Stack is empty.

---

## 🌐 Other Asynchronous Web APIs

*   **`addEventListener`**: When you attach a listener like `document.getElementById('myBtn').addEventListener('click', function cb() {...})`, you are registering a callback.
    *   The browser's Web API keeps track of this event.
    *   When the user clicks the button, the Web API pushes the callback `cb` into the Callback Queue.
    *   The Event Loop then moves it to the Call Stack when it's empty.
*   **`fetch`**: When you make a network request with `fetch`, it returns a promise. The response handling (`.then()`) also involves asynchronous callbacks that are managed by the runtime environment (specifically, the Microtask Queue).

---

## ✨ Summary

1.  JavaScript is synchronous and single-threaded at its core (in the JS Engine).
2.  Asynchronous behavior is handled by the **Browser Runtime Environment**, which includes Web APIs, the Callback Queue, and the Event Loop.
3.  When an async function like `setTimeout` is called, it is handed off to the **Web API** environment, and the JS Engine continues executing synchronous code.
4.  Once the async task is complete, its callback is placed in the **Callback Queue**.
5.  The **Event Loop** constantly checks if the **Call Stack** is empty. If it is, it moves the first callback from the queue to the stack for execution.
6.  This mechanism prevents blocking the main thread and allows for a responsive user interface.
