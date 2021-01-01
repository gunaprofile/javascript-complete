# Javascript Complete

## More on Numbers & Strings

### How Numbers Work & Behave in JavaScript

* Refer : https://2ality.com/2012/04/number-encoding.html

* Refer : https://stackoverflow.com/questions/11695618/dealing-with-float-precision-in-javascript

* Refer toFixed, toString

### The BigInt Type

* So that was a lot about precision and numbers but it's important to understand it. In modern versions of Javascript, there actually also is an alternative to the normal number we worked with all the time and that's a big integer.

* Now the big integer type is a primitive value and its goal is to allow you represent numbers that are above the maximum safe integer we learned about, so a number bigger than that. If you for example wanted to represent a number bigger than that by adding extra nine at the end, you would see what I explained before, we get a totally different number because Javascript only works with 64 bits and we would exceed that and therefore Javascript cuts the number at a certain point and shows a different number.

```js
Number.MAX_SAFE_INTEGER 
9007199254740991
9007199254740991n
9007199254740991n
```
* this keeps its precision, its value and we can represent arbitrarily big numbers with that because internally, this is managed differently, not as a 64 bit floating point number but instead in the end as a string and Javascript does all the heavy lifting of converting this around when you use it in calculations and so on. So the big int can be useful if you're working with very large numbers, of course not just positive ones but also negative ones.Now be aware that there is no floating point calculation there, that there are no supported decimal places, you see I'm getting an error if I try to add a decimal place, it's called big int because it's only about integers and it's the perfect type if you are working with very large integers. 

* Of course you can always represent smaller numbers with that, the question is if this makes a lot of sense and then you can also perform typical calculations with it, just important, you can't mix big int and other numbers, for example 10n and -4, where 10n is a big int but 4 is a normal number would yield an error,

* you see you cannot mix big int and other types, use explicit conversions so you would have to convert this to a big integer or this to a normal number before mixing it. 

* You can convert it to a normal number with parseInt for example,

```js
parseInt(10n) - 4
or
10n - BigInt(4)
```
* this is how you can convert numbers back and forth and this is how you then can work with them and how you can run various calculations. 

* One important note, if you divide a big int, since decimal places can't be represented there, Javascript will actually omit them and we don't see it here because this can be divided with a perfect result without decimal places

* but if we have 5n divided by 2n, then you see we're not getting 2.5n because this does not exist as you see, instead we get 2n because Javascript simply cuts off the decimal place

### The Global "Number" and "Math" Objects

* Let's have a look at the number and the math global object. We used some of the features of these objects already and now I just want to have a second look and walk you through the important ones,
let's start with number.

* There we saw MAX_SAFE_INTEGER, MAX_VALUE, MIN_SAFE_INTEGER and MIN_VALUE, you also have negative and positive infinity. These are special values in Javascript which you also can just type like this
by the way, infinity and -infinity

```js
1/0 // Infinity
```

* and this is the value you for example get if you divide by zero, you'll not get an error which you might expect, instead you get infinity because it basically approximates the result you could say.

```js
Number.isFinite(10) // true
Number.isFinite(Infinity) // false
```
* So infinity is a special value you can get and you can also check for infinity in places where you need true or false, for that you've got the isFinite method,

* So number isFinite can be useful, we also have isNaN there, we saw that earlier already, also exists as a global method. We got parseFloat and parseInt here which also exists globally though and therefore we don't really need that here

* More interesting than that is the math object,there you get various methods and constants or properties that help you with mathematical operations.

```js
Math.E
2.718281828459045
Math.PI
3.141592653589793
Math.abs(-5)
5
Math.random()
0.9689793335561592
Math.random()
0.9315767268045132
```
### Example: Generate Random Number Between Min/ Max

```js
function randomIntBetween(min, max) {
  // min: 5, max: 10
  return Math.floor(Math.random() * (max - min + 1) + min); // 10.999999999999 => 10
}

console.log(randomIntBetween(1, 10));
```
### Exploring String Methods

* I want to point your attention to something which you already know actually, strings of course can be created in three different ways - double quotes, single quotes and back ticks, back ticks create a string with that template literal notation where you can dynamically inject dynamic values like this

```js
`${1}` // "1"
'hello'.toUpperCase(); // HELLO

```
* Refer : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String   

### Tagged Templates

```js
function productDescription(strings, productName, productPrice) {
  console.log(strings);  // ["This product (", ") is ", "."]
  console.log(productName); // JavaScript Course
  console.log(productPrice); // 29.99
  let priceCategory = 'pretty cheap regarding its price';
  if (productPrice > 20) {
    priceCategory = 'fairly priced';
  }
  // return `${strings[0]}${productName}${strings[1]}${priceCategory}${
  //   strings[2]
  // }`;
  return {name: productName, price: productPrice};
}

const prodName = 'JavaScript Course';
const prodPrice = 29.99;

const productOutput = productDescription`This product (${prodName}) is ${prodPrice}.`; // here we called productDescription function with 3 different parameters
console.log(productOutput);
```

* in this function and Javascript simply goes through your template literal, takes all non-dynamic parts and puts them into this first argument which is an array and then takes the dynamic parts and adds them in the right order as argument values to this function. 

* the obvious question is, why would we use that? Where can this be useful? 

* Tagged templates can be useful if you have a scenario where you conveniently want to create some output, could be a string but could be something totally different as well, based on some string input, for example you could be using that to take some input here, some input text and convert this to a different text where you replace some values, for example here we could check if product price is greater than 20 and if it is, then we set some variable which I introduced in this function, price category maybe, to cheap here and if the price is greater than 20, we set price category to fair.

###  Introducing Regular Expressions ("RegEx")

*   Refer : https://www.youtube.com/watch?v=0LKdKixl5Ug&list=PL55RiY5tL51ryV3MhCbH8bLl7O_RZGUUE

* you're having an input and there, you get some user input and that should be an email address, something like test@test.com,

* Regular expressions don't just exist in Javascript, they exist in most programming languages and they help you search for patterns in strings.

* Now you create a regular expression in Javascript in one of two ways, you can create it with the new regex constructor and then to this constructor, you pass a string which describes the pattern you want to look for and there is a rich regular expression language and syntax you can use here or you use a literal notation for creating that regex object, just like you can use square brackets to create an array object, you use two forward slashes like this and then in between these slashes, you define your pattern and a simple email validation pattern could look something like this

* Regular expression are case sensitive