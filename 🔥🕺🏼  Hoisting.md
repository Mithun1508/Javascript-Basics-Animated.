Hoisting is one of those terms that every JS dev has heard of because you googled your annoying error and ended up on StackOverflow, where this person told you that this error was caused because of hoisting 🙃 So, what is hoisting? (FYI - scope will be covered in another post, I like to keep posts small and focused)

If you’re new to JavaScript, you may have experienced “weird” behavior where some variables are randomly undefined, ReferenceErrors get thrown, and so on. Hoisting is often explained as putting variables and functions to the top of the file but nah, that’s not what’s happening, although the behavior might seem like it 😃

1)When the JS engine gets our script, the first thing it does is setting up memory for the data in our code. No code is executed at this point, it’s simply just preparing everything for execution. The way that function declarations and variables are stored is different. 
Functions are stored with a reference to the entire function.
![1](https://user-images.githubusercontent.com/93249038/210699217-89abbd15-8063-4552-82e6-d1b125acad9f.jpg)

2)With variables, it’s a bit different. ES6 introduced two new keywords to declare variables: let and const. Variables declared with the let or const keyword are stored uninitialized.

![2](https://user-images.githubusercontent.com/93249038/210699268-05e021ad-482d-4c54-902b-1eca5c72e109.jpg)

3) Variables declared with the var keyword are stored with the default value of undefined.

![3](https://user-images.githubusercontent.com/93249038/210699316-6c14f53a-2e7b-49ab-b000-81bedfb781e4.jpg)

4)Now that the creation phase is done, we can actually execute the code. Let's see what happens if we had 3 console.log statements on top of the file, before we declared the function or any of the variables.

Since functions are stored with a reference to the entire function code, we can invoke them even before the line on which we created them! 🔥
![4](https://user-images.githubusercontent.com/93249038/210699373-0005abb1-4937-4fdd-b450-12ddc21e81c3.jpg)

5) When we reference a variable declared with the var keyword before their declaration, it’ll simply return its default value that it was stored with: undefined! However, this could sometimes lead to "unexpected" behavior. In most cases this means you’re referencing it unintentionally (you probably don’t want it to actually have the value of undefined) 😬
 ![5](https://user-images.githubusercontent.com/93249038/210699436-cf327318-4ba2-4305-a56f-6710a0707fae.jpg)

6) In order to prevent being able to accidentally reference an undefined variable, like we could with the var keyword, a ReferenceError gets thrown whenever we try to access uninitialized variables. The "zone" before their actual declaration, is called the temporal dead zone: you cannot reference the variables (this includes ES6 classes as well!) before their initialization.

![6](https://user-images.githubusercontent.com/93249038/210699468-baa9e489-b422-4d2d-9bda-7706e371b014.jpg)

7) When the engine passes the line on which we actually declared the variables, the values in memory are overwritten with the values we actually declared them with.
 ![7](https://user-images.githubusercontent.com/93249038/210699533-72bcf17b-5746-4a4a-8cb6-fc7f2ff60d92.jpg)
