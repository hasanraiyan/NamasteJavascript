# Lecture 15: JS Event Loop, Callback Queue & Microtask Queue

*   **Video Author**: Akshay Saini
*   **Notes generated using**: Gemini 2.5 Pro by Raiyan Hasan

---

## 📋 Table of Contents

1.  [Recap: The Browser Runtime Environment](#-recap-the-browser-runtime-environment)
2.  [Introducing the Microtask Queue](#-introducing-the-microtask-queue)
    *   [What is the Microtask Queue?](#what-is-the-microtask-queue)
    *   [What goes into the Microtask Queue?](#what-goes-into-the-microtask-queue)
    *   [What goes into the Callback Queue?](#what-goes-into-the-callback-queue)
3.  [The Priority Rule: Microtask vs. Callback Queue](#-the-priority-rule-microtask-vs-callback-queue)
4.  [Code Example Walkthrough](#-code-example-walkthrough)
    *   [Execution Analysis](#execution-analysis)
5.  [Starvation of the Callback Queue](#-starvation-of-the-callback-queue)
6.  [Summary: The Complete Picture](#-summary-the-complete-picture)

---

## 🔄 Recap: The Browser Runtime Environment

As we saw in the previous lecture, JavaScript's asynchronous capabilities are provided by the browser's runtime environment. The key components are:
*   **JS Engine (with Call Stack)**: Executes synchronous code.
*   **Web APIs**: Handles async tasks like `setTimeout`, DOM events (`click`), and network calls (`fetch`).
*   **Callback Queue (Task Queue)**: Holds callback functions that are ready to be executed.
*   **Event Loop**: The gatekeeper that moves callbacks from the queue to the Call Stack when the stack is empty.

However, this picture is incomplete. There is another crucial queue that plays a major role.

---

## ⚡ Introducing the Microtask Queue

### What is the Microtask Queue?
The **Microtask Queue** is another queue that holds callback functions, similar to the Callback Queue.

> 💡 **Key Difference:** The Microtask Queue has a **higher priority** than the Callback Queue. Tasks in the Microtask Queue will always be executed before any tasks in the Callback Queue.

### What goes into the Microtask Queue?
All callback functions that come through **Promises** and **MutationObserver** go into the Microtask Queue.

*   Callbacks from `Promise.then()`, `Promise.catch()`, `Promise.finally()`.
*   Callbacks from `MutationObserver` (which is used to detect changes in the DOM).
*   Callbacks from `queueMicrotask()`.

The `fetch` API, which is based on Promises, is a prime example. The callback function inside its `.then()` block is pushed to the Microtask Queue once the network call is complete.

### What goes into the Callback Queue?
This queue is also known as the **Task Queue** or **Macrotask Queue**. It handles all other asynchronous callbacks.

*   `setTimeout()` and `setInterval()` callbacks.
*   DOM event callbacks (e.g., `addEventListener('click', ...)`).
*   I/O operations.

---

## 🥇 The Priority Rule: Microtask vs. Callback Queue

The Event Loop follows a strict order of operations, which is the core of this lecture.

> ⚙️ **Event Loop Execution Order:**
> 1.  It checks the **Microtask Queue**.
> 2.  If there are any tasks in the Microtask Queue, it will execute **ALL** of them one by one until the queue is empty.
> 3.  Only when the Microtask Queue is completely empty will it pick **ONE** task from the **Callback Queue** and push it to the Call Stack.
> 4.  After that one task from the Callback Queue is executed, the Event Loop goes back to Step 1 and checks the Microtask Queue again.

---

## 💻 Code Example Walkthrough

Let's see this priority in action with a detailed example.

```js
console.log("Start");

// Registers a callback in the Web API environment, will enter Callback Queue
setTimeout(function cbT() {
  console.log("CB SetTimeout");
}, 5000);

// Fetch returns a promise. The callback will enter Microtask Queue
fetch("https://api.netflix.com")
  .then(function cbF() {
    console.log("CB Netflix");
  });

console.log("End");
```

### Execution Analysis
1.  **`console.log("Start")`**: Prints "Start" to the console.
2.  **`setTimeout(cbT, 5000)`**: The browser's Web API starts a 5-second timer. The callback `cbT` is registered.
3.  **`fetch(...)`**: The browser makes a network request. The callback `cbF` is registered with the promise.
4.  **`console.log("End")`**: Prints "End". The main script finishes, and the Global Execution Context is popped from the Call Stack.

**Now, the async part begins:**
*   Let's assume the `fetch` network call is fast and completes in **100ms**.
*   The `fetch` promise is fulfilled, and its callback `cbF` is pushed into the **Microtask Queue**.
*   The `setTimeout` timer is still running.

**Event Loop's Turn:**
1.  The Event Loop checks the Call Stack, which is empty.
2.  It checks the **Microtask Queue**. It finds `cbF`.
3.  `cbF` is pushed to the Call Stack and executed. **"CB Netflix" is logged.**
4.  The Microtask Queue is now empty.

**Later...**
*   At the 5000ms mark, the `setTimeout` timer expires.
*   Its callback `cbT` is pushed into the **Callback Queue**.

**Event Loop's Turn Again:**
1.  The Event Loop checks the Call Stack (empty).
2.  It checks the Microtask Queue (empty).
3.  It checks the **Callback Queue**. It finds `cbT`.
4.  `cbT` is pushed to the Call Stack and executed. **"CB SetTimeout" is logged.**

**Final Output:**
```
Start
End
CB Netflix
CB SetTimeout
```
This proves that the `fetch` callback from the Microtask Queue was executed before the `setTimeout` callback, even though the timer was set much earlier.

---

## 🚫 Starvation of the Callback Queue

Because the Microtask Queue has higher priority, it's possible for the Callback Queue to "starve."

> ⚠️ **Starvation**: A situation where tasks in a lower-priority queue (the Callback Queue) are indefinitely delayed because the higher-priority queue (the Microtask Queue) is never empty.

This can happen if a microtask keeps adding new microtasks to the queue.
```js
// Example of starvation
fetch("...").then(function process() {
  // This microtask creates another microtask
  Promise.resolve().then(process);
});

setTimeout(() => console.log("I will starve!"), 0);
```
In this scenario, the `process` function will be called repeatedly, creating an infinite loop within the Microtask Queue. The Event Loop will never get a chance to check the Callback Queue, and the `setTimeout` callback will never run.

---

## ✨ Summary: The Complete Picture

1.  **JavaScript Runtime** consists of the JS Engine, Web APIs, a **Microtask Queue**, and a **Callback (Macrotask) Queue**.
2.  The **Event Loop** orchestrates everything.
3.  **Microtask Queue** has higher priority. It includes callbacks from Promises (`fetch`) and MutationObserver.
4.  **Callback Queue** has lower priority. It includes callbacks from `setTimeout`, DOM events, etc.
5.  The Event Loop will execute **all** tasks in the Microtask Queue before picking just **one** task from the Callback Queue.
6.  This high priority can lead to the **starvation** of the Callback Queue if microtasks are continuously generated.
