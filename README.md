# Javascript Complete

## Advanced Function Concepts

### Pure Functions & Side-Effects

* Let's start with pure functions and side effects, what is that? Well, a pure function is a function which for some given input, so for some given arguments, always produces the same output, so the same arguments, the same values for the given arguments always produce the same output

```js
function add(num1, num2) { // pure function
  return num1 + num2;
}

// function sendDataToServer() {}

console.log(add(1, 5)); // 6
console.log(add(12, 15)); // 27

function addRandom(num1) { // impure function - you can't predict the output for the input
  return num1 + Math.random();
}

console.log(addRandom(5));

let previousResult = 0;

function addMoreNumbers(num1, num2) {
  const sum = num1 + num2;
  previousResult = sum; // we change the value that is defined outside of this variable
  return sum;
}

console.log(addMoreNumbers(1, 5));

// Another example for a function with a side effect would be a function that changes an object or an array that you pass into it.
const hobbies = ['Sports', 'Cooking'];

function printHobbies(h) {
  h.push('NEW HOBBY');
  console.log(h);
}

printHobbies(hobbies);
```

*  A function is also not to be considered pure if it introduces side effects which is a fancy term for saying it changes anything outside of the function, that could be an HTTP request which you are sending or data which is stored in the database but it could also be something more trivial, like let's say changing some variable which is defined outside of the function. 

### Impure vs Pure Functions

* Now that being said, when you're building an application, when you're writing code, it will be impossible for you to avoid introducing side effects at some point of time. A function might need to set up some event listener or might need to send some data to a server and that is absolutely fine.

* The goal just is to minimize the amount of functions which are impure or which have side effects, just work with your code in a logical way, try to keep your functions predictable and pure and if you then occasionally have some functions that need to perform a side effect or that needs to be impure, that's absolutely fine.

* Maybe just try to be clear regarding the function naming that this might be a function that does something as a side effect, for example a function which is named send data to server would be expected to send some data to a server and it wouldn't come as a surprise that this function as a side effect produces an HTTP request. A function which is just called add on the other hand should probably not create some side effect or be impure because a function with this name, well we would expect this to be pure, right?

* As always, it comes with experience and now of course it's OK to not be perfect regarding that, you should definitely be aware of this concept of pure functions though and as I said, aim for more pure functions and less impure functions that might introduce side effects.

### Factory Functions

* let's dive into factory functions. Pretty unrelated to pure functions but still an important concept.

* The idea behind factory functions is that you have a function that produces another function, now that might sound strange but it makes more sense once we see an example.

```js
function createTaxCalculator(tax) {
  function calculateTax(amount) {
    return amount * tax;
  }

  return calculateTax;
}

const calculateVatAmount = createTaxCalculator(0.19);
const calculateIncomeTaxAmount = createTaxCalculator(0.25);

console.log(calculateVatAmount(100));
console.log(calculateVatAmount(200));
```

* so it runs separately two times, with different arguments for the tax, so different values for the tax argument and therefore this inner function is recreated two times, once for every execution of the outer function and every time it logs in the tax percentage of that outer function which differs for the two executions and therefore the function which we return is this preconfigured inner function with the logged in tax amount

### Closures

* So that are factory functions and closely related to factory functions is the concept of closures.Now here's an important rule which you can memorize for Javascript interviews, every function in Javascript is a closure, so why is every function a closure then? What is that concept of being a closure then?

* Well this is closely related to the idea of having scopes for our variables

* you have different scopes - you have block scope when we're working with variables that are created with const or let and that simply means that inside of curly braces like for example here in a function definition, if you create a variable in there or if you get a parameter, it's only usable inside of that function but not outside of it.

* On the other hand, global variables or constants which are created outside of functions can be used inside of the functions, that's something you already knew.

* Now if you have a function in a function, that inner function can use all the variables or parameters of the outer function and all variables that are defined globally, the outer function can not access the inner functions specific constants or variables.

```js
function createTaxCalculator(tax) {
  function calculateTax(amount) {
    return amount * tax;
  }

  return calculateTax;
}
```
* So for example here, amount could not be accessed from inside that outer function, only from inside that inner function, so that's what you already knew.

* The more technical term for this would be that we have different lexical environments.Each function has its own lexical environment and you have a global environment as well and the variables and constants are registered in these different environments you could say.

```js
const calculateVatAmount = createTaxCalculator(0.19);
const calculateIncomeTaxAmount = createTaxCalculator(0.25);

```
* Now when we call create tax calculator, then this inner function is created, not before we do so because it's inside of that creates tax calculator function,

