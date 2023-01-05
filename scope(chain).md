


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
