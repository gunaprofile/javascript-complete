# Javascript Complete

## Data Structures & Algorithms Introduction

### What are "Data Structures" & "Algorithms"?

* algorithm, the sequence of steps we need there to derive this desired output for the given input, so for the algorithm we expect an input of a certain format and that format should always be the same and we know what we want to get as an output,
so the minimum number in this case and then it's up to us to come up with a number of steps that help us get there, that help us find a solution here. That is what we would call an algorithm, that sequence of steps that gives us for a given input a certain desired output.

* the data structure is something like an array or an object or also maps and sets and the algorithm is a sequence of steps to solve a problem, that is what this module is about in the end.

### Example

```js
function getMin(numbers) {
  if (!numbers.length) {
    throw new Error('Should not be an empty array!');
  }
  let currentMinimum = numbers[0];

  for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] < currentMinimum) {
      currentMinimum = numbers[i];
    }
  }

  return currentMinimum;
}

const testNumbers = [3, 1, 2];

const min = getMin(testNumbers);

console.log(min);
```
### Solving the Same Problem Differently

```js
function getMin(numbers) {
  if (!numbers.length) {
    throw new Error('Should not be an empty array!');
  }
  let currentMinimum = numbers[0];

  for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] < currentMinimum) {
      currentMinimum = numbers[i];
    }
  }

  return currentMinimum;
}

function getMin2(numbers) {
  if (!numbers.length) {
    throw new Error('Should not be an empty array!');
  }

  for (let i = 0; i < numbers.length; i++) {
    let outerElement = numbers[i];
    for (let j = i + 1; j < numbers.length; j++) {
      let innerElement = numbers[j];

      if (outerElement > innerElement) {
        // swap
        numbers[i] = innerElement;
        numbers[j] = outerElement;

        outerElement = numbers[i];
        innerElement = numbers[j];
      }
    }
  }

  return numbers[0];
}

const testNumbers = [5, 1, 5];

const min = getMin2(testNumbers);

console.log(min);

```

### Performance & The "Big O" Notation

* When we have multiple available solutions for a given problem, we of course want to find the best one and for this, we need to compare the solutions but how do we compare different algorithms?

* In the end, it typically comes down to the runtime performance. We wonder how long does an operation take and of course, we want to pick the solution which takes the less amount of time, so which is fastest to execute.

* Now the problem just is, of course we could use a stopwatch and measure how long our different algorithms take for different inputs but this would not really be a good idea, it depends way too much on the hardware we're running on, which other processes might be running in the background and for small data sets, for small amounts of data, there might be a lot of variation.

* So indeed we try to measure and express this in a generalized form and not in time units, not in seconds and so on because that would be really hard to measure, instead we use something which is called time complexity

* and there, you could simply imagine a chart where you have two axis and one is the time axis and one is the number of items or the size of the input you're feeding into your algorithm and then you have a certain relation between the number of items you're passing into your algorithm and the time it takes to execute and this is expressed with something which is called the Big O notation

* Refer : DS and Algo

* Now that's very abstract, so let me explain what exactly this means, what the Big O notation is and how we could measure this for our two example algorithms. Back to the two algorithms we wrote, how long does this approach take and how long does this solution take? Well let's start with getMin, how can we find out how long it takes? 

* How often will this if block execute then? Exactly once, this is not inside of a loop, this is just in the function itself, we call that function once no matter how big our array is, so this part here executes exactly once

* like this we can calculate number of execution.

###  More Time Complexities & Comparing Algorithms

```js
function getMin(numbers) { // [3, 1, 2]
  if (!numbers.length) { // 1 execution
    throw new Error('Should not be an empty array!');
  }
  let currentMinimum = numbers[0]; // 1 execution

  for (let i = 1; i < numbers.length; i++) { // 1 execution
    if (numbers[i] < currentMinimum) { // 2 executions
      currentMinimum = numbers[i]; // 1 execution
    }
  }

  return currentMinimum; // 1 execution
}

// T = n => Linear Time Complexity => O(n)

function getMin2(numbers) {
  if (!numbers.length) {
    throw new Error('Should not be an empty array!');
  }

  for (let i = 0; i < numbers.length; i++) {
    let outerElement = numbers[i]; // n times
    for (let j = i + 1; j < numbers.length; j++) {
      let innerElement = numbers[j]; // n times

      if (outerElement > innerElement) {
        // swap
        numbers[i] = innerElement;
        numbers[j] = outerElement;

        outerElement = numbers[i];
        innerElement = numbers[j];
      }
    }
  }

  return numbers[0];
}

// Quadratic Time Complexity => n * n => O(n^2)

const testNumbers = [5, 1, 5];

const min = getMin(testNumbers);

console.log(min);

```
### More on Big O

* isEvenOrOdd
```js
function isEvenOrOdd(number) {
  // const result = number % 2;
  // if (result === 0) {
  //   return 'Even';
  // } else {
  //   return 'Odd';
  // }
  return number % 2 ? 'Odd' : 'Even';
}

 // Constant Time Complexity => O(1)

console.log(isEvenOrOdd(10)); // 'Even'
console.log(isEvenOrOdd(11)); // 'Odd'
```
* sumUp

```js
function sumUp(numbers) {
  let sum = 0; // 1

  for (let i = 0; i < numbers.length; i++) { // n
    sum += numbers[i];
  }

  return sum; // 1
}

// Linear Time Complexity => O(n)

const array = [1, 2, 5];

console.log(sumUp(array)); // 8
```

###  Diving into Data Structures & Time Complexities

```js
const age = [30, 29, 54];
// [30, 29, 54] => [30, 29, 54, _]
// [0, 1, 2, 3] => [0,  1,  2, _]

age.push(60) // Constant Time Complexity => O(1)

// [30, 29, 54] => [_, 30, 29, 54]
// [0, 1, 2, 3] => [0,  1,  2,  3]

age.unshift(10) // Linear Time Complexity => O(n)

const myAge = age[0] // Constant Time Complexity => O(1)

// ----

const namePopularity = [{userName : 'max', usages: 5}, {userName : 'Manu', usages: 15}]

const manuUsages = namePopularity.find(pers => pers.userName === 'manu').usages;

// BEST CASE: constant time complexity => O(1)
// WORST CASE: Linear time complexity => O(n)
// AVERAGE CASE: Linear time complexity => O(n)

const nameMap = {
    'max' : 5,
    'manu' : 6
}

const manuUsages2 = nameMap['manu'] // constant time complexity =>  O(1)
```
### Resources

* The following resources may be helpful.

* More on Algorithms & Data Structures: https://adrianmejia.com/how-you-can-change-the-world-learning-data-structures-algorithms-free-online-course-tutorial/

* Understanding Time Complexity & Big O (incl. Examples): https://adrianmejia.com/most-popular-algorithms-time-complexity-every-programmer-should-know-free-online-tutorial-course/

* More on Data Structures: https://adrianmejia.com/data-structures-time-complexity-for-beginners-arrays-hashmaps-linked-lists-stacks-queues-tutorial/

* A Comprehensive Collection of Examples: https://github.com/trekhleb/javascript-algorithms