# Javascript Complete

## More on Functions

### Parameters vs Arguments 

* Parameters are these variables which you specify between parentheses when defining a function.

```js
function sayHi(name) { ... } 
```

* In this example, name is a parameter.

* Arguments then are the concrete values you pass to a function when calling that function:

```js
sayHi('Max');
```
* 'Max' is an argument of the function therefore - for the name parameter to be precise.

* Since both concepts obviously are extremely close connected, I will often say "let's define which arguments a function receives" or something comparable, since defining the arguments of a function in the end means that you set up its parameters (and vice-versa).

### Functions vs Methods


```js
// const startGameBtn = document.getElementById('start-game-btn');

const start = function() {
  console.log('Game is starting...');
};

start();
// const person = {
//   name: 'Max',
//   greet: function greet() {
//     console.log('Hello there!');
//   }
// };

// person.greet();

// console.dir(startGame);

// startGameBtn.addEventListener('click', start);
```
* this would be this so-called direct execution which I meant.Now on the other hand, you can also go to the start game button and add an event listener

```js
const startGameBtn = document.getElementById('start-game-btn');

function start() {
  console.log('Game is starting...');
};

startGameBtn.addEventListener('click', start);
```

* this is this indirect execution.

* Now the first important thing which I want to highlight here or which I want to dive in is this add event listener function actually. This is not a function defined by us, instead it's in the end provided by the browser but we don't call it like this, instead we call it on this start game button, which seems to be an object because we use this dot notation. Now you learned that objects can have properties,

* Now here we also use the dot notation but what we then do is we don't access a property, we're not logging it or doing some calculation with it, instead we execute it as a function.

* so indeed we can store a function in an object and that is then called a method, it's just a term for it,

```js
const person = {
  name: 'Max',
  greet: function greet() {
    console.log('Hello there!');
  }
};

person.greet();
```
* if you store a function in such a property, so in a key, in an object, then you call it a method but a method is nothing else than a function attached to an object you could say.

* And addEventListener therefore is a method on this start game button object and this is an object generated for us by Javascript by calling this method up here, the getElementByID method which exists on this document thing.

* So storing functions in objects is possible, you call them like this and they're then basically methods, you call them methods if they're stored in objects.

### Functions are Objects!

* So functions in objects is a thing, what if I would tell you that functions themselves also are objects?

```js
console.log(typeof(start())) // here we execute start and this console will show whatever the type returned from start function

console.log(typeof(start)) // here we are not executing just we are passing the name here... it will return type as function
```
* Well actually, function is a separate type which is being logged here but it's based on object.

```js
console.dir(typeof(start)) 
```
* which gives us a different insight into objects and if we then go back and reload here, you see here we get this strange thing here, looks like a function but I can expand it and what you see here is actually, there's a bunch of strange stuff in it, we got a name for example, we got an arguments thing and these are all key-value pairs in there, so these are all properties of this function in the end. Now it also has a scopes thing and that's interesting, this basically tells Javascript where to manage the variables of these functions, so what the scope and on is and we don't need to worry too much about the stuff that's in there, some of the things like prototype will become clearer later in the course once we dive way deeper into objects and how they work internally, not something we have to worry about right now.

* The only key takeaway right now is that a function in the end whilst having its own type is an object. It's basically also an object, a special type of object if you will with special pre-configured properties but still an object

* and the implication of that is for example that it's typically stored in the heap as you learned and basically anything that applies to objects, for example that they are reference types also applies to functions. Often that might not really matter to you, it's still important to be aware of that,

### Function Expressions: Storing Functions in Variables

```js
const startGameBtn = document.getElementById('start-game-btn');

const start = function() { // Storing Functions in Variables
  console.log('Game is starting...');
};

startGameBtn.addEventListener('click', start);
```
* the implication is that this function is still created but it's not stored under this name here in the global scope anymore, instead this is just some kind of internal name which is attached to the function but which is not available in the global scope but instead, it's now stored in this constant.

* So you can store functions in variables or constants, what are you doing here in the end is you're using this function as an expression instead of as a declaration.

* Remember, expressions were essentially the thing that yielded you values, so something you could store somewhere else, I mentioned that earlier as well.

* but the difference is that Javascript will not go ahead and take that object and store it in the global scope for us in the end under that name but that instead, we just store it in this variable, that variable or that constant here is then stored in a global scope but we can therefore now only reference this function by using that variable or constant name, this is something we can do

* Now the concept of having an anonymous function is detached from the concept of storing a function in a constant.

* Refer : function-declaration-vs-expression.pdf

* Now the important difference to what it really boils down here is that for this function declaration, function statement syntax, Javascript automatically hoists the function to the top and then also fully initializes it for you.

* So what I mentioned, Javascript reads the file and no matter where you defined your function, it acts as if it would be defined at the top of the file, which means you can actually call functions before you declare them.

* With function expressions, this hoisting is also working but the constant gets hoisted but it gets hoisted as undefined something I also had a look at in this behind the scenes module.

* So it's basically not initialized so you can't call the function before you defined it and that's an important difference, you have to define, you have to initialize your functions before you try calling them

### Anonymous Functions

```js
const startGameBtn = document.getElementById('start-game-btn');

startGameBtn.addEventListener('click', function() {
  console.log('Game is starting...');
});

```
* instead of assigning anonymous functions to a variable and then addEventListener you can directly give eventLister with the anonymous function

* Now important, this is basically the same as if you define it before and store it in some variable and then just reference the variable here. This will not immediately execute the function, why would it, there are no parentheses after it,

```js
startGameBtn.addEventListener('click', function() {
  console.log('Game is starting...');
}());

```
* This would now immediately execute the anonymous function but we will not execute immediately

* There is nothing wrong with having a separate function and as you learned in the behind the scenes module, there might even be some memory leak considerations you want to take into account

* So this function actually takes a function as an argument, this is a great scenario for using such an anonymous function if you never need that same function in any other place in your code.

* So why might you still assign a name to this anonymous function though? Because maybe that function throws an error. Now what you'll see is if you want to find that error, of course it gives us the line number, so that helps us but what if we don't see here is the name of the function where this problem occurred.

* You can't call this function by that name as you learned, it's not declared and managed globally by Javascript but that name is used for debugging, for throwing errors by the browser. So if I now reload this and I produce this, you see start game here. So that's a reason why you might want to give your anonymous functions a name, you don't have to, obviously with the line number we would have found the problematic line as well but it is worth a thought and this is why you might still want to assign names to your anonymous functions,

### Experiment project

```js
const startGameBtn = document.getElementById('start-game-btn'); 

const ROCK = 'ROCK';
const PAPER = 'PAPER';
const SCISSORS = 'SCISSORS';
const DEFAULT_USER_CHOICE = ROCK;

const getPlayerChoice = function() {
  const selection = prompt(`${ROCK}, ${PAPER} or ${SCISSORS}?`, '').toUpperCase();
  if (
    selection !== ROCK &&
    selection !== PAPER &&
    selection !== SCISSORS
  ) {
    alert(`Invalid choice! We chose ${DEFAULT_USER_CHOICE} for you!`);
    return DEFAULT_USER_CHOICE;
  }
  return selection;
};

startGameBtn.addEventListener('click', function() {
  console.log('Game is starting...');
  const playerSelection = getPlayerChoice();
  console.log(playerSelection);
});

```
### Introducing Arrow Functions

* So what's special about such an arrow function? Refer : arrow-functions.pdf

* Well the advantage is it's a bit shorter, you save the extra function keyword and obviously that's not much to type but it's a bit shorter but in addition to that, this function syntax has some extra tricks up on its sleeve.

* Well if you have a function, an arrow function, where you only have one expression in it, you can actually omit the curly braces, so you can get rid of that and also you then have to omit the return keyword. So you can write this entire function in the end like this 

```js
const getWinner = (cChoice, pChoice) =>
  cChoice === pChoice
    ? RESULT_DRAW
    : (cChoice === ROCK && pChoice === PAPER) ||
      (cChoice === PAPER && pChoice === SCISSORS) ||
      (cChoice === SCISSORS && pChoice === ROCK)
    ? RESULT_PLAYER_WINS
    : RESULT_COMPUTER_WINS;

// const getPlayerChoice = function() {
//   if (cChoice === pChoice) {
//     return RESULT_DRAW;
//   } else if (
//     (cChoice === ROCK && pChoice === PAPER) ||
//     (cChoice === PAPER && pChoice === SCISSORS) ||
//     (cChoice === SCISSORS && pChoice === ROCK)
//   ) {
//     return RESULT_PLAYER_WINS;
//   } else {
//     return RESULT_COMPUTER_WINS;
//   }
// };
```
### Different Arrow Function Syntaxes

#### 1) Default syntax:

```js
const add = (a, b) => {
    const result = a + b;
    return result; // like in "normal" functions, parameters and return statement are OPTIONAL!
};
```
* Noteworthy: Semi-colon at end, no function keyword, parentheses around parameters/ arguments.

#### 2) Shorter parameter syntax, if exactly one parameter is received:

```js
const log = message => {
    console.log(message); // could also return something of course - this example just doesn't
};
```
* Noteworthy: Parentheses around parameter list can be omitted (for exactly one argument).

#### 3) Empty parameter parentheses if NO arguments are received:

