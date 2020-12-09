# Javascript Complete

## Basics: Variables, Data Types, Operators & Functions

### Adding JavaScript to the Website

* Script tag should be closed seperately by a closing script tag

* Always add script to the bottom of the page so that it will not block rendering template.

* Order always a matter in javascript so make sure you imported the dependency before we used in next file

### Introducing Variables & Constants

* Refer : Refer Image /variables-constants-overview.pdf

### Declaring & Defining Variables

* Refer : Refer Image / naming-variables.pdf

```js
let currentResult //un initialised variable
```
* Now in Javascript, you don't have to initialize this variable with a value, so you can leave it like this and now it's an uninitialized variable. it's not initialized or defined yet, it has no value yet or no value set by you. So you can leave it like this and sometimes this is just what you need,

* Now in Javascript, using the semicolon is generally optional, I'm saying generally because there are some very rare niche cases where it's not optional but you can avoid these cases typically and hence you can write Javascript code without semicolons

* You can't ommit the semicolon when having two expression in same line

```js
let a = 1; const b = 2;
```
### Working with Variables & Operators

* Refer : basic-operators.pdf

### Understanding the String 

* Refer : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Escape_notation

### String excape 

* Incase if you want to show single quotes you need to wrap single quotes with double quotes.

* Alternative approach instead of that you can use backtick . make sure you used dollar with bracket to get the expression value. this only work with backtick

```js
const defaultResult = 0;
let currentResult = defaultResult;

currentResult = (currentResult + 10) * 3 / 2 - 1;

let calculationDescription = `(${defaultResult} + 10) * 3 / 2 - 1`;

outputResult(currentResult, calculationDescription);
```
* This overall construct here is called a template literal. 

* One convenient thing about template literals is that you can easily write multiline strings there

```js
const defaultResult = 0;
let currentResult = defaultResult;

currentResult = (currentResult + 10) * 3 / 2 - 1;

let calculationDescription = `(${defaultResult} 


+ 10) * 3 / 2 - 1`;

outputResult(currentResult, calculationDescription);
```

* This white space won't be visible in you browser. but it will be visible in browser code.

* If you want to see white space in browser too .. add the below css

```css
white-space : pre;
```

### Introducing Functions

* Refer Image : functions-definition.pdf

* functions - code on demand

* Now when you have a function defined, the interesting thing here is that the code in there doesn't run immediately when your script gets first executed, instead Javascript just sees that function and kind of registers it, stores it in memory.

* you then execute the code in the function by calling that function. 

* Now with functions, we can define code that should run later and we can then take such a function and actually attach it to the buttons here to make sure that only when a button is pressed, the function runs

### Adding A Custom Function

```js
function add(num1, num2) {
    const result = num1 + num2;
    alert('The result is ' + result);
}

add(1, 2);
add(5, 5);
```
* You can place your function anywhere in your file

* Javascript is smart enough to kind of read your entire file before it executes it and it will therefore find any functions you use so that it then automatically pulls these functions to the top so to say

### The (Un)Importance of Code Order

```js
const defaultResult = 0;
let currentResult = defaultResult;

function add(num1, num2) {
    const result = num1 + num2;
    return result;
}

currentResult = add(1, 2);

let calculationDescription = `(${defaultResult} + 10) * 3 / 2 - 1`;

outputResult(currentResult, calculationDescription);
```
* if we move currentResult to down ie declare after we used currentResult

```js
const defaultResult = 0;
// let currentResult = defaultResult;

function add(num1, num2) {
    const result = num1 + num2;
    return result;
}

currentResult = add(1, 2); // here we will get error as can not access 'currentResult' before initialization

let calculationDescription = `(${defaultResult} + 10) * 3 / 2 - 1`;

outputResult(currentResult, calculationDescription);

let currentResult = defaultResult;
```
* We will get error as can not access 'currentResult' before initialization 

*  you need to declare your variables and constants before you use them and declare is the part where you create the variable with let or const.

