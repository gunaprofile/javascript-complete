# Javascript Complete

## Behind the Scenes & The (Weird) Past (ES3, ES5) & Present (ES6+) of JavaScript

###  ES5 vs ES6+ ("Next Gen JS") - Evolution of JavaScript

* ES stands for ECMAScript and you might remember from section one that ECMAScript in the end is the language behind Javascript so to say, you could say Javascript is a specific version of ECMAScript, of course the most important version.So ECMAScript in the end is the language being standardized by this ecma international body

* one major version that was released in the past was basically ECMAScript version 5

* Now ECMAScript 5 included some important improvements and most importantly, standardization back then because before that, a lot of browsers basically did what they wanted and this was the first real big standard after ECMAScript 3 which also went into this direction where the browsers could agree on and where we then had one common ground for browsers to implement features, so that was very important.

* Now one other very important major in which was kind of finalized in 2015 and therefore is also known as ECMAScript 2015 or also another version number assigned to it, ES6,

* so another major milestone was released in or was finished in 2015 and from that point on slowly integrated into browsers and that's also important to understand. Just because there is a new standard, a new draft on which the people involved in the standardization process agreed on does not mean that all browsers suddenly ship all these features, instead features are getting added to browsers incrementally, so step-by-step and at different speeds and old browsers, so for example Internet Explorer version 9 of course don't include these new features.

* ES6 was another major mark in development of Javascript and since then more, features have been added, we just don't have these striking new version numbers, instead now it's more of an incremental process where we have basically small advancements that are integrated by browser vendors step-by-step. So ES6 and later is what this course teaches from the beginning on because we have some huge syntax changes from ES6 to ES5

* Changes really means new features added, not old features removed, so you could still use the old syntax, the old features but ES6 introduced many new exciting features and since then, many more exciting features have been added

* Refer : es1 image !!!!

### var vs let & const - Introducing "Block Scope"

* variables created with let are changeable, variables credited with const are constants 

* Now it turns out there actually is also a third way of creating variables which I haven't taught you thus far and you'll see why I haven't taught it in these lectures here. You can create a variable with the var keyword as well, so besides let and const, that is also available.

* Var creates a variable, just like let does and the only difference we can clearly see right now is that const creates a constant. So var indeed, just like let creates a variable which you can really change, which of course raises the question why do we have var and let? If they do the same, why do we need these two different keywords?

* Now let's first have a look at availability and there var has been available since ever, this is the way to create variables since Javascript exists essentially. Let on the other hand is only available since that ES6 version, so since browsers started to adopt that and it's the same for const. So that's a difference but this only explains since when we can use these features, not why we need let

* Var allows us to create variables in the function and global scope but actually let does not really use function and global scope, although in many cases it behaves exactly like that but it uses a concept called block scope and the same is true for const.

* So let's now dive into block scope and find out how that differs from function and global scope 

```js
let name = 'Max'; //Global block scope

if (name === 'Max') { 
  let hobbies = ['Sports', 'Cooking']; //if block scope
  console.log(hobbies);

}

function greet() {
  let age = 30; // function block scope
  let name = 'Manuel'; //function block scope
  console.log(name, age, hobbies);
}

console.log(name, hobbies);

greet();
```
* but actually let and const don't care about functions, they care about the curly braces, therefore they are only available in that block, they don't spill outside of that block and that is a key difference and that is great because that gives you more control over where your data is available.

* Refer : es2

### Understanding "Hoisting"

* So that block scope thing really is extremely important and is one of the major changes we saw from ES5 to ES6.

* One other major difference between var and let, which also is yet another reason to use let and const instead of var is how Javascript actually read and initialized variables created with var versus how it does that for variables and constants created with let and const.

```js
console.log(userName);

var userName = "test"
```
 * for var this it will console undefined

 ```js
console.log(userName);

let userName = "test"
```
* for let this it will throw error as "uncaught reference error- cannot access 'userName' before initialization".

* So with let and const, we get an error, with var we don't. Now why is that?

* Well Javascript has a special feature called hoisting which in the end means that the Javascript engine, the browser, when it loads your script goes over your entire script and it does things like look for functions which it then automatically loads and registers so that you can write functions below the code where you actually use them,

