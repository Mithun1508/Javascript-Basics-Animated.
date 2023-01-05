
![gen](https://user-images.githubusercontent.com/93249038/210703954-f5a777d8-75a1-40d0-9a53-861962bfd923.jpg)
ES6 introduced something cool called generator functions 🎉 Whenever I ask people about generator functions, the responses are basically: "I've seem them once, got confused, never looked at it again", "oh gosh no I've read so many blog posts about generator functions and I still don't get them", "I get them but why would anyone ever use that" 🤔 Or maybe that's just the conversations I've been having with myself because that's how I used to think for a long time! But they're actually quite cool.

So, what are generator functions? Let's first just look at a regular, old-fashioned function 👵🏼
![normal fn](https://user-images.githubusercontent.com/93249038/210704029-397d007c-e2a1-473f-976a-54d1d53d5365.jpg)

Yep absolutely nothing special about this! It's just a normal function that logs a value 4 times. Let's invoke it


![invoke](https://user-images.githubusercontent.com/93249038/210704116-eed5faf9-80eb-47a7-b969-29966337c01d.jpg)
"But Lydia why did you just waste 5 seconds of my life by making me look at this normal boring function", a very good question. Normal functions follow something called a run-to-completion model: when we invoke a function, it will always run until it completes (well, unless there's an error somewhere). We can't just randomly pause a function somewhere in the middle whenever we want to.

Now here comes the cool part: generator functions don't follow the run-to-completion model! 🤯 Does this mean that we can randomly pause a generator function in the middle of executing it? Well, sort of! Let's take a look at what generator functions are and how we can use them.

We create a generator function by writing an asterisk * after the function keyword.

![gen fn](https://user-images.githubusercontent.com/93249038/210704171-70fee067-b79d-4272-8882-18aa08df3067.jpg)
But that's not all we have to do to use generator functions! Generator functions actually work in a completely different way compared to regular functions:

Invoking a generator function returns a generator object, which is an iterator.
We can use the yield keyword in a generator function to "pause" the execution.
But what does that even mean!?

Let's first go over the first one: Invoking a generator function returns a generator object. When we invoke a regular function, the function body gets executed and eventually returns a value. However when we invoke a generator function, a generator object gets returned! Let's see what that looks like when we log the returned value.
![fn gen fn ](https://user-images.githubusercontent.com/93249038/210704216-4ab2f590-dd54-4f48-8bfc-28eb6a5c8bb5.jpg)
Now, I can hear you screaming internally (or externally 🙃) because this can look a little overwhelming. But don't worry, we don't really have to use any of the properties you see logged here. So what's the generator object good for then?

First we need to take a small step back, and answer the second difference between regular functions and generator functions: We can use the yield keyword in a generator function to "pause" the execution.

With generator functions, we can write something like this (genFunc is short for generatorFunction):
![grn func](https://user-images.githubusercontent.com/93249038/210704271-40db7736-6285-4f6e-94c7-aad10037a5b0.jpg)

What's that yield keyword doing there? The execution of the generator gets "paused" when it encounters a yield keyword. And the best thing is that the next time we run the function, it remembered where it previously paused, and runs from there on! 😃 Basically what's happening here (don't worry this will be animated later on):

The first time it runs, it "pauses" on the first line and yields the string value '✨'
The second time it runs, it starts on the line of the previous yield keyword. It then runs all the way down till the second yield keyword and yields the value '💕'.
The third time it runs, it start on the line of the previous yield keyword. It runs all the way down until it encounters the return keyword, and returns the value 'Done!'.
But... how can we invoke the function if we previously saw that invoking the generator function returned a generator object? 🤔 This is where the generator object comes into play!

The generator object contains a next method (on the prototype chain). This method is what we'll use to iterate the generator object. However, in order to remember the state of where it previously left off after yielding a value, we need to assign the generator object to a variable. I'll call it genObj short for generatorObject.


![yield](https://user-images.githubusercontent.com/93249038/210704333-7ce1f574-9f5f-4654-afeb-9bd865578062.jpg)
Yep, the same scary looking object as we saw before. Let's see what happens when we invoke the next method on the genObj generator object!

![scary](https://user-images.githubusercontent.com/93249038/210704417-4416e66e-2712-4cfd-96f0-fd5255f4961e.jpg)

The generator ran until it encountered the first yield keyword, which happened to be on the first line! It yielded an object containing a value property, and a done property.
{ value: ... , done: ... }
The value property is equal to the value that we yielded.
The done property is a boolean value, which is only set to true once the generator function returned a value (not yielded! 😊).

We stopped iterating over the generator, which makes it look like the function just paused! How cool is that. Let's invoke the next method again! 😃
![genobjnext](https://user-images.githubusercontent.com/93249038/210704477-c1747008-f31c-4d6a-9e6b-399e6d0a92b0.jpg)
First, we logged the string First log! to the console. This is neither a yield nor return keyword, so it continues! Then, it encountered a yield keyword with the value '💕'. An object gets yielded with the value property of '💕' and a done property. The value of the done property is false, since we haven't returned from the generator yet.

We're almost there! Let's invoke next for the last time.
![yielddionefalse](https://user-images.githubusercontent.com/93249038/210704546-fe396995-362d-4882-89c0-52936a485856.jpg)
We logged the string Second log! to the console. Then, it encountered a return keyword with the value 'Done!'. An object gets returned with the value property of 'Done!'. We actually returned this time, so the value of done is set to true!

The done property is actually very important. We can only iterate a generator object once. What?! So what happens when we call the next method again?
![yiled yield yield](https://user-images.githubusercontent.com/93249038/210704599-a9368c60-bd6a-4435-9ddf-a94c5ab88e33.jpg)
![yie yield yie](https://user-images.githubusercontent.com/93249038/210704650-7b8cb2dc-705f-4eaf-aa86-2ccf5e1592eb.jpg)
But what makes an iterator an iterator? Because we can also use for-of loops and the spread syntax with arrays, strings, maps, and sets. It's actually because they implement the iterator protocol: the [Symbol.iterator]. Say that we have the following values (with very descriptive names lol 💁🏼‍♀️):
![arry string obj](https://user-images.githubusercontent.com/93249038/210704794-09e40186-5885-4ebc-b792-98f856c7c69c.jpg)

The array, string, and generatorObject are all iterators! Let's take a look at the value of their [Symbol.iterator] property.
![arry string obj](https://user-images.githubusercontent.com/93249038/210704875-ac649382-8817-441d-94bc-90f8fef6478e.jpg)
But then what's the value of the [Symbol.iterator] on the values that aren't iterable?

![regularfn](https://user-images.githubusercontent.com/93249038/210704913-60880c02-f93a-44ff-98d4-9ff7c28f3cdf.jpg)

Yeah, it's just not there. So.. Can we simply just add the [Symbol.iterator] property manually, and make non-iterables iterable? Yes, we can! 😃

[Symbol.iterator] has to return an iterator, containing a next method which returns an object just like we saw before: { value: '...', done: false/true }.

To keep it simple (as lazy me likes to do) we can simply set the value of [Symbol.iterator] equal to a generator function, as this returns an iterator by default. Let's make the object an iterable, and the yielded value the entire object:

![yieldthuis](https://user-images.githubusercontent.com/93249038/210704974-b18f8f66-1dd8-4a08-8436-f695e7eb8755.jpg)

![for let](https://user-images.githubusercontent.com/93249038/210705077-6e63c05e-c52e-4c02-aefc-9d6786d3eaf8.jpg)
Lets try that 

![yty that](https://user-images.githubusercontent.com/93249038/210705132-4e861207-729a-4011-b2c1-c63f0f9c9596.jpg)
Oh shoot. Object.keys(this) is an array, so the value that got yielded is an array. Then we spread this yielded array into another array, resulting in a nested array. We didn't want this, we just wanted to yield each individual key!

Good news! 🥳 We can yield individual values from iterators within a generator using the yield* keyword, so the yield with an asterisk! Say that we have a generator function that first yield an avocado, then we want to yield the values of another iterator (an array in this case) individually. We can do so with the yield* keyword. We then delegate to another generator!
![emojis](https://user-images.githubusercontent.com/93249038/210705197-be978d3b-b31e-4e41-a5ad-2b1d68e8c460.jpg)

Each value of the delegated generator gets yielded, before it continued iterating the genObj iterator.

This is exactly what we need to do in order to get all object keys individually!

![obhject](https://user-images.githubusercontent.com/93249038/210705235-1a7a7b4f-06e6-4bec-b89f-e0fe7122ee7f.jpg)
Another use of generator functions, is that we can (sort of) use them as observer functions. A generator can wait for incoming data, and only if that data is passed, it will process it. An example:


![genfn](https://user-images.githubusercontent.com/93249038/210705285-baac65b6-1e22-4c3f-b0df-c9b245a325b4.jpg)
A big difference here is that we don't just have yield [value] like we saw in the previous examples. Instead, we assign a value called second, and yield value the string First!. This is the value that will get yielded the first time we call the next method.

Let's see what happens when we call the next method for the first time on the iterable.
![genobjnext](https://user-images.githubusercontent.com/93249038/210705332-1da8c90a-7472-486f-bd3c-ea944199e520.jpg)
It encountered the yield on the first line, and yielded the value First!. So, what's the value of the variable second?

That's actually the value that we pass to the next method the next time we call it! This time, let's pass the string 'I like JavaScript'.
![genobj](https://user-images.githubusercontent.com/93249038/210705389-51fd06d3-0c41-4e08-952e-8667b4ebb93b.jpg)

It's important to see here that the first invocation of the next method doesn't keep track of any input yet. We simply start the observer by invoking it the first time. The generator waits for our input, before it continues, and possibly processes the value that we pass to the next method.

So why would you ever want to use generator functions?

One of the biggest advantages of generators is the fact that they are lazily evaluated. This means that the value that gets returned after invoking the next method, is only computed after we specifically asked for it! Normal functions don't have this: all the values are generated for you in case you need to use it some time in the future.
![calculating](https://user-images.githubusercontent.com/93249038/210705441-52559682-c583-4c10-a7f7-0012bdf09fcb.jpg)
There are several other use cases, but I usually like to do it to have way more control when I'm iterating large datasets!

Imagine we have a list of book clubs! 📚 To keep this example short and not one huge block of code, each book club just has one member. A member is currently reading several books, which is represented in the books array!


![bookclubs](https://user-images.githubusercontent.com/93249038/210705482-8c7f2c9f-11d6-4a39-a630-751c029801c1.jpg)
Now, we're looking for a book with the id ey812. In order to find that, we could potentially just use a nested for-loop or a forEach helper, but that means that we'd still be iterating through the data even after finding the team member we were looking for!

The awesome thing about generators, is that it doesn't keep on running unless we tell it to. This means that we can evaluate each returned item, and if it's the item we're looking for, we simply don't call next! Let's see what that would look like.

First, let's create a generator that iterates through the books array of each team member. We'll pass the team member's book array to the function, iterate through the array, and yield each book!

![books](https://user-images.githubusercontent.com/93249038/210705523-f57df493-b7e5-498b-b8b7-4f5cfe287c5d.jpg)
Perfect! Now we have to make a generator that iterates through the clubMembers array. We don't really care about the club member itself, we just need to iterate through their books. In the iterateMembers generator, let's delegate the iterateBooks iterator in order to just yield their books!
![books](https://user-images.githubusercontent.com/93249038/210705571-2f5cdafa-ee09-408f-ad4d-08c1c1b63827.jpg)
Almost there! The last step is to iterate through the bookclubs. Just like in the previous example, we don't really care about the bookclubs themselves, we just care about the club members (and especially their books). Let's delegate the iterateClubMembers iterator and pass the clubMembers array to it.


![bookclubs](https://user-images.githubusercontent.com/93249038/210705621-67875a6f-1654-43be-8dcc-f2ce6d93152c.jpg)

In order to iterate through all this, we need to get the generator object iterable by passing the bookClub array to the iterateBookClubs generator. I'll just call the generator object it for now, for iterator.
![booksxclub](https://user-images.githubusercontent.com/93249038/210705670-305cb87f-e2a0-499e-96d9-d7ad93b2f376.jpg)
Let's invoke the next method, until we get a book with the id ey812.
![next](https://user-images.githubusercontent.com/93249038/210705710-13a0a52a-2e49-42ee-ae28-6be9b644e017.jpg)

Nice! We didn't have to iterate through all the data in order to get the book we were looking for. Instead, we just looked for the data on demand! of course, calling the next method manually each time isn't very efficient... So let's make a function instead!

Let's pass an id to the function, which is the id of the book we're looking for. If the value.id is the id we're looking for, then simply return the entire value (the book object). Else, if it's not the correct id, invoke next again!

![findbook](https://user-images.githubusercontent.com/93249038/210705805-7915e1e2-9882-4e77-9673-6af31f78d482.jpg)