* Now for functions, that's actually a bit different. I could grab this function and move it to the bottom of this file and if I do this, please keep in mind that I do call it up here, so technically before I define it.

```js
const defaultResult = 0;
let currentResult = defaultResult;

currentResult = add(1, 2);

let calculationDescription = `(${defaultResult} + 10) * 3 / 2 - 1`;

outputResult(currentResult, calculationDescription);

function add(num1, num2) {
    const result = num1 + num2;
    return result;
}
```

* Still if I now reload this page, we get the same result as before and if we have a look into the developer tools, you see no new error here. The reason for that is that for functions, we have a special behavior in Javascript

* What Javascript actually does or what the browser actually does is that when it loads your script , it runs through it from top to bottom, it parses the entire script and actually it doesn't execute it right away, instead it just reads it from top to bottom and will take any functions it finds in there and automatically pull them to the top and be aware of them so to say. So it automatically registers all functions before it then really executes your script and that's just the special behavior Javascript has there.

###  An Introduction to Global & Local Scope

* Local scope is also called as lexical scope

### Shadowed Variables

* You learned about local ("function-internal") variables and global variables.

```js
let userName = 'Max';
function greetUser(name) {
  let userName = name;
  alert(userName);
}
userName = 'Manu';
greetUser('Max');
```
* This will actually show an alert that says 'Max' (NOT 'Manu').

* You might've expected that an error gets thrown because we use and declare userName more than once - and as you learned, that is not allowed.

* It indeed is not allowed on the same level/ in the same scope.

```js
let userName = 'Max';
let userName = 'Manu';
```
* Why does it work in the first code snippet though?  Because we first create a global variable userName via

```js
let userName = 'Max';
```
* But then we never re-declare that on the global level (that would not be allowed).

* We only declare another variable inside of the function. But since variables in functions get their own scope, JavaScript does something which is called "shadowing".

* It creates a new variable on a different scope - this variables does not overwrite or remove the global variable by the way - both co-exist.

* When referring to userName inside of the greetUser function we now always refer to the local, shadowed variable. Only if no such local variable existed, JavaScript would fall back to the global variable.

###  More about the "return" Statement

* You by the way also can return nothing,

```js
function greetUser(name) {
  let userName = name;
  return;
}
```

* you might wonder why you might want to do that, for this function it makes absolutely no sense but you will later encounter scenarios where a function does something, let's say it does some calculation and then sends that with a HTTP request to some server to store it there and then you want to quit function execution, if that condition is true and only continue if it is not true,

* so in such cases you could return from a function and you would return nothing just to cancel function execution. 

### Executing Functions "Indirectly"

```js
const defaultResult = 0;
let currentResult = defaultResult;

function add() {
  currentResult = currentResult + userInput.value;
  outputResult(currentResult, '');
}

addBtn.addEventListener('click', add); // addBtn reference from vendor.js ie document.getElementById('btn-add');
```
* addEventListener - build in browser function, first argument is click event and second argument is function to be called.

* addBtn.addEventListener('click', add()); - if we do like this, it will get executed while script load itself

* we want to tell the Javascript engine, don't execute this function right away, instead keep in mind that this function should be executed when this button is clicked.

* solution for this is don't pass with paranthesis addBtn.addEventListener('click', add);

### "Indirect" vs "Direct" Function Execution - Summary

* It's important to understand why we have these "two ways"!

* In general, you call a function that you defined by using its name (e.g. add) and adding parentheses (with any parameters the function might need - or empty parentheses if no parameters are required like in the above example).

* => add()

* This is how you execute a function from your code. Whenever JavaScript encounters this statement, it goes ahead and runs the code in the function. Period!

* Sometimes however, you don't want to execute the function immediately. You rather want to "tell JavaScript" that it should execute a certain function at some point in the future (e.g. when some event occurs).

* That's when you don't directly call the function but when you instead just provide JavaScript with the name of the function.

* => someButton.addEventListener('click', add);