* So when this inner function then is created in here, something interesting happens, it's in this case here logs in the value for tax when this outer function runs.

* So if you then call that outer function again with a different value, since we execute a brand new function, since we have a totally different function execution, the inner function receives this brand new tax value and is totally detached from that other function execution.

```js
var multiplier = 1.1;
function createTaxCalculator(tax) {
  function calculateTax(amount) {
    return amount * tax * multiplier;
  }

  return calculateTax;
}

const calculateVatAmount = createTaxCalculator(0.19);
const calculateIncomeTaxAmount = createTaxCalculator(0.25);

multiplier = 2.2 // before execute inner function we changed multiplier now multiplier value is new updated 2.2 for inner function

console.log(calculateVatAmount(100));
console.log(calculateVatAmount(200));
```
* So what does this tell us?

* It tells us that we do log in tax here because that's part of this specific environment of the outer function when it runs but we don't log in the concrete value of multiplier because that's part of the global environment

* and in the end what happens is just each function registers its surrounding environments and the variables that are defined in there and if these variables change and this function uses such a variable, then it takes the latest value.

* Now for tax in this case here, that would be the case too but when this inner function is created, it has a look at its surrounding environment which is this functions, the create tax calculator functions environment for example and there we have a tax variable, a tax parameter and if that would change, the inner function would take the new one but this never changes because the only time we pass in a new value is when we execute this function again,

* this however will be a brand new execution and not change that inner function that was created on the first function execution here, which is why that inner function created by that first function execution still has its old environment with the tax you set up here as an argument. 

* Now why is it called a closure

* Because every function closes over the surrounding environment which means it registers the surrounding environment and the variables registered there and it memorizes the values of these variables.

* If you then change a variable let say multiplier, well that is reflected inside of the function but if it does not change, the function still is able to use for example the tax value you passed in when you created the outer function.

* You could say when the outer function, when create tax calculator is called with a value of .19, then what happens is that this function runs, a new function is created which for the moment is simply a task that is done pretty fast and then we return this created function. So you could say that theoretically tax isn't used anymore,the inner function uses it but Javascript could just ignore that.

* The outer function is done and it was the outer function which received the tax parameter, so we could think that Javascript just gets rid of that tax value and ignores it

* So a function, every function in Javascript is a closure because it closes over the variables defined in its environment and it kind of memorizes them, so that they're not thrown away when you don't need them in the surrounding context anymore.

### Closures in Practice

```js
let userName = 'Max';

function greetUser() {
   let name = userName;
  console.log('Hi ' + name);
}

userName = 'Manuel';

greetUser(); // Hi Manuel
```
* now name is part of the environment of the function, so of the lexical environment of the function itself, it however points or it refers to user name which is part of the outer lexical environment, what would you expect now?

* we again see Hi Manuel and the reason for it is the same as before, we do refer to that user name variable inside of our function but when the function executes, it reaches out to that surrounding lexical environment to which it holds a pointer, so to which the functional the pointer and gets the latest value from there, which is why we see this.

```js
let userName = 'Max';

function greetUser() {
   let name = 'Anna'; // while creation we created this local scope name with value as 'Anna'
  console.log('Hi ' + name);
}

let name = 'Maximilian'; // here we created global name variable

userName = 'Manuel';

greetUser(); // Hi Anna
```

* because name here is created as a variable inside of the function. Now you also learned earlier that you can have a function with the same name outside and inside of the function, this is a concept called shadowing, the inner function, the inner environment you would say wins over the outer environment.

* So this function has its own lexical environment and there it adds a name variable, it also adds a pointer at the outer lexical environment and there, it also at the time it runs will have a name variable but and that's really important, when the function executes, it first of course checks its inner environment and only if it doesn't find a variable there, then it goes to the next level, to the outer environment, so the environment of a surrounding function or the global environment and then it checks that outer environment for that variable of that name but here we do have that variable inside of the function so there's no need to go to the outer one and look for this name and that's what we see Hi Anna.

```js

function greetUser() {
   //let name = 'Anna'; 
  console.log('Hi ' + name);
}

let name = 'Maximilian'; // here we created global name variable

greetUser(); // Hi Maximilian
```
* Things of course would be different if I would comment out the name declaration inside of that function. Now I still refer to a name variable which wasn't even declared when the function was created but which was declared before the function was called and therefore here if I reload, we indeed see Hi Maximilian because now we can't find the name variable on the inner lexical environment and therefore we have to reach out to the next one in line which in this case is the global lexical environment and there, we find that name.

### Closures & Memory Management

