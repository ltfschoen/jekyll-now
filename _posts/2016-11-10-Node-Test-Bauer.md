---
layout: post
title: Node.js Test Bauer (in progress!)
---

# Table of Contents
  * [Chapter 1 - Node.js Test Bauer](#chapter-1)

## Chapter 1 - Node.js Test Bauer<a id="chapter-1"></a>

### Links

* Node.js app http://blog.modulus.io/absolute-beginners-guide-to-nodejs
* Mocha and Chai testing https://www.codementor.io/nodejs/tutorial/unit-testing-nodejs-tdd-mocha-sinon
* JS refresher https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript
* Singleton http://www.dofactory.com/javascript/singleton-design-pattern
* 2D Matrix array JS
- http://stackoverflow.com/questions/7545641/javascript-multidimensional-array
- http://stackoverflow.com/questions/22123531/associative-array-in-javascript-using-a-pair-tuple-as-multi-value-key-index
* Module exports http://stackoverflow.com/questions/5797852/in-node-js-how-do-i-include-functions-from-my-other-files
* Async (Non-Blocking I/O) vs Sync (Blocking I/O) readFile Node.js https://code-maven.com/reading-a-file-with-nodejs
* Iterate JS Array http://www.sebarmeli.com/blog/2010/12/06/best-way-to-loop-through-an-array-in-javascript/
* Custom CLI Node https://www.sitepoint.com/javascript-command-line-interface-cli-node-js/
* Refactoring http://www.integralist.co.uk/posts/refactoring-techniques.html
* Do not transpile Server-side JS to ES6 http://vancelucas.com/blog/dont-transpile-javascript-for-node-js/


### Node.js Notes


#### Node.js Platform
* `libuv` exposed using bindings to `node-core` API (API that **Normalises Non-Blocking I/O across different Operating Systems**)
* V8 JavaScript engine exposed using bindings to `node-core` API (speed and efficient memory management)
* `node-core` JS library implements Node.js API

#### About and Benefits
* Server-side platform
* Single-threaded
* Scalable and performant
* **Queues** replace Mutexes
* **Callbacks Pattern** in Node.js
* **Reactor Pattern** associated with the Node.js Asynchronous single-threaded architecture, non-blocking I/O 
* **Observer Pattern** implemented in Node.js with EventEmitter class with use of **Callbacks** (instead of Threads)
* **Causality** replaces Synchronisation
* Node.js Core functionality and **Module System** leveraging the **NPM package manager** and database of **userland** modules outside the core
* Design patterns, techniques, and practices in coding (proven/recommended) may be used for modularity and efficiency
* **Module System** to organise code allows apps to house multiple versions of same dependency
* **Modules** to create apps and reusable libraries (packages) each with an entry point
* Principle of designing small modules with minimal code and scope (**Single Responsibility Principle**, and **DRY** reusable code, easy to test and maintain and share with browser)
* Avoids **dependency hell and conflicts** since each package is focused and has separate set of dependencies
* Principle of defining modules that expose only single piece of primary functionality (**single entry point**) with secondary features embedded as properties of the exported function or constructor
* Node.js modules internals are created to be used (not extended) to reduce use cases, simplify implementation, and increase usability
* Design implementation and interface must follow **KISS** (ship faster, easily implement and adapt)
* Practical approach with less complexity and easier to maintain is preferred to pure flawless designs

#### I/O
* RAM access speed is fast, bandwidth is fast 
* Disk/Network speed is slow, bandwidth is slow
* CPU-wise I/O not expensive, but delay b/w request and actual operation
* Human-factor-wise waiting for input from user may be slow
* Threads are **expensive** (memory and switching context) and inefficient in terms of system resources so be careful to minimise idle time or not use
* **Traditional Blocking I/O** function calls for I/O request blocked main thread until complete
* **Traditional Blocking I/O** approach on **web servers** is to handle concurrent connections with new threads/processes and reusing from a pool when idle to prevent Blocking I/O on a thread from impacting other requests
* **Non-Blocking I/O mechanism** (operating mode) should avoid the **Polling Pattern (Busy-Waiting)** loop of checking until data is returned (but may waste CPU trying to access resources that is mostly unavailable) when trying to accesss resources. The system calls return immediately and do not wait for read/write of data to complete
* **Synchronous Event Demultiplexing (Event Notification Interface)** should be used for processing concurrent **Non-Blocking I/O** on a single thread (spreading tasks over time instead of threads to minimise idle time) in an **event loop** by queuing I/O events from group of resources watched by the event notifier, and blocking until available to process new I/O events
- **Simple Concurrency Strategy** avoiding **race conditions** and having to synchronise threads 

#### Reactor Pattern
* **Definition** I/O operations handled by blocking until new event available in set of observed sources and reacting by dispatching each to a **Handler**

* Program generates new I/O operations by sending request (**non-blocking call**) to **Event Demuliplexer**
* **Handler (Callback function)** invoked after each I/O operation event completes (i.e. after event produced and processed in **event loop**) and **Event Demultiplexer** pushes new event onto **Event Queue** and **Event Loop** iterates over the queue
* **Handler** returns control to **Event Loop** when execution completes
* **Event Demultiplexer** may receive and insert new asynchronous I/O operations during the handling of existing I/O operations before returning control to **Event Loop**
* **Event Demultiplexer** uses interface on MacOS `kqueue`
- **Unix** regular filesystem files do not support **Non-Blocking I/O** operations so to simulate the behaviour we use separate threads outside the **Event Loop**
* **Normalise Non-Blocking I/O across different Operating Systems** with API of abstracted system calls using Node.js low-level I/O engine C library `libuv` exposed to JavaScript by wrapping a set of bindings (offers Reactor Pattern to create Event Loops, managing Event Queue, and running asynch operations

#### Closures
* Object that combines a function with the environment (local variables and functions in-scope when it was created) 
* **Function Factories**

#### Callback Pattern

* **Handlers** implemented using **Closures** (referencing contxt the function was created in) invoked to propagate outcomes of **asynchronous** operations (since `return` only handles **synchronous** operations)

* Links
- [Closures and Lexical Scoping that remember](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)