* This snippet would tell JavaScript: "Hey, when the button is clicked, go ahead and execute add.".

* someButton.addEventListener('click', add()); would be wrong.

* Why? Because JavaScript would encounter that line when it parses/ executes your script and register the event listener AND immediately execute add - because you added parentheses => That means (see above): "Please execute that function!".

* Just writing add somewhere in your code would do nothing by the way:

```js
let someVar = 5;
add
alert('Do something else...');
```
* Why? Because you just throw the name of the function in there but you don't give any other information to JavaScript. It basically doesn't know what to do with that name ("Should I run that when a click occurs? After a certain amount of time? I don't know...") and hence JavaScript kind of ignores this statement.

### Converting Data Types

```js
const defaultResult = 0;
let currentResult = defaultResult;

function add() {
  currentResult = currentResult + parseInt(userInput.value); // parseInt
  outputResult(currentResult, '');
}

addBtn.addEventListener('click', add);
```

### Mixing Numbers & Strings

* You saw the example with a number and a "text number" being added

* 3 + '3' => '33' in JavaScript.

* That happens because the + operator also supports strings (for string concatenation).It's the only arithmetic operator that supports strings though. For example, this will not work:

* 'hi' - 'i' => NaN

* NaN is covered a little later, the core takeaway is that you can't generate a string of 'h' with the above code. Only + supports both strings and numbers.

* Thankfully, JavaScript is pretty smart and therefore is actually able to handle this code: 3 * '3' => 9

* Please note: It yields the number (!) 9, NOT a string '9'!

* Similarly, these operations also all work:

* 3 - '3' => 0

* 3 / '3' => 1

* Just 3 + '3' yields '33' because here JavaScript uses the "I can combine text" mode of the + operator and generates a string instead of a number.

### Splitting Code into Multiple Functions

```js
const defaultResult = 0;
let currentResult = defaultResult;

function getUserNumberInput() {
  return parseInt(usrInput.value);
}

function createAndWriteOutput(operator, resultBeforeCalc, calcNumber) {
  const calcDescription = `${resultBeforeCalc} ${operator} ${calcNumber}`;
  outputResult(currentResult, calcDescription);
}

function add() {
  const enteredNumber = getUserNumberInput();
  const initialResult = currentResult;
  currentResult = currentResult + enteredNumber;
  createAndWriteOutput('+', initialResult, enteredNumber);
}

function subtract() {
  const enteredNumber = getUserNumberInput();
  const initialResult = currentResult;
  currentResult = currentResult - enteredNumber;
  createAndWriteOutput('-', initialResult, enteredNumber);
}

function multiply() {
  const enteredNumber = getUserNumberInput();
  const initialResult = currentResult;
  currentResult = currentResult * enteredNumber;
  createAndWriteOutput('*', initialResult, enteredNumber);
}

function divide() {
  const enteredNumber = getUserNumberInput();
  const initialResult = currentResult;
  currentResult = currentResult / enteredNumber;
  createAndWriteOutput('/', initialResult, enteredNumber);
}

addBtn.addEventListener('click', add);
subtractBtn.addEventListener('click', subtract);
multiplyBtn.addEventListener('click', multiply);
divideBtn.addEventListener('click', divide);
```
* Refer : operators-summary.pdf

* Post increment/decrement just increment/decrement one value the existing value.

* Pre increment also works similar Little side note, you could also write minus minus or plus plus in front of the variable and it will actually have a small impact or a small difference,

* not if you just use it like this to change the value but if you're interested in the return type of this operation, 

* if you add plus plus in front of this, it will actually add it to current result by adding one, just as it does if you add it thereafter

* if you have plus plus or minus minus in front of this variable, then this operation returns the edited value
```js
return ++currentResult
```

* if you have it after the variable, well then this returns the value of the variable before it was changed.

```js
return currentResult++
```

*  So this plus plus and minus minus operator return something, it returns the value before or after the change depending on where you add the pluses and minuses.