* Now one word about memory consumption and memory usage. If every function logs in all surrounding variables, doesn't that lead to a pretty bad effect in our memory because in big applications where we have many variables, a function might log in a lot of variables which it never uses. So Javascript doesn't get rid of these variables because a function closed over them but the function might never get called or even if it gets called, it might not be interested in all variables, so isn't that a memory issue?

* In theory, it would be but of course Javascript engines are pretty smart or the people who are working on them are pretty smart and therefore, modern Javascript engines optimize this.

* They basically track variable usage you could say and if a variable obviously isn't getting used by anything, not by any functions and nowhere else, then they will get rid of it and they will do so of course in a safe way so that they don't accidentally crash your program because you need to use that function at some point of time, they're really smart about that and they optimize this whole behavior so that you don't manually have to reset all variables that you don't need, Javascript engines will do that for you in a smart way.

### Optional: IIFEs

* In JavaScript - especially in older scripts - you sometimes find a pattern described as "IIFEs". IIFE stands for "Immediately Invoked Function Expression" and the pattern you might find looks like this (directly in a script file):

```js
(function() {
    var age = 30;
    console.log(age); // 30
})()
 
console.log(age); // Error: "age is not defined"
```
* What's that?

* We see a function expression which calls itself (please note the () right after the function).

* It's NOT a function declaration because it's wrapped in () - that happens on purpose since you can't immediately execute function declarations.

* But why would you write some code?

* Please note that the snippet uses var, NOT let or const. Remember that var does NOT use block scope but only differ between global and function scope.

* As a consequence, it was hard to control where variables were available - variables outside of function always were available globally. Well, IIFEs solve that problem since the script (or parts of it) essentially are wrapped in a function => Function scope is used.

* Nowadays, this is not really required anymore. With let and const we got block scope and if you want to restrict where variables are available (outside of functions, if statements, for loops etc - where you automatically have scoped variables since these structures create blocks), you can simply wrap the code that should have scoped variables with {}.

```js
{
    const age = 30;
    console.log(age); // 30
}
 
console.log(age); // Error: "age is not defined"
```
* Not something you see too often but something that is possible.

### Introducing "Recursion"

* So now that we learned a lot about closures and scope and so on, let's have a look at recursion which is a quite interesting pattern, a quite interesting way of using functions, which is not exclusive to Javascript but which you can use in any programming language.

* Let's start with a simple recursion. Let's say we want to create a function, power of which should calculate the power of something

* and this function should get two arguments - the value and then the power which we want to use, so that we for example can run power of two and three and what we would get is the result 2*2*2  which would be eight, that's two at the power of three.

```js
function powerOf(x, n) {
  let result = 1; // initial result is 1

  for (let i = 0; i < n; i++) {
    result *= x;  // updating result
  }

  return result; // returning final updated result
}

console.log(powerOf(2, 3)); // 2 * 2 * 2
```

* Now we could implement it like this but with the concept of recursion, we can actually write this in a shorter way.

```js
function powerOf(x, n) {
  if (n === 1) {
    return x; // if n = 1 exit loop
  }
  return x * powerOf(x, n - 1);
}

console.log(powerOf(2, 3)); // 2 * 2 * 2
```

* In more shorterway we can use recursion function

```js
function powerOf(x, n) {
  return n === 1 ? x : x * powerOf(x, n - 1);
}

console.log(powerOf(2, 3));
```
### Advanced Recursion

* So recursion can save us code, it can also solve problems which we couldn't solve with a for loop

```js
const myself = {
  name: 'Max',
  friends: [
    {
      name: 'Manuel',
      friends: [
        {
          name: 'Chris',
          friends: [
            {
              name: 'Hari'
            },
            {
              name: 'Amilia'
            }
          ]
        }
      ]
    },
    {
      name: 'Julia'
    }
  ]
};

function getFriendNames(person) {
  const collectedNames = [];

  if (!person.friends) {
    return [];
  }
  
  for (const friend of person.friends) {
    collectedNames.push(friend.name);
    console.log(friend)
    collectedNames.push(...getFriendNames(friend)); // we should spead here because here we will get return collectedNames from getFriendNames
  }
  
  return collectedNames;
}

console.log(getFriendNames(myself));
```

* we now have recursion to go through that data structure here. And you see how flexible this approach is, we can easily add more friends and deeper and deeper nesting of friends and due to the way how this function is written and the logic we have in there, this will go through as many levels of nesting as we need because it doesn't care, it calls itself and dives into the friends until there are no more friends for a given person and then it returns and returns and returns until it's done with the outermost function call where then it returns the overall value.