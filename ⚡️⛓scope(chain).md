
![Global execution context](https://user-images.githubusercontent.com/93249038/210699980-dd775b9b-a15a-4b5b-bbed-09c2b99cfd55.jpg)


Let's take a look at the following code:

const name = "Lydia"
const age = 21
const city = "San Francisco"


function getPersonInfo() {
  const name = "Sarah"
  const age = 22

  return `${name} is ${age} and lives in ${city}`
}

console.log(getPersonInfo())
We're invoking the getPersonInfo function, which returns a string containing the values of the name, age and city variables:
Sarah is 22 and lives in San Francisco. But, the getPersonInfo function doesn't contain a variable named city 🤨? How did it know the value of city?

First, memory space is set up for the different contexts. We have the default global context (window in a browser, global in Node), and a local context for the getPersonInfo function which has been invoked. Each context also has a scope chain.

For the getPersonInfo function, the scope chain looks something like this (don't worry, it doesn't have to make sense just yet):
![getpersoninfo](https://user-images.githubusercontent.com/93249038/210699993-171933e5-7a26-4fa8-a84e-de76228beaed.jpg)

The scope chain is basically a "chain of references" to objects that contain references to values (and other scopes) that are referencable in that execution context. (⛓: "Hey, these are all the values you can reference from within this context".) The scope chain gets created when the execution context is created, meaning it's created at runtime!

However, I won't talk about the activation object or the execution contexts in general in this post, let's just focus on scope! In the following examples, the key/value pairs in the execution contexts represent the references that the scope chain has to the variables.

![However](https://user-images.githubusercontent.com/93249038/210700072-6456950b-86e2-4eb1-a911-5fc221c20154.jpg)

The scope chain of the global execution context has a reference to 3 variables: name with the value Lydia, age with the value 21, and city with the value San Francisco. In the local context, we have a reference to 2 variables: name with the value Sarah, and age with the value 22.

When we try to access the variables in the getPersonInfo function, the engine first checks the local scope chain.

![The scope chain](https://user-images.githubusercontent.com/93249038/210700160-684aee7a-975b-4884-8d90-0cd9f12903e3.jpg)

The local scope chain has a reference to name and age! name has the value of Sarah and age has the value of 22. But now, what happens when it tries to access city?

In order to find the value for city the engine "goes down the scope chain". This basically just means that the engine doesn't give up that easily: it works hard for you to see if it can find a value for the variable city in the outer scope that the local scope has a reference to, the global object in this case.
![The local scope](https://user-images.githubusercontent.com/93249038/210700212-3510ad83-db84-494b-bf9d-bb426a85f5a6.jpg)

In the global context, we declared the variable city with the value of San Francisco, thus has a reference to the variable city. Now that we have a value for the variable, the function getPersonInfo can return the string Sarah is 22 and lives in San Francisco 🎉

We can go down the scope chain, but we can't go up the scope chain. (Okay this may be confusing because some people say up instead of down, so I'll just rephrase: You can go to outer scopes, but not to more inner... (innerer..?) scopes. I like to visualize this as a sort of waterfall:
![outerinner](https://user-images.githubusercontent.com/93249038/210700364-1ba03d35-db66-4f6f-be77-f6ad05993ca6.jpg)

Even deeper :
![even deeper](https://user-images.githubusercontent.com/93249038/210700392-06dc33fc-7e1c-4724-b296-845c7f88fe9a.jpg)
![lets take this ](https://user-images.githubusercontent.com/93249038/210700441-48bcf03e-ae83-4742-b864-10b5503b6724.jpg)


It's almost the same, however there's one big difference: we only declared city in the getPersonInfo function now, and not in the global scope. We didn't invoke the getPersonInfo function, so no local context is created either. Yet, we try to access the values of name, age and city in the global context.

![referneceerr](https://user-images.githubusercontent.com/93249038/210700510-64c49292-c4f2-4f0f-8394-b3fe2070c11b.jpg)

It throws a ReferenceError! It couldn't find a reference to a variable called city in the global scope, and there were no outer scopes to look for, and it cannot go up the scope chain.

This way, you can use scope as a way to "protect" your variables and re-use variable names.

Besides global and local scopes, there is also a block scope. Variables declared with the let or const keyword are scoped to the nearest curly brackets ({}).
const age = 21

function checkAge() {
  if (age < 21) {
    const message = "You cannot drink!"
    return message
  } else {
    const message = "You can drink!"
    return message
    }
  }
  You can visualize the final scope :
  ![Scope](https://user-images.githubusercontent.com/93249038/210700600-3b0454ba-d4c3-450b-84cc-472181c68e10.jpg)