* Refer : data-types-summary.pdf

### undefined, null & NaN
* Refer : undefined-null-nan.pdf

* so if you create a variable and you don't assign a value at the beginning with the equal sign, then this variable is undefined.

* We also saw undefined in the log when we try to access an element in the array at an index where no element was created yet, then we tried to access an element which wasn't there and therefore we got back a value of undefined.

* So just like numbers and strings are a data type, undefined is also a data type actually.

* Now you should never assign undefined to a value on your own, so never write equals undefined because that's just a default value you have if something has never been assigned a value.

* null - Now it's very similar to undefined, it means that there is no data but undefined is the default value for variables and so on where you never assigned a new value. Null is never a default value on the other hand, instead you have to set something to null to use that value and that's often used if you want to reset or clear a variable, let's say you have some user input and you want to reset this, you could set it to an empty string but you could also set it to null for example, to make it clear in your program that there currently shouldn't be a value because no value has been entered or whatever.

* And again this is especially useful later if we can also run some checks, some conditional checks with if statements to find out if a value is set and then do something or if it's not set. So undefined and null are important to manage empty data,either because it never was set or because you want to set it to empty, to null.

* They're very similar but they're not entirely equal because of the reasons I explained.

* Now there also is one other special value and that's NaN, that stands for not a number and technically, unlike undefined and null which also are their own types, this is not a type, instead it's of type number, it's not its own type and you can therefore use it in calculations where you work with numbers.

* Now the idea behind NaN is that it's kind of like an error code if you run a calculation with something that doesn't include a number, so if you have let's say a multiplication with text or anything like that, then you would get not a number as a result and if you use the not a number value in that calculation, you also get not a number as a result.

* Now you might wonder why you would ever run an invalid calculation, keep in mind that Javascript allows you to write highly dynamic code. So like in the case of our calculator here, you could have a calculator where users can enter any value, where they are maybe not forced to enter a number and where therefore you could run a calculation with some user input which is not a number and then you could get such a not a number result.

###  Importing Scripts Correctly with "defer" & "async"

* Now to conclude this module, let's come back to something we actually started the module with and that's how we import our scripts into the HTML file.

* I'm doing this here at the end of the body tag, at the end of the body section in the document and I'm doing it here because our scripts rely on the HTML page being rendered. 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Basics</title>
    <!-- <link
      href="https://fonts.googleapis.com/css?family=Roboto:400,700&display=swap"
      rel="stylesheet"
    /> -->
    <link rel="stylesheet" href="assets/styles/app.css" />
  </head>
  <body>
    <header>
      <h1>The Unconventional Calculator</h1>
    </header>

    <section id="calculator">
      <input type="number" id="input-number" />
      <div id="calc-actions">
        <button type="button" id="btn-add">+</button>
        <button type="button" id="btn-subtract">-</button>
        <button type="button" id="btn-multiply">*</button>
        <button type="button" id="btn-divide">/</button>
      </div>
    </section>
    <section id="results">
      <h2 id="current-calculation">0</h2>
      <h2>Result: <span id="current-result">0</span></h2>
    </section>
    <script src="assets/scripts/vendor.js" ></script>
    <script src="assets/scripts/app.js"></script>
  </body>
</html>

