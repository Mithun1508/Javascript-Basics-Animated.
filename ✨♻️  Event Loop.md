JavaScript is single-threaded: only one task can run at a time. Usually that’s no big deal, but now imagine you’re running a task which takes 30 seconds.. Ya.. During that task we’re waiting for 30 seconds before anything else can happen (JavaScript runs on the browser’s main thread by default, so the entire UI is stuck) 😬 It’s 2019, no one wants a slow, unresponsive website.

Luckily, the browser gives us some features that the JavaScript engine itself doesn’t provide: a Web API. This includes the DOM API, setTimeout, HTTP requests, and so on. This can help us create some async, non-blocking behavior 🚀

  1) When we invoke a function, it gets added to something called the call stack. The call stack is part of the JS engine, this isn’t browser specific. It’s a stack, meaning that it’s first in, last out (think of a pile of pancakes). When a function returns a value, it gets popped off the stack 👋

   ![1](https://user-images.githubusercontent.com/93249038/210689043-70692378-b7f5-4e50-94bb-18cbfcffd074.jpg)

  2) The respond function returns a setTimeout function. The setTimeout is provided to us by the Web API: it lets us delay tasks without blocking the main thread. The callback function that we passed to the setTimeout function, the arrow function () => { return 'Hey' } gets added to the Web API. In the meantime, the setTimeout function and the respond function get popped off the stack, they both returned their values!

   ![2](https://user-images.githubusercontent.com/93249038/210689153-cefb7d78-faf1-414c-867d-5b7940689d6e.jpg)   

  3) In the Web API, a timer runs for as long as the second argument we passed to it, 1000ms. The callback doesn’t immediately get added to the call stack, instead it’s passed to something called the queue.
       
       
![3](https://user-images.githubusercontent.com/93249038/210690292-b8aeb1d0-e132-49c8-9ef6-e7a32394b7b1.jpg)


     4) This can be a confusing part: it doesn't mean that the callback function gets added to the callstack(thus returns a value) after 1000ms! It simply gets added to the queue after 1000ms. But it’s a queue, the function has got to wait for its turn!  Now this is the part we’ve all been waiting for… Time for the event loop to do its only task: connecting the queue with the call stack! If the call stack is empty, so if all previously invoked functions have returned their values and have been popped off the stack, the first item in the queue gets added to the call stack. In this case, no other functions were invoked, meaning that the call stack was empty by the time the callback function was the first item in the queue.
    

     ![4](https://user-images.githubusercontent.com/93249038/210691041-e0856bf6-7813-4b0d-b821-2401702874e2.jpg)

     5) The callback is added to the call stack, gets invoked, and returns a value, and gets popped off the stack.
     
    
![5](https://user-images.githubusercontent.com/93249038/210690679-1641c905-1f46-4597-b868-1f8aed4db220.jpg)

      
      Let's see what if we run the below code on the browser:
      
      const foo = () => console.log("First");
      const bar = () => setTimeout(() => console.log("Second"), 500);
      const baz = () => console.log("Third");
      bar();
      foo();
      baz();
     
     
![6](https://user-images.githubusercontent.com/93249038/210690704-700a5ab8-1ea3-4189-867d-6f1caf8c45d6.jpg)
    # STEPS:
    1) We invoke bar. bar returns a setTimeout function.
    2) The callback we passed to setTimeout gets added to the Web API, the setTimeout function and bar get popped off the callstack.
    3) The timer runs, in the meantime foo gets invoked and logs First. foo returns (undefined),baz gets invoked, and the callback gets added to the queue.
baz logs Third. 
    4) The event loop sees the callstack is empty after baz returned, after which the callback gets added to the call stack.
    5) The callback logs Second.


