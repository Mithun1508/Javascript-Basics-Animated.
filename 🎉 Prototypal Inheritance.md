
![prototyp inherit](https://user-images.githubusercontent.com/93249038/210702240-21257350-1969-4cfe-9b4a-d2d92cd84d16.jpg)

Ever wondered why we can use built-in methods such as .length, .split(), .join() on our strings, arrays, or objects? We never explicitly specified them, where do they come from? Now don't say "It's JavaScript lol no one knows, it's magic 🧚🏻‍♂️", it's actually because of something called prototypal inheritance. It's pretty awesome, and you use it more often than you realize!

We often have to create many objects of the same type. Say we have a website where people can browse dogs!

For every dog, we need object that represents that dog! 🐕 Instead of writing a new object each time, I'll use a constructor function (I know what you're thinking, I'll cover ES6 classes later on!) from which we can create Dog instances using the new keyword (this post isn't really about explaining constructor functions though, so I won't talk too much about that).

 Every dog has a name, a breed, a color, and a function to bar

![dog bark ](https://user-images.githubusercontent.com/93249038/210702321-c01a1db1-2408-4b8c-abd7-2ac12e94d6d8.jpg)

1)When we created the Dog constructor function, it wasn't the only object we created. Automatically, we also created another object, called the prototype! By default, this object contains a constructor property, which is simply a reference to the original constructor function, Dog in this case.
![dog cons](https://user-images.githubusercontent.com/93249038/210702376-ce3f61d4-7b8f-4aa0-8077-32ab32ecb8cb.jpg)

The prototype property on the Dog constructor function is non-enumerable, meaning that it doesn't show up when we try to access the objects properties. But it's still there!

Okay so.. Why do we have this property object? First, let's create some dogs that we want to show. To keep it simple, I'll call them dog1 and dog2. dog1 is Daisy, a cute black Labrador! dog2 is Jack, the fearless white Jack Russell 😎

![dog russel](https://user-images.githubusercontent.com/93249038/210702436-ab10f52a-f6a6-493b-8a13-3e7abca31729.jpg)

Let's log dog1 to the console, and expand its properties!
![dog properties](https://user-images.githubusercontent.com/93249038/210702483-f0e50d0e-d0eb-447e-9194-0f827da1b137.jpg)

We see the properties we added, like name, breed, color, and bark.. but woah what is that __proto__ property! It's non-enumerable, meaning that it usually doesn't show up when we try to get the properties on the object. Let's expand it! 😃
![dog properties](https://user-images.githubusercontent.com/93249038/210702517-ac24b685-f0c6-4969-afdb-abd516853207.jpg)

3) Woah it looks exactly like the Dog.prototype object! Well guess what, __proto__ is a reference to the Dog.prototype object. This is what prototypal inheritance is all about: each instance of the constructor has access to the prototype of the constructor! 🤯
![cool](https://user-images.githubusercontent.com/93249038/210702558-b19d0d97-4d2b-45db-888f-42e7e357f8d7.jpg)

4)So why is this cool? Sometimes we have properties that all instances share. For example the bark function in this case: it's the exact same for every instance, why create ![4](https://user-images.githubusercontent.com/93249038/210702588-7df079ae-3625-4343-9090-fc4027b11558.jpg)
a new function each time we create a new dog, consuming memory each time? Instead, we can add it to the Dog.prototype object! 🥳

5) Whenever we try to access a property on the instance, the engine first searches locally to see if the property is defined on the object itself. However, if it can't find the property we're trying to access, the engine walks down the prototype chain through the __proto__ property!
 ![5](https://user-images.githubusercontent.com/93249038/210702685-6fb21bf2-2496-4734-8f60-7a193ad9538b.jpg)

Now this is just one step, but it can contain several steps! If you followed along, you may have noticed that I didn't include one property when I expanded the __proto__ object showing Dog.prototype. Dog.prototype itself is an object, meaning that it's actually an instance of the Object constructor! That means that Dog.prototype also contains a __proto__ property, which is a reference to Object.prototype!
 
![dog1](https://user-images.githubusercontent.com/93249038/210702766-0f772024-e5fe-4fbb-943e-92139ceec188.jpg)

6) Finally, we have an answer to where all the built-in methods come from: they're on the prototype chain! 😃

For example the .toString() method. Is it defined locally on the dog1 object? Hmm no.. Is it defined on the object dog1.__proto__ has a reference to, namely Dog.prototype? Also no! Is it defined on the object Dog.prototype.__proto__ has a reference to, namely Object.prototype? Yes! 🙌🏼

![6](https://user-images.githubusercontent.com/93249038/210702823-8d3a69e3-e649-4295-8d77-b8700a208eec.jpg)

7)Now, we've just been using constructor functions (function Dog() { ... }), which is still valid JavaScript. However, ES6 actually introduced an easier syntax for constructor functions and working with prototypes: classes!

Classes are only syntactical sugar for constructor functions. Everything still works the same way!

We write classes with the class keyword. A class has a constructor function, which is basically the constructor function we wrote in the ES5 syntax! The properties that we want to add to the prototype, are defined on the classes body itself.

![7](https://user-images.githubusercontent.com/93249038/210703124-dbeee89a-2733-4c9f-894d-9191321c8000.jpg)
Another great thing about classes, is that we can easily extend other classes.

Say that we want to show several dogs of the same breed, namely Chihuahuas! A chihuahua is (somehow... 😐) still a dog. To keep this example simple, I'll only pass the name property to the Dog class for now instead of name, breed and color. But these chihuahuas can also do something special, they have a small bark. Instead of saying Woof!, a chihuahua can also say Small woof! 🐕

In an extended class, we can access the parent class' constructor using the super keyword. The arguments the parent class' constructor expects, we have to pass to super: name in this case.

![smallbark](https://user-images.githubusercontent.com/93249038/210703196-11693a66-557c-423b-8bec-a6a721c06e0f.jpg)

8) ![8](https://user-images.githubusercontent.com/93249038/210703261-dbd42e15-598b-4a18-93cb-46da13a2e3fb.jpg)

Since Chihuahua.prototype has the smallBark function, and Dog.prototype has the bark function, we can access both smallBark and bark on myPet!

Now as you can imagine, the prototype chain doesn't go on forever. Eventually there's an object which prototype is equal to null: the Object.prototype object in this case! If we try to access a property that's nowhere to be found locally or on the prototype chain, undefined gets returned.

We do this, by passing an existing object as argument to the Object.create method. That object is the prototype of the object we create!

![obj create](https://user-images.githubusercontent.com/93249038/210703402-781c575a-7b67-423e-a9c6-a4ba3eff8e2b.jpg)
Let's log the me object we just created.

![me](https://user-images.githubusercontent.com/93249038/210703460-88c97f52-3304-4583-bf34-86ca578b4cea.jpg)