```
* we need to run the scripts after the browser parsed and rendered all the HTML code, otherwise the buttons to which we want to establish connections simply don't exist. So that's why I import the scripts down there.

* Now this is still not entirely optimal and let me show you why?

* Let open this file in chrome cognito window  I simply use this incognito window here because this allows me to rule out that what I'm about to show you gets distorted by any extensions or browser plugins.

* So I opened the page here in Chrome, in the incognito window and now we can again open the developer tools which allow us to have a look behind the scenes. chrome developer tool ==> performance tag 

* Now what's the performance tab? The performance tab allows us to get an idea of what the browser does in detail when it renders this page and there are many things you can do with it but it's a great tool for understanding how the scripts are loaded, parsed and executed and what might be the issue here. (Refer : debug1)

* So with the page being used here, press this record button here and then reload the page by using a shortcut for it or using the reload button and then stop recording this.

* Now if you select this, it gets zoomed in here in the middle and bottommost window and there, you see which network requests were sent and what the browser did, so what it parsed, what it executed and so on. Now what we can see relatively quickly is that we have one long going network request which downloads the fonts, this kind of distorts everything here (Refer : debug2)

* so let me comment out this link here which does load the fonts to not have this distraction in here so that we can focus entirely on the scripts.So comment this out in the HTML file so that this is no longer getting used, save the file thereafter and then let's repeat it,

* Now we have a clearer picture of what happened. What happened is that here in that work tab, we first download the index.html file,that's the blue part and thereafter the CSS file and the script files.(Refer : debug3)

* Now let's go to the bottommost window, there we see what the browser did in detail and if we zoom in here, which you can do with the mouse wheel, you see that in the end here we receive a data, that's the downloaded HTML file, if you see, this (Refer : debug4)

* this roughly lines up, here in network tab it's done downloading the file, that's when this receive, data event is triggered when it finished loading it (Refer : debug4)

* Then it starts parsing the HTML code. Now it starts parsing the HTML code and what you can see is that pretty much at the end of that when it's done parsing this, it sends off requests to the Javascript files,

* CSS parsed prior to js because css at the top.Javascript files get requested a bit later because we request them at the bottom of our HTML file.

* Now obviously that's not a huge file, so there's not much time difference between but still we request the Javascript files only after the parsing is done or when it's almost done entirely because we do that at the bottom of the HTML file.

* So that's why we only request the files once we're almost done parsing the HTML document as you can see with this vertical line

* Now what's the implication of that? Of course the benefit is that the scripts only execute after parsing is done but we also see that we start parsing and only when we're done, we download the scripts and only once the scripts are downloaded way over here,

* only after this is done, we actually execute the scripts, these are these yellow blocks you find down there, these are the two scripts which are getting executed.(Refer : debug5)

* now what we effectively see is that we start executing the first script at around 930ms and we're done passing at around 906, 907, so there are roughly around 20 milliseconds of pause between us being done with parsing the HTML file and executing the script.

* Now these are all very small numbers which we can't even feel when we load the page because we have small scripts and we have a small, short HTML file but imagine that our scripts would be longer, that they would take longer to load and execute and that we have way more HTML code that needs to be parsed. 

* Then it's not that great that we wait for all this code to be parsed just to start loading the scripts. We certainly want to execute them after this was parsed because we rely on the parsed content, so that's fine,

* we don't want to execute the scripts earlier but loading them earlier, downloading them from the server earlier, that would be a good idea.

* Also keep in mind that all the loading is very fast here of course because we're doing this all locally, we're just accessing the file here, there is no web server involved, here is no latency, so this would be slower if we were really serving this webpage and hence we have the ideal scenario here and yet we have this unnecessary delay and we certainly want to shrink that delay if we think about really hosting this on a web server. We don't want to start loading the scripts once everything was parsed, we want to load the scripts as early as possible and then still only execute them after everything was parsed so we want to get the best of both worlds.

* Now of course we can grab the scripts and move them into head section, maybe here below the stylesheet. Now if we do that and I clear here, this start recording and reload and stop, 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Basics</title>
    <link
      href="https://fonts.googleapis.com/css?family=Roboto:400,700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="assets/styles/app.css" />
    <script src="assets/scripts/vendor.js" ></script>
    <script src="assets/scripts/app.js" ></script>
  </head>
  <body>
    <header>
      <h1>The Unconventional Calculator</h1>
    </header>

    <section id="calculator">
      <input type="number" id="input-number" />
      <div id="calc-actions">
        <button type="button" id="btn-add">+</button>
        <button type="button" id="btn-subtract">-</button>
        <button type="button" id="btn-multiply">*</button>
        <button type="button" id="btn-divide">/</button>
      </div>
    </section>
    <section id="results">
      <h2 id="current-calculation">0</h2>
      <h2>Result: <span id="current-result">0</span></h2>
    </section>
  </body>
</html>

```