```js
const greet = () => {
    console.log('Hi there!');
};
```
* Noteworthy: Parentheses have to be added (can't be omitted)

#### 4) Shorter function body, if exactly one expression is used:

```js
const add = (a, b) => a + b;
```
* Noteworthy: Curly braces and return statement can be omitted, expression result is always returned automatically

#### 5) Function returns an object (with shortened syntax as shown in 4)):

```js
const loadPerson = pName => ({name: pName });
```

* Noteworthy: Extra parentheses are required around the object, since the curly braces would otherwise be interpreted as the function body delimiters (and hence a syntax error would be thrown here). That last case can be confusing: Normally, in JavaScript, curly braces always can have exactly one meaning.

```js
const person = { name: 'Max' }; // Clearly creates an object
if (something) { ... } // Clearly used to mark the if statement block
```
### Default Arguments in Functions

```js
let message = `You picked ${playerChoice || DEFAULT_USER_CHOICE}, computer picked ${computerChoice}, therefore you `;
// Here we used || to set default argument
```
* default argument work only for undefined value.

### Introducing Rest Parameters ("Rest Operator")

* Now my idea here is that we can pass in as many arguments as we want and build a sum for those, so not an array but as many arguments as we want.

* So here I want to have like a, b, c, d but I want to have an infinite amount of arguments here, that's my idea and you can see where the problem lies, I can't know when I'm writing this code with how many arguments this function will be called. Well obviously, I will know if I'm the one calling it here but let's say this is more dynamic, it's done inside of a loop which is based on some user input.

* So anything where we can't fully control how often that will be called. So here I will call it myself in the code but again let's assume I'm not the one calling it like this but instead, we wrote some code where this is called dynamically with a different amount of arguments.

```js
const sumUp = (a, b, ...numbers) => {
  let sum = 0;
  for (const num of numbers) {
    sum += num;
  }
  return sum;
};

console.log(sumUp(1, 5, 10, -3, 6, 10));
console.log(sumUp(1, 5, 10, -3, 6, 10, 25, 88));
```
* So this should be flexible regarding the amount of arguments it gets.

* maybe you really want to allow that the function is called with multiple arguments, for whatever reason that may be but you want to make sure that this is the case. In such a case, you can call the so-called rest operator.

* It looks like the spread operator which I showed to you in the behind the scenes module, you add three dots but the place where you use that operator is different from the spread operator.

* You might remember that spread operator was used when you created an object or when you created an array and then you could use the three dots in there to take an existing object or an existing array and pull out all the key-value pairs or all the elements of the array and add them to the new object or new array.

* Now here we use the three dots in a different place, we use it in a parameter list and there in such a function parameter list, it does something different. Instead of pulling elements out of an array, here it takes all arguments this function gets and that might be as many as you want and it merges them into an array,

* So now you can call the function without passing an array by having real different arguments but they're getting merged into an array inside of the function, which you of course don't see when you just call the function and then we can work with that array and in this case, return our sum.

* you can't pass a new argument after rest arguments

* So if you would have arguments after an argument which uses the rest operator, you can never reach that argument. So here, we even get an error rest parameter must be last formal parameter for this exact reason so this is not allowed.

* You also can only have one parameter, one rest parameter I mean because for the same reason, this automatically merges all arguments together so there is no room for a second rest parameter.

* What you can have though is parameters in front of rest arguments the first two arguments in this case, 1 for a and 5 for b will be assigned to these parameters and only all the other arguments or parameters thereafter will be merged together.So you really got a lot of flexibility here with the rest operator. Now a little side note, when not using an arrow function,

```js
const subtractUp = function() => {
  let sum = 0;
  for (const num of arguments) { // don't use this.... arguments use rest operator, arguments is a pretty old method before rest operator
    sum += num;
  }
  return sum;
};

console.log(sumUp(1, 5, 10, -3, 6, 10));
```

* Now you can of course as I just explained use the rest parameter here just as you did it in the arrow function and if you do so, everything will be fine, we can reload and it works but now and that's the blast from the past thing, we can also use something different. In functions created with the function keyword, you have a special variable available which you never define, which you don't accept as an argument but which is just magically there so to say and that's the arguments variable.

* Arguments as you see is already marked as a keyword because it's built into Javascript, you can use it inside of functions but only inside of functions that use the function keyword and it gives you an array-like object, not a true array but array-like with all the arguments this function got.

* So before the rest operator was introduced which happened with ES6, this was the way of building a function like this one, so this was then the automatically merged array. so this is an alternative to using the rest operator.

### Creating Functions Inside of Functions

```js
const sumUp = (a, b, ...numbers) => {
  const validateNumber = (number) => {
    return isNaN(number) ? 0 : number;
  };

  let sum = 0;
  for (const num of numbers) {
    sum += validateNumber(num);
  }
  return sum;
};
```
### Understanding Callback Functions

* As we already saw for addEventListener we are passing another function...

* what we're effectively doing here therefore is we're passing a pointer at a function to another function. So this function here is effectively a parameter or a value for a parameter of the add event listener function and that's a pattern you will see in Javascript a lot, you can also build for your own functions but especially built-in functions will often have this behavior where you need to provide them with a function, typically all built-in functions that do something which is event based or which takes a bit longer because this is a pattern called a callback function.

* In the end what you you're doing here is you're creating a function, either in place with this anonymous function or you store that in a constant before, doesn't matter and you're just passing a pointer at the function to add event listener, that's what you already learned.

* Now you never call the function yourself, right, the browser, so the thing behind add event listener in the end, calls it for you when that click event in this case occurs.

* Now this is called a callback function therefore because it's called for you by something else, you can't control when exactly it's called, it's just coming back at you, it's called for you at some point in the future.

* for example for events, is the core pattern because you can't execute the function on your own, you don't know when the click occurs, you just want to give the browser the function or a pointer at a function to execute for you. That's why in this case you pass a function as a value for an argument to another function because the browser needs that function to do its job

* Now you can also build your own function that takes a function as an argument.

```js
const sumUp = (resultHandler, ...numbers) => {
  const validateNumber = (number) => {
    return isNaN(number) ? 0 : number;
  };

  let sum = 0;
  for (const num of numbers) {
    sum += validateNumber(num);
  }
  resultHandler(sum);
};

// so i expect to get the result Here
const showResult = (result) => { // this is the resultHandler(sum)...
  alert('The result after adding all numbers is: ' + result);
};

sumUp(showResult, 1, 5, 'fdsa', -3, 6, 10); // while call sumUp we should provid this function like this 
sumUp(showResult, 1, 5, 10, -3, 6, 10, 25, 88);
```
* Let's say the sum up function here should actually not to return the sum here but instead it should execute a function which we pass to it which then gets the sum as an argument.

### Working with "bind()"

```js
const combine = (resultHandler, operation, ...numbers) => {
  const validateNumber = number => {
    return isNaN(number) ? 0 : number;
  };

  let sum = 0;
  for (const num of numbers) {
    if (operation === 'ADD') {
      sum += validateNumber(num);
    } else {
      sum -= validateNumber(num);
    }
  }
  resultHandler(sum); // sum we are passing here will be automatically appeneded as last argument of showResult function 
  };


const showResult = (messageText, result) => {
  // we will receive result from resultHandler(sum) as last argument 
  alert(messageText + ' ' + result);
};

combine(
  // here we are binding  the message with showResult function but we are not calling showResult function.

  // messageText is our firt argument
  showResult.bind(this, 'The result after adding all numbers is:'),
  'ADD',
  1,
  5,
  1,
  -3,
  6,
  10
);
combine(
  showResult.bind(this, 'The result after adding all numbers is:'),
  'ADD',
  1,
  5,
  10,
  -3,
  6,
  10,
  25,
  88
);
combine(
  showResult.bind(this, 'The result after subtracting all numbers is:'),
  'SUBTRACT',
  1,
  10,
  15,
  20
);
```

* Well functions are objects and they actually have special properties attached to them or to be precise, special methods here and one method is the bind method.

* Now you just call .bind as a method on your function object and you do execute this and what bind will do is it will create a new function, a new function, a new reference at a function which it returns to you which will be preconfigured regarding the arguments it receives and that's the interesting part.

* So with bind, you can create a function which is not immediately executed, what would be the case if you manually call it like this but instead, which is prepared for a future execution, where certain values for certain parameters which you already know at this point of time are already set.

* So how does this now work? Well bind takes at least two arguments, the first argument is one we have to ignore for now. It sets a value for a thing called this, a special keyword in Javascript 

* Again this does not execute show result immediately, it just prepares it for execution.

* So it works exactly as it should, thanks to the help of bind which allows us to preconfigure functions in places where we need to pass in a value but we also don't want to directly execute a function.

### call() and apply()

* Bind is a really useful function or method to be precise which you will use quite often as a Javascript developer to prepare your to-be-called functions which you don't call yourselves.

* Now, there are similar functions which you often find when you research the bind method and that is the call and the apply method.

* Now essentially, they also allow you to specify what should be passed in as parameters but they also immediately execute the function and that's of course not what bind does, this prepares the function. Apply and call immediately execute the function

* and there also is a difference between apply and call but since also this part with immediate the executing a function isn't really helpful to us right now because we can always call a function like this if we want to immediately execute it, we'll ignore apply and call for now and dive into them later