* so that's what the browser does invisibly and then just leaves your initialization, so where you assign a value down there, where you really wrote the code. So invisibly, the browser does this. Now it does not only do that for var but also technically for let and const, this hoisting concept also exists but there, the browser doesn't initialize this variable to undefined, it just kind of declares it but assigns no initial value at all and therefore you get this error that there has been no initialization.

* For var, this is different, there the browser pulls this up and also assigns an initial value of undefined, so that you get no error message but you have an undefined value here.

* Now that's one difference and it's not necessarily an advantage that you could do that with var because it really leads to a code that's harder to understand. If I'm a developer and I'm reading your code and let's say you have more code in there, so the declaration is way at the bottom of the file and I find this line, I as a developer might be wondering wait, where is userName coming from, I didn't see that thus far in the file and of course knowing Javascript, I'm aware that it could be defined somewhere below but why would you do that, that's really hard to understand, you have to dig through the entire file to find out what's in userName. So it's much clearer if you are forced to declare a variable before you start using it, so that you're forced to move your declaration at the top and of course you could do that with var and older versions of Javascript but you weren't forced to do it, with let and const you actually are, so that's an advancement.

### Strict Mode & Writing Good Code

* you can create a variable without any keyword at all, so just like this.

```js
userName = 'Guna';
```
* here we didnt mention let , const or var, If you save that and you reload your file, you see Guna is getting printed here and that might be strange because I did use neither var nor let nor const here.

* Now actually, that's a default behavior of Javascript because Javascript is quite a forgiving language. It sees that you missed a keyword here but in the end, this can only be a variable declaration and therefore it invisibly adds var.

* Now actually, that's a default behavior of Javascript because Javascript is quite a forgiving language. It sees that you missed a keyword here but in the end, this can only be a variable declaration and therefore it invisibly adds var.

* People reading your code might think that maybe you declared this variable userName somewhere earlier in this file and therefore they might go looking for it and never find it which costs some time or that it's even defined in some other script which you import before this script and which would therefore expose userName as a global variable,that would be possible, right? So people might think that this is declared somewhere else and might be looking for it when you actually just forgot to add var, let or const.

* Now actually to disable such forgiving behavior, Javascript has a special mode which you can turn on and that's the strict mode. It was introduced with ES5 and even though modern Javascript syntax as you're learning it in this course already prohibits some of the things you can rule out with strict mode, it might still be worth turning it on,

```js
'use strict';

userName = 'Guna';

console.log(userName);
```
* It might look strange but browsers understand this, they pick this up and they now know that you want to turn on strict mode for this script or for this function where you place this in,so only for this script, not for all scripts on your webpage, just for this script or for this function where it's placed in

* strict mode disables certain behaviors, so for example behaviors like this and attached, you find a link with a full list of features or behaviors that are disabled by strict mode.

* Refer : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode#Changes_in_strict_mode

* So with strict mode enabled, for example if you now reload here, you get an error, userName is not defined, previously that was not the case, previously that worked, now you get an error which is good because you don't want to allow that behavior here.

* Another feature which was disabled is that you could use reserve name and assign values to it, for example if you used undefined, which is a reserved name because that's a special value in Javascript, as a variable name, then without strict mode, if I comment this out, this will actually work. If I now reload, this works, we get no error.

* Now that already changes if we use let or const and that's what I meant, the modern syntax already disables some of these behaviors but if I use let or const here and I reload, we get an error that it has already been declared because it's built into Javascript and we also would get an error if we would use var which we shouldn't but if we would and we turn on strict mode. If we do that, strict mode turned on and we use var and we try to assign a value to undefined, now if we reload here, we also get an error here, a different error message but still we get one. So that is strict mode

* So that is strict mode turning it on certainly is an option, though as I mentioned in this course you will already learn patterns and a syntax which doesn't use these bad behaviors, still you can turn it on to be super safe. Modern Javascript with let and const also already prohibits and avoids certain of these behaviors, like this thing with undefined you just saw

### How Code is Parsed & Compiled

* So let's have a look behind the scenes now, at how our Javascript code is executed and how all these things are working together.

* So we wrote our Javascript code,  Now we can split the question at how this is executed into two parts -

* one is what does the browser do with that code, how does that actually result in something being shown on our screen?

* And the second part is what the browser does internally with our code, with whatever it did with that, so that it manages the execution flow of our code, that things are executed in the right order and that our program works the way it should.

* These are two different questions as you will see throughout this module and here we'll start with what the browser does with this piece of code. 

*  So we wrote this code, we import it in our HTML file and therefore the browser reads the HTML file, that's what browsers do, detects the script import and then and that's already something you can memorize or keep in mind, whenever the browser hits a script imported in HTML or written as an inline script in HTML, it will execute it. So the browser comes in and executes our script,

* now what does execute the script mean? And for that, please keep in mind that the exact details of course depend on the browser and the engine used. The Javascript code you write always is the same but what the browser does with that code in detail, that differs and if you really want to dive super deep into that, attached you find a couple of links where you can dive way deeper into the different engines and how they work under the hood, though you absolutely don't need to go through that to learn and use Javascript, it's just a bonus if you're interested in that.

* Refer : https://hackernoon.com/javascript-v8-engine-explained-3f940148d4ef

* So we're talking about the browser executing the Javascript code, what does this mean?

* with parsing, I basically mean that the browser reads this Javascript code, loads it so to say and then execution is the actual process where something happens, where our code has an effect and for that part, browsers have a Javascript engine, every major browser has a Javascript engine, the engine of Google's browser Chrome for example is named v8, for Firefox it's called Spider Monkey and these engines do that parsing and execution and they typically consist of two parts - an interpreter and a compiler (the compiler typically is a just-in-time (JIT) compiler and I'll come back to what that means in a second.)

* So we then parse our script which basically means we load it and we then start execution and that is done by the interpreter.

* The interpreter basically loads our script, reads it, it then kind of translates it to byte code which is a bit easier to execute you could say and then it goes ahead and starts running our script

* but what's the job of the compiler then? What is a compiler? Well the interpreter starts executing our script but it does so line-by-line in an un-optimized way, which means the script execution works but is far from being as fast as possible. To have the best possible performance, you don't just want to interpret the code and basically execute it like this but instead you want to compile it to machine code and hand it off to your machine, to your operating system if you will because that will always be faster and that translation of your interpreted Javascript code to machine code is exactly what the compiler does.

* So the interpreter does not just start executing your script, it also hands off the byte code to the compiler, so this loaded script in the end, it hands this off to the compiler.

* Now the compiler is also built into the browser, so it's part of the Javascript engine the browser ships with, in case of the v8 engine which works in Google Chrome browser, the name of the compiler is turbo fan but that's just a side note, doesn't really matter. So the compiler, whilst the interpreter already started execution of your script, now compiles the script to machine code.

* Hence "Just in Time" compilation: The compiler starts compiling + executing the compiled code while the code is being read/executed.

* so this happens side-by-side with the interpreter executing your script. Now once this compilation machine code is done, which typically is also relatively fast, this machine code is handed off to your computer which then may execute it or which then will execute it and that's then the fast possible way of executing the script.

* Now the Javascript engine, the browser, applies a couple of optimization techniques to speed up that execution and compilation time, so for example code that didn't change between the last execution and the current execution is not necessarily recompiled but instead the already compiled code is taken again.

* So if an existing script or a part of the script executes again, it's not necessarily recompiled but instead maybe the already compiled code can be taken which of course is way faster than if you go through that interpreter, then the compiler and so on process again.

* it's also important to note that the browser also gives you a couple of features, so-called APIs which are built in, which you can tap into from your Javascript code.Things like working with the loaded HTML code or for example,
a couple of other built-in APIs, for example for getting the user's location.

* These browser APIs are part of the browser, for example written in C++ but again that depends on the browser you're using and the browser gives you communication bridges if you want to call it like this which you can call from Javascript. So it gives you Javascript functions or objects which are just available in your Javascript code, which you can simply call in your Javascript code and when that code then gets interpreted and compiled, the browser knows where to find these functions or objects, it knows where to find these APIs and it basically includes that call to that native API or to that feature which is built in the browser into the interpreted or compiled code and therefore the finished compiled code which runs on a machine of course also has access to these browser APIs because that's access or that wish for access, whatever you want to call it, is basically included in that compiled machine code.

* Refer : js-engines-in-detail.pdf

### Inside the JavaScript Engine - How the Code Executes

*  if we take a closer look, if we zoom in into our code execution, how exactly does that work? I don't mean the technical aspects regarding the code getting compiled but how the different steps in our code, how the logic there is executed.

* So let's look inside the Javascript engine, the thing that executes our code and there, it's in the end all about managing memory and managing execution steps you could say and for this, we have two important concepts - the heap and the stack. Now what are these concepts? What's the idea?

* The heap is our long term memory if you will, it's all about memory allocation. It stores data in our system memory and with it, I mean the browser does that, the browser manages does heap thing, the browser taps into our system memory and of course our operating system has a say in that as well, it allocates memory to the browser application, so that the bigger picture but if we zoom into the browser and then into the Javascript engine, in the end the browser does this memory allocation in that thing which is called a heap.So there data gets stored with which we work in our program and important, long term data

* and then there is this stack thing. Now that stack thing is in the end important for the execution of our code. Of course the data is also important for that but the stack is kind of the short term memory if you will.

* It manages our program flow and mainly that means that the stack keeps control over which function is currently executing and when we execute a new function that this function is not executing,  and if a function returns, to which function it returns that data. 

* Now this becomes much clearer if we simply have a look at an example

```js
//app.js
const addListenerBtn = document.getElementById('add-listener-btn');
const clickableBtn = document.getElementById('clickable-btn');
const messageInput = document.getElementById('click-message-input');

function printMessage() {
  const value = messageInput.value;
  console.log(value || 'Clicked me!');
}

function addListener() {
  clickableBtn.addEventListener('click', printMessage);
}

addListenerBtn.addEventListener('click', addListener);
```
* In HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Behind the Scenes</title>
  <script src="app.js" defer></script>
</head>
<body>
  <input type="text" id="click-message-input">
  <button id="add-listener-btn">Add a listener to the other button</button>
  <button id="clickable-btn">Click me</button>
</body>
</html>
```
* Refer : how-code-executes.pdf

* functions themselves are basically stored in the heap, so that's where the browser stores these functions and the code it should execute when these functions are executed you could say and now when our script runs, the stack is getting active or to be precise, the browser in the end pushes stuff into our stack and remember the stack is basically our short term memory, heap is the long term memory.

* Now in the stack, it all starts with some anonymous code execution which basically is the script file itself, it doesn't have a name, of course the file has a name but it's like in a big gigantic function to which you didn't assign a name. So the code directly in the file is in a function without a name you could say and therefore, we have this anonymous execution in our stack, so that's the overall script which is getting executed.

* Of course that execution of our script only happens after Javascript evaluated the entire script and for example saw all the function definitions, that's what I already explained earlier

* So the script execution started, we're executing the entire script and that basically means that this greet line is getting executed, so we call the greet function, therefore Javascript moves this greet function execution onto the stack

* So the stack is in the end a data structure, a short living data structure you could say, where your function executions are stored so to say, so where the browser or the Javascript engine keeps track of what's currently happening and the stack is simply populated by pushing new function calls or new short living data on top of it and popping it off when it's not required anymore.

* So we started with the overall script execution, this anonymous function and in there, we call the greet function hence this is pushed onto the stack and is now the topmost item in the stack and the topmost item in the stack is always the thing which is currently happening you could say.

* Now if we have a look at the greet function, what happens there is that we create a variable and the variable is created by calling yet another function, the get name function.

* So now the Javascript engine pushes this get name function execution onto the stack, therefore it now knows this is the currently running function,it was started by the greet function which in turn was started by this anonymous function, so by our overall script. This is how Javascript or the Javascript engine can also keep track of the relations of these function calls, so which function called what.

* Now technically, we therefore also called the prompt("alert") function which of course is not a function written by us but a function provided by the browser which we can access through Javascript but still this is a function call and therefore this also gets pushed on top of the stack and it would continue like this, the more functions you call, the more it gets pushed onto the stack.

* Now what happens next? Prompt opens that dialog and once we entered something there and we clicked OK or also when we click cancel, prompt actually returns this value to get name. Now when a function returns or is done executing, it's popped off the stack, which means it's removed from the stack, it's removed from that short term memory because it's done.

* Now important, that does not mean that the function definition is removed from the heap, from that long term stack just as current execution which of course also involves resources that need to be allocated and data that needs to be managed for this function execution. That data, this execution context therefore is removed from the stack.

* like this one by one it will get popped off from the stack 

* which leaves us with the main script, with the script itself which you could imagine as being wrapped by a big anonymous function and now in this script, clearly once we're done with greet, there is no other step left, right, there is nothing after greet, no other steps left so we're done with this overall script execution. So therefore, we also pop this off, that means we're done with everything that needs to be executed at the moment.

* Now in the browser if we put a breakpoint and see the call stack (This is the stack i was talking about)

* This is exactly the stack I was talking about, not the full stack because these stack memory itself does actually not just manage function calls but also short living values stored in variables but it shows us the order in which the functions are executed and how they are managed as a stack and this is just the stack I was just referring to and here you see get name before you see greet and then you see this anonymous thing at the bottom. (Refer : es3)

* This is how Javascript manages the data and the execution context, the order of execution and this is how it therefore works under the hood. Now you might remember from the first course section that there I mentioned that Javascript is single threaded.

*  Now without getting too technical, that basically means that Javascript can only do one thing at a time. That's exactly what happened here, it called one function at a time and the other functions waited for the response of that function, just as you saw it in that stack.

* So the stack and this idea of managing our function calls and the flow of the script therefore in such a stack, that basically ensures that this single threading thing works and that the order of execution is ensured and that every function basically knows to which function it relates.

* Now there is one other important concept which is actually not part of the Javascript engine but of modern browsers, like Google Chrome browser but also of other browsers and that's a thing called the event loop.

* This will actually help us with asynchronous code which we haven't really learned too much about yet, it helps us with things like event listeners, these click listeners which we added to buttons

* we have this stack which does stuff and which eventually is empty kind of implies that a script always has a certain point of time where it's just done with everything,

* but the main thing which is important to us right now is that here, we're actually adding event listeners. We're adding event listeners here to buttons and that of course therefore means that if we would actually execute this 

* still if we go to the console and you enter something here in the input, click add a listener and then click me, you see this actually still does something, this output here is coming from our script from line 7. So this script is still alive, even though we're at the end of it and even though our call stack is empty.So that can be confusing, why is that happening?
Well that's exactly what that event loop is about.

* for now you can just keep in mind that there is something else in the browser, not in the Javascript engine, the engine is really just that heap stack thing but in the browser which communicates with the engine of course which is this event loop and in the end, you can imagine these click listeners as information being passed to the browser, so therefore the Javascript engine does not care at all about these ongoing listeners, the browser manages them and the browser basically pings the Javascript engine you could say whenever it has a new event being fired by the listeners set up by the Javascript code.

* So the browser kind of manages these listeners and whenever such a listener triggers, whenever a button is clicked which of course is part of the web page rendered by the browser, so the browser knows when that happens, whenever that happens, it basically pushes that information to your script you could say and it does so with the help of that event loop which is there to control that this pushed event does not interrupt with your ongoing script which might be running but instead is executed in order once your call stack is empty

* but again we'll have a detailed look at this event loop thing in a later module after we learned about asynchronous code.

###  Primitive vs Reference Values (important!!!)

* primitive versus reference values, now what is that? In Javascript, we have two categories or types or of values you could say.

* Refer : primitive-vs-reference-values.pdf

* Now primitive values are these six types of data - strings, numbers, boolean, null, undefined and symbol. So every number that you create and which you then store in a variable or which you use in a calculation is created by Javascript or by the browser behind the scenes as a primitive value.

* Thus far, that didn't really matter to us and in many cases, it won't matter to you but there will be one important difference to the reference values later. So it's stored in memory, normally on the stack because normally these are relatively short living because Javascript can get rid of them relatively easy, they're cheap to be recreated so it can easily duplicate them, this doesn't cost too much, not much memory is consumed by these values in the end and therefore, they're typically stored in its stack memory.

* So primitive values typically are stored in the stack. I'm saying typically because they could be stored in the heap as well if it's a long running operation and in the end, it's the browser which decides which data ends up where. It actually doesn't even matter for what I'm explaining here, I just want to briefly mention it here that these primitive values are generally managed in the stack.

* More important to us and the key difference between primitive and reference values is that when you copy a variable, which means you assign it to a new one which holds a primitive value, then the value is actually copied.

* Whenever we took some value in a variable or maybe in a parameter of a function and we stored that in another variable, it was always clear to us that if we change that original variable, the other variable where we stored it in would not change as well and that's the case because primitive values are copied by value,

* we're telling Javascript and the browser, hey take the value which is stored in name, in this variable, create a copy   of it and store that copy in another user.

* Primitive values store real copy, it won't store reference.

* what are reference values to begin with? Well all other objects or generally, all objects in Javascript. Now technically if you want to be really correct, string numbers and so on are not objects but are dynamically transformed to pseudo objects you could say when you do something like name.length.

* We have used that before in the course but this gives you the length of the name and with the dot notation, it kind of implies that we're accessing a property of name which then in turn implies that name would be an object but it actually isn't, it's a bit more complex than that and we don't need to dive in all the internals here but in the end what happens here is Javascript dynamically transforms a string or a number, so any primitive value to an object temporarily if you use the dot notation on it but other than that, it's a primitive value 

* Now all real objects which always stay objects are handled differently because they're more expensive to create you could say. A real object which you create on your own or which might be built into the browser and exposed to you, like that math object where we also called the random function earlier in the course, well these objects as you can imagine hold a bit more data than just a couple of characters or just a single number, they're more complex to manage and therefore creating them takes longer, occupies more memory and so on.

* Therefore the browser typically stores these in the heap but again, the exact allocation logic is something the browser is responsible for, not you or Javascript but that's what the browser does and important, a variable then only stores a pointer, so the address of that place in memory and not the value itself. For primitive values, a variable really stored the value itself in it, for reference values that's not the case and instead the value is stored somewhere in memory, somewhere in the heap and the variables stores the address of that place in memory, so a pointer at this place a memory you could say. 

* Therefore if you copy a variable which holds a reference value, so if you assign it to a different variable, you only copy the pointer, the reference and not the value itself.

* If you want to a new reference object

```js
let person = {age : 10};

let yetAnotherPerson = {...person}; // this will create a new reference
```
* similar approach for arrays too

```js
let person1 = {age : 10};

let person2 = {age : 10};
person1 === person2 // returns false because both are different reference
```
* As you know if we create const we can't change value let's use const array

```js
const hobbies = ['sports'];

hobbies.push('cooking'); // this will not show any error 

```

* Keep in mind that it's a constant so we can't change the value after it has been created. Well you might expect an error therefore and yet if I hit enter, we get no errors and if I have a look at hobbies, we see cooking was added. So how does that work?

* Well again keep in mind, this array gets created somewhere in memory and what's getting stored in hobbies is the address of that array or of that object, arrays are objects as you learned, so the address is getting stored in this constant. Now when we call push, we certainly do manipulate the data in memory but what do we not manipulate? The address, it's still the same address. The data in memory changed but it's still at the same address, maybe more memory was allocated to it, that's something the browser does but we don't care, we don't care if more memory was allocated, the address is the same. So in this constant, the value never changed because the value is the address and the address didn't change, just the data behind the address but that's not what's getting stored here, therefore here we can change this like this without getting an error.


```js
const hobbies = ['sports'];

hobbies.push('cooking'); // this will not show any error 
hobbies = ['sports1', 'cooking1']; // this will throw an error why ??
```

* Because we used the equal sign here and that means we try to assign a new array. Now of course as you learned, the array will never really be stored in there but what matters is that we do indeed create a brand new array here, which means a brand new place in memory, a brand new address and therefore Javascript tries to store a brand new address in hobbies. So now the value of hobbies would get changed and that indeed is not allowed because it's a constant.

* So using constants with a single equal sign to assign a new value is never allowed but changing the objects which might be stored in them, that is allowed as long as you use something like push.

* Now of course, you can't change primitives stored in constants because these are not stored in memory and there is no address stored in the constant but instead the real value is stored in it but for objects and arrays, this works.

```js
const person = {age : 10};

person.age = 32; // here we used equal but stil will not throw error !! why 

```
* You could argue that I am using the single equality operator, so the assignment operator here but I am using it on age, not on person, right, I'm not saying person is equal to something, that would fail if I set person to a new object, this would fail but I'm setting age inside of the person to a new value, not person itself. So I'm only setting a part of the data stored in memory to a new value just as we did it with the array a second ago and therefore the data in memory certainly changes, the address does not and therefore the address stored in the constant person also does not change.

### Garbage Collection & Memory Management

* The last important concept I want to have a look at here is the concept of garbage collection.

* Now let's ignore the stack for that, the stack is short living memory and as you saw, the stack is cleared automatically because items, function calls, execution contexts are popped off when they're done

* but what about the heap, that long living memory? How is this memory managed? It's important that it's managed because otherwise it might overflow at some time. The operating system of your computer, of your machine of course only allocates a certain amount of memory to Chrome and if that would be exceeded, it would simply kill Chrome at some point of time. Now actually, that will never happen because Chrome also has internal memory management and it will simply kill your website before it occupies too much memory.

* So you want avoid that and the good thing is you don't really need to actively manage the memory unless you're building super complex three dimensional calculation intensive applications 

* but still, there are some things which you should keep in mind and which you should understand, how does Chrome manage this memory?

* Well there is a thing which is called the garbage collector, every Javascript engine has one. So here let's say we're talking about v8 garbage collector which is the engine, the Javascript engine Chrome ships with but of course Firefox and so on, the engines built into these browsers also have a garbage collector.

* Refer: https://v8.dev/blog/free-garbage-collection

* So what does this garbage collector do? In the end, it periodically checks the heap memory for unused objects and unused objects are objects without references.

* Now remember, references are these things, the addresses is in the end which are stored in variables and therefore the garbage collector will go ahead and basically remove, clear all unused objects from the memory, from the heap memory, all the objects where it sees that you certainly won't work with them anymore in your code.

* if you're not using the variable anymore, that will also be picked up by garbage collectors.the Javascript garbage collector can't be triggered by you, you can't force Javascript to go ahead and garbage collect, instead it will run its algorithm

* Refer : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management

* as I mentioned, you generally don't have to do anything for that to happen, you don't have to tell the browser that it should go ahead and do garbage collection for example. It will try to do that in a smart way without interrupting your web page or without slowing it down, without being visible to your users.

* Now you should still be aware of memory leaks, that is when you actually write code where you have a certain object which you don't work with anymore but you still have a reference to it, so you still have some variable or some place in your code where you still hold a reference to that object and even though you're not interested in it anymore, it's still there and there still is a reference to it and therefore it can't be garbage collected because even though you don't want it anymore, you wrote code that doesn't make it clear that you don't want it anymore, so you basically have a bug in your code

* Javascript here is a really smart or the browser and Javascript together and if you add an event listener to a button or to any element with an event to which you listen and a function which you already use before, Javascript will replace that old listener with the new one and not add a brand new one

* and that's a good behavior because otherwise you could easily end up with hundreds of event listeners on the same object and that would probably lead to a behavior in your app which you don't want and introduce memory leaks because you're populating your memory with all these functions and functions in the end also are just kind of objects and they are therefore also stored in the heap in memory and they cluttern up that memory over time.

```js
function addListener() {
  clickableBtn.addEventListener('click', function() {
      const value = messageInput.value;
      console.log(value || 'Clicked me!');
    });
}
```
* So this function is created on the fly so to say, it's not getting executed immediately or anything like that, it behaves just as when we pointed at print message but the difference here and that's a crucial difference is that this function doesn't have a name and it is indeed created as a brand new object whenever this add listener function runs because this function is created here on the fly. Previously, this print message function here was only created once when this entire script ran for the first time, then this code never runs again and therefore only one function object is created, here, we create a new one whenever add listener runs. Well and the behavior we see therefore is that if I enter something here and I add a listener and I click click me, we got the same as before but if I click add a listener multiple times here, we see more and more outputs printed for this test thing here, for click me right.

* So we see more and more outputs being printed, one for every time I clicked the add a listener to the other button and that simply is the case because a new function is now created for every execution of add listener.

* This is dangerous or a potential problem at least because that can lead to memory leaks where you have dozens or hundreds of functional objects in your memory and Javascript is not able to clean them up because every execution of add listener creates a brand new function. To Javascript, this is a brand new object, not the same as before and therefore it thinks OK, you want add a new click listener with the new function than you did before. It doesn't see that the function does the same thing as the old function to Javascript since we create a new function, it is a new function, so that's something you should be careful about.