*  we get a better picture here. If I zoom in, here we download the HTML file and now what we see is that we start parsing HTML. Now we then fetch the styles and the scripts and we pause parsing here as you can see, we pause that, we only pick it up back here when basically downloading the scripts finished, we also executed the scripts here.

* Now this looks a bit strange, looks like downloaded app.js for longer than it actually took because we do execute it here
so this is a bit distorted.

* The main takeaway here is that we start parsing HTML, then we encounter the script imports, we then download the scripts and pause parsing therefore, this blocks parsing and then we execute the scripts and only thereafter we continue parsing.

* Now that's of course bad and this also causes an error because now we try to interact with the buttons on the webpage without those being ready, so that's also not ideal.

* We download the scripts earlier which is great but we now also execute them too early. The solution is an extra attribute which we can add to the script and that's the defer attribute.

```html
<script src="assets/scripts/vendor.js" defer></script>
<script src="assets/scripts/app.js" defer></script>
```
* You add it like this, without a special value, just like this to both script tags and defer tells the browser that it should download these scripts right away but that it should not block parsing HTML so that it instead should continue with parsing HTML and only execute the scripts after everything has been parsed.

* So it starts downloading early but it doesn't execute the scripts right away once they've finished downloading, instead it guarantees us that it only executes the scripts once they were downloaded and once parsing HTML finished.

* So now, the script downloading and execution doesn't block the HTML code from being parsed and rendered and instead, that continues and the only thing that does change is that we download the scripts earlier which is great.

* you can see that all this parsing and script execution is done roughly 20 milliseconds after we started parsing the HTML document, so that is faster than what we saw previously when we started loading and then executing the scripts at the bottom of the body tag.

* Now sometimes, you also have scripts which you want to load early but which you then also want to execute early because maybe they don't rely on the HTML code, you don't establish a connection to it and therefore you don't care whether parsing HTML finished or not. This can also be achieved by using the async keyword instead of the defer keyword.

```html
<script src="assets/scripts/vendor.js" async></script>
<script src="assets/scripts/app.js" async></script>
```

* Async works a bit like defer, it tells the browser to start loading the scripts as early as possible and then the browser is not blocked but instead continues parsing HTML but the difference to defer is that with async, the script then executes right away once it was downloaded. So it does not wait for all the HTML code to be parsed, instead it executes as early as possible. So then parsing of HTML will be paused until it is executed and only thereafter it will pick up again. 

* One other important difference to be aware of is that with async, a script really executes as early as possible, so as soon as it was downloaded. The order of the script execution is therefore not guaranteed, the app.js script could execute before the vendor.js script if it is loaded earlier. 

* With defer on the other side, the browser guarantees the order. So even if app.js would be downloaded faster, vendor.js would still execute before app.js, so the order is guaranteed with defer, it's not with async,

* Also note that defer and async are only available if you have an external script, if you have an inline script, so a script which you write right in here, in your HTML file, if you have something like this, defer and async is ignored because what would that do, there is no file to download, this is embedded in the HTML file so this is available once the HTML file was downloaded, so therefore defer and async doesn't make sense here because there is nothing that would need to be downloaded.

* Such scripts are always executed immediately and therefore if those scripts which you embed here rely on the HTML code, you'll always have to move them to the end of the body section but in general it's not a good idea to have important or longer scripts here in your HTML file, you should always use external files for that to keep your HTML files small and focused and don't bloat it with a lot of scripts.

* Refer : import-javascript-summary.pdf

* MDN => JavaScript Basics: https://developer.mozilla.org/en-US/docs/Web/JavaScript

* MDN => Variables: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Variables

* MDN => Operators: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Math

* MDN => Functions: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions

* MDN => Arrays: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Arrays

* MDN => Objects: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics