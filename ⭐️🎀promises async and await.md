
![promises ](https://user-images.githubusercontent.com/93249038/210706123-f5853f6d-b1ac-4334-89fb-bdb914aa0b33.jpg)
When writing JavaScript, we often have to deal with tasks that rely on other tasks! Let's say that we want to get an image, compress it, apply a filter, and save it 📸

The very first thing we need to do, is get the image that we want to edit. A getImage function can take care of this! Only once that image has been loaded successfully, we can pass that value to a resizeImage function. When the image has been resized successfully, we want to apply a filter to the image in the applyFilter function. After the image has been compressed and we've added a filter, we want to save the image and let the user know that everything worked correctly! 🥳

In the end, we'll end up with something like this:
![getiMAGE](https://user-images.githubusercontent.com/93249038/210706189-7d871969-4003-4fae-b8c0-e17e16ce99b3.jpg)
Hmm... Notice anything here? Although it's... fine, it's not great. We end up with many nested callback functions that are dependent on the previous callback function. This is often referred to as a callback hell, as we end up with tons of nested callback functions that make the code quite difficult to read!

Luckily, we now got something called promises to help us out! Let's take a look at what promises are, and how they can help us in situations like these! 😃

Promise Syntax
ES6 introduced Promises. In many tutorials, you'll read something like:

"A promise is a placeholder for a value that can either resolve or reject at some time in the future"

Yeah... That explanation never made things clearer for me. In fact it only made me feel like a Promise was a weird, vague, unpredictable piece of magic. So let's look at what promises really are.

We can create a promise, using a Promise constructor that receives a callback. Okay cool, let's try it out!

![new promise](https://user-images.githubusercontent.com/93249038/210706291-b3348bdb-0cbb-431f-bbed-29fe67b1b911.jpg)
A Promise is an object that contains a status, ([[PromiseStatus]]) and a value ([[PromiseValue]]). In the above example, you can see that the value of [[PromiseStatus]] is "pending", and the value of the promise is undefined.

Don't worry - you'll never have to interact with this object, you can't even access the [[PromiseStatus]] and [[PromiseValue]] properties! However, the values of these properties are important when working with promises.

The value of the PromiseStatus, the state, can be one of three values:

✅ fulfilled: The promise has been resolved. Everything went fine, no errors occurred within the promise 🥳
❌ rejected : The promise has been rejected. Argh, something went wrong..
⏳ pending: The promise has neither resolved nor rejected (yet), the promise is still pending.
Alright this all sounds great, but when is a promise status "pending", "fulfilled" or "rejected"? And why does that status even matter?

In the above example, we just passed the simple callback function () => {} to the Promise constructor. However, this callback function actually receives two arguments. The value of the first argument, often called resolve or res, is the method to be called when the Promise should resolve. The value of the second argument, often called reject or rej, is the value method to be called when the Promise should reject, something went wrong.
![reolve rej](https://user-images.githubusercontent.com/93249038/210706374-de66e58a-1877-4bf4-b45e-f12709c5c63b.jpg)
Let's try and see that gets logged when we invoke either the resolve or reject method! In my example, I called the resolve method res, and the reject method rej.


![undefined ](https://user-images.githubusercontent.com/93249038/210706460-4251c0e0-7fb7-4dc6-97b0-fd5db41fdeca.jpg)

Awesome! We finally know how to get rid of the "pending" status and the undefined value! The status of a promise is "fulfilled" if we invoked the resolve method, and the status of the promise is "rejected" if we invoked the rejected method.

The value of a promise, the value of [[PromiseValue]], is the value that we pass to the either the resolved or rejected method as their argument.

Fun fact, I let Jake Archibald proofread this article and he actually pointed out there's a bug in Chrome that currently shows the status as "resolved" instead of "fulfilled". Thanks to Mathias Bynens it's now fixed in Canary! 🥳🕺🏼

Okay so, now we know a little bit better how to control that vague Promise object. But what is it used for?

In the introductory section, I showed an example in which we get an image, compress it, apply a filer, and save it! Eventually, this ended up being a nested callback mess.

Luckily, Promises can help us fix this! First, let's rewrite the entire code block, so that each function returns a Promise instead.

If the image is loaded and everything went fine, let's resolve the promise with the loaded image! Else, if there was an error somewhere while loading the file, let's reject the promise with the error that occurred.
![getiMAGE](https://user-images.githubusercontent.com/93249038/210706555-2e04ed57-9fa0-44c7-b9bf-6810b1ec4a3d.jpg)

Cool! A promise got returned with the value of the parsed data, just like we expected.

But... what now? We don't care about that entire promise object, we only care about the value of the data! Luckily, there are built-in methods to get a promise's value. To a promise, we can attach 3 methods:

.then(): Gets called after a promise resolved.
.catch(): Gets called after a promise rejected.
.finally(): Always gets called, whether the promise resolved or rejected.
![getImaag](https://user-images.githubusercontent.com/93249038/210706607-dd0601b9-1c93-4424-853e-7f9a75ae8380.jpg)

The .catch method receives the value passed to the rejected method
![catch](https://user-images.githubusercontent.com/93249038/210706659-19d209cd-5d05-4f4a-8dba-7a53af2c5613.jpg)

Finally, we have the value that got resolved by the promise without having that entire promise object! We can now do whatever we want with this value.

FYI, when you know that a promise will always resolve or always reject, you can write Promise.resolve or Promise.reject , with the value you want to reject or resolve the promise with!

![reolve rej](https://user-images.githubusercontent.com/93249038/210706790-67965494-106c-4ffd-b2dc-518e7a09840a.jpg)

In the case of the getImage example, we can chain multiple then callbacks in order to pass the processed image onto the next function! Instead of ending up with many nested callbacks, we get a clean then chain.

Microtasks and (Macro)tasks
Okay so we know a little better how to create a promise and how to extract values out of a promise. Let's add some more code to the script, and run it again:

![microtasks](https://user-images.githubusercontent.com/93249038/210706873-99b3f397-01c2-40a1-abe0-7ddcabf06a60.jpg)
First, Start! got logged. Okay we could've seen that one coming: console.log('Start!') is on the very first line! However, the second value that got logged was End!, and not the value of the resolved promise! Only after End! was logged, the value of the promise got logged. What's going on here?

We've finally seen the true power of promises! 🚀 Although JavaScript is single-threaded, we can add asynchronous behavior using a Promise!

But wait, haven't we seen that before? 🤔 In the JavaScript event loop, can't we also use methods native to the browser such as setTimeout to create some sort of asynchronous behavior?

Yes! However, within the Event Loop, there are actually two types of queues: the (macro)task queue (or just called the task queue), and the microtask queue. The (macro)task queue is for (macro)tasks and the microtask queue is for microtasks.

So what's a (macro)task and what's a microtask? Although there are a few more than I'll cover here, the most common are shown in the table below!

(Macro)task	setTimeout | setInterval | setImmediate
Microtask	process.nextTick | Promise callback | queueMicrotask
Ahh, we see Promise in the microtask list! 😃 When a Promise resolves and calls its then(), catch() or finally(), method, the callback within the method gets added to the microtask queue! This means that the callback within the then(), catch() or finally() method isn't executed immediately, essentially adding some async behavior to our JavaScript code!

So when is a then(), catch() or finally() callback executed? The event loop gives a different priority to the tasks:

All functions in that are currently in the call stack get executed. When they returned a value, they get popped off the stack.
When the call stack is empty, all queued up microtasks are popped onto the callstack one by one, and get executed! (Microtasks themselves can also return new microtasks, effectively creating an infinite microtask loop 😬)
If both the call stack and microtask queue are empty, the event loop checks if there are tasks left on the (macro)task queue. The tasks get popped onto the callstack, executed, and popped off!
Let's take a look at a quick example, simply using:

Task1: a function that's added to the call stack immediately, for example by invoking it instantly in our code.
Task2, Task3, Task4: microtasks, for example a promise then callback, or a task added with queueMicrotask.
Task5, Task6: a (macro)task, for example a setTimeout or setImmediate callback

![task](https://user-images.githubusercontent.com/93249038/210706931-b0e15910-ebcf-4ae6-8097-3fd7ade4c55a.jpg)

First, Task1 returned a value and got popped off the call stack. Then, the engine checked for tasks queued in the microtask queue. Once all the tasks were put on the call stack and eventually popped off, the engine checked for tasks on the (macro)task queue, which got popped onto the call stack, and popped off when they returned a value.

Okay okay enough pink boxes. Let's use it with some real code!
![settime](https://user-images.githubusercontent.com/93249038/210706986-c69b291e-d172-4699-a918-3bc2a99040e8.jpg)

First, Task1 returned a value and got popped off the call stack. Then, the engine checked for tasks queued in the microtask queue. Once all the tasks were put on the call stack and eventually popped off, the engine checked for tasks on the (macro)task queue, which got popped onto the call stack, and popped off when they returned a value.

Okay okay enough pink boxes. Let's use it with some real code!
![callstack web ](https://user-images.githubusercontent.com/93249038/210707032-aace9297-09f9-468e-8ab5-4728daf94535.jpg)


The engine encounters the setTimeout method, which gets popped on to the call stack. The setTimeout method is native to the browser: its callback function (() => console.log('In timeout')) will get added to the Web API, until the timer is done. Although we provided the value 0 for the timer, the call back still gets pushed to the Web API first, after which it gets added to the (macro)task queue: setTimeout is a macro task!

![evenrtlopp](https://user-images.githubusercontent.com/93249038/210707096-623c360f-6346-47db-865f-31bca32758ba.jpg)
The engine encounters the Promise.resolve() method. The Promise.resolve() method gets added to the call stack, after which is resolves with the value Promise!. Its then callback function gets added to the microtask queue.

![calls tack](https://user-images.githubusercontent.com/93249038/210707181-adf85ffa-cb88-4e43-b36a-87562a6c8b34.jpg)
The engine encounters the console.log() method. It gets added to the call stack immediately, after which it logs the value End! to the console, gets popped off the call stack, and the engine continues.

![call stack2](https://user-images.githubusercontent.com/93249038/210707245-659a2bce-5aee-4a4d-9a77-377a750641ab.jpg)
The engine sees the callstack is empty now. Since the call stack is empty, it's going to check whether there are queued tasks in the microtask queue! And yes there are, the promise then callback is waiting for its turn! It gets popped onto the call stack, after which it logs the resolved value of the promise: the string Promise!in this case.
![res](https://user-images.githubusercontent.com/93249038/210707298-4f41c9c2-e0c3-45cc-96e1-857750a391f3.jpg)
It's time to check the (macro)task queue: the setTimeout callback is still waiting there! The setTimeout callback gets popped on to the callstack. The callback function returns the console.log method, which logs the string "In timeout!". The setTimeout callback get popped off the callstack.

![cll stack](https://user-images.githubusercontent.com/93249038/210707373-a75a5c23-5dcc-4a52-953d-a0aa1e58e24a.jpg)

Async/Await
ES7 introduced a new way to add async behavior in JavaScript and make working with promises easier! With the introduction of the async and await keywords, we can create async functions which implicitly return a promise. But.. how can we do that? 😮

Previously, we saw that we can explicitly create promises using the Promise object, whether it was by typing new Promise(() => {}), Promise.resolve, or Promise.reject.

Instead of explicitly using the Promise object, we can now create asynchronous functions that implicitly return an object! This means that we no longer have to write any Promise object ourselves.


![async await](https://user-images.githubusercontent.com/93249038/210707464-175c0a9d-e755-4254-91f9-c10b8afc0188.jpg)

Although the fact that async functions implicitly return promises is pretty great, the real power of async functions can be seen when using the await keyword! With the await keyword, we can suspend the asynchronous function while we wait for the awaited value return a resolved promise. If we want to get the value of this resolved promise, like we previously did with the then() callback, we can assign variables to the awaited promise value!

So, we can suspend an async function? Okay great but.. what does that even mean?

Let's see what happens when we run the following block of code:
![const one](https://user-images.githubusercontent.com/93249038/210707518-43c47f87-a9b2-4885-8c08-9d2d5c55fd30.jpg)
Hmm, what' happening here?..

![wts happeing](https://user-images.githubusercontent.com/93249038/210707616-0ece65e5-8c31-467b-a811-8982b378c5e5.jpg)


![micro](https://user-images.githubusercontent.com/93249038/210707664-8938d6f6-b02a-42c8-a2bc-66f95ee2664c.jpg)
First, the engine encounters a console.log. It gets popped onto the call stack, after which Before function! gets logged.


![event loop](https://user-images.githubusercontent.com/93249038/210707727-537b9b68-c311-47e8-9d5f-6d3e22e9d4cb.jpg)
Then, we invoke the async function myFunc(), after which the function body of myFunc runs. On the very first line within the function body, we call another console.log, this time with the string In function!. The console.log gets added to the call stack, logs the value, and gets popped off.
![my func](https://user-images.githubusercontent.com/93249038/210707793-78749bb0-55e6-491f-bb66-ce390e8e7de0.jpg)
The function body keeps on being executed, which gets us to the second line. Finally, we see an await keyword! 🎉

The first thing that happens is that the value that gets awaited gets executed: the function one in this case. It gets popped onto the call stack, and eventually returns a resolved promise. Once the promise has resolved and one returned a value, the engine encounters the await keyword.

When encountering an await keyword, the async function gets suspended. ✋🏼 The execution of the function body gets paused, and the rest of the async function gets run in a microtask instead of a regular 


![async ](https://user-images.githubusercontent.com/93249038/210707865-239ddea9-8f38-4e7a-852c-06a767da6142.jpg)
Now that the async function myFunc is suspended as it encountered the await keyword, the engine jumps out of the async function and continues executing the code in the execution context in which the async function got called: the global execution context in this case! 🏃🏽‍♀️

![Uploading async 2 .jpg…]()


Finally, there are no more tasks to run in the global execution context! The event loop checks to see if there are any microtasks queued up: and there are! The async myFunc function is queued up after resolving the valued of one. myFunc gets popped back onto the call stack, and continues running where it previously left off.

The variable res finally gets its value, namely the value of the resolved promise that one returned! We invoke console.log with the value of res: the string One! in this case. One! gets logged to the console and gets popped off the call stack! 😊

Finally, all done! Did you notice how async functions are different compared to a promise then? The await keyword suspends the async function, whereas the Promise body would've kept on being executed if we would've used then!
