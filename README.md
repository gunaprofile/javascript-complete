# Javascript Complete

##  More on Arrays & Iterables

### What are "Iterables" and "Array-like Objects"?

* Refer : iterables-array-like.pdf

* So what's an iterable in Javascript? An iterable can be defined technically, there it's basically an object or any objects that implement the iterable protocol and have to is @@iterator method with the symbol iterator property. Now symbols are a special type of values in Javascript

* human readable definition of an iterable in the end is that we can use a for/of loop on it. Yes, it's indeed as simple as that, we can loop through it with for/of.

* Now the important thing is that not every iterable in Javascript is an array. We work with arrays thus far but there also are other iterable objects, like a node list for example which we saw in the last course module but also strings and also maps and sets which we'll have a look at later.

* Now there also is another term which I already used and that's array-like and that's actually not the same as an iterable,both iterable and array-like are official terms in the Javascript language.

* Now what are array-like objects therefore? Again, the technical definition is here, much more readable, that an array-like object are in the end any objects that have a length property and use indexes to access items, it's as simple as that and I'd say it is also pretty clear to us humans.

* Now just as with iterables, not every array-like object is an array, that's why it's called array-like. 

* We have array-like objects, like node list and string. Node lists and strings are objects that have a length, that have indexes and where we can use for/of and still, they're not real arrays because as you will learn in this module, real arrays have a couple of interesting behaviors and also a bunch of important methods available to them which do not exist on these array-like or iterable objects.

### Creating Arrays

```js
const numbers = [1, 2, 3];
console.log(numbers);

// const moreArray = new Array; // []
// console.log(moreArray);

// const moreNumbers = Array(5, 2); // [5,2]
// console.log(moreNumbers);

const moreNumbers2 = Array(5);  // This is essentially an empty array with a fixed size, with a fixed length of 5
console.log(moreNumbers2);

// const yetMoreNumbers = Array.of(1, 2); //[1,2] This is a special method on this globally available array object, Now again, you should use square brackets, this will be slower from a performance perspective than that.
// console.log(yetMoreNumbers);

const listItems = document.querySelectorAll('li');
console.log(listItems); // NodeList array like structure

const arrayListItems = Array.from(listItems);
console.log(arrayListItems);
```

* Refer array1 - Now these are not all totally different methods as you see but actually, how these methods create arrays also sometimes depends on the kind of data you pass to that method.

* So let's have a look at the different ways of creating arrays before we then work with them and for that of course, we'll not just have a look at that, we'll also see how that behaves and when you might want to use which.

* array.from is special. It does not take multiple numbers like this, if I would try to do that, if I would try to console log this here with multiple arguments passed in, you see I get an error because I must not pass in multiple arguments here,
instead this takes an iterable or an array-like object and that's the interesting thing. Array.from in the end allows you to convert an iterable or an array-like object which isn't an array yet to an array.

```js
// const arrayItems = Array.from(1,2,3,4); // this will throw error
// console.log(arrayItems);

const moreNumbers = Array.from("Hi!"); // ["H","i","!"]
console.log(moreNumbers);
```
###  Which Data Can You Store In Arrays?

```js
const hobbies = ['Cooking', 'Sports'];
const personalData = [30, 'Max', {moreDetail: []}];

const analyticsData = [[1, 1.6], [-5.4, 2.1]];

for (const data of analyticsData) {
  for (const dataPoint of data) {
    console.log(dataPoint);
  }
}

console.log(personalData[1]);
```

### push(), pop(), unshift(), shift() - Adding & Removing Elements

```js
const hobbies = ['Sports', 'Cooking'];
hobbies.push('Reading');
hobbies.unshift('Coding');
const poppedValue = hobbies.pop();
hobbies.shift();
console.log(hobbies);
```
* push -> always adds new elements at the end of the array.

* unshift -> always adds new elements at the beginning of the array.

* pop -> Remove last elements

* shift -> Remove First element

* Now what if you need to add items or manipulate items in different places of an array? Well for that, you can use the direct index access.

```js
hobbies[1] = 'CODING'
```

* If we would want to insert an element between sports and cooking, there is a different method,the splice method that we will see in sometimes.

* Before we have a look at that, let's have a look at another case, what if we actually target an index which isn't set, like 5? This array only has two elements after all these operations, so 5 actually targets the 6th element which it doesn't have.

```js
hobbies[5] = 'reading' // Rarely used...
```
* we log that to the console, what we'll see is that here we got sports and coding and then at index five, reading and we get three empty slots in between. 

```js
console.log(hobbies[4]); // undefined  - you get undefined here because nothing could be found in there in the end so that's also nice to know.
```
###  The splice() Method

* splice method that would help us insert elements between two elements,by the way is one of the methods which is only available on real arrays, not on iterables, not on array-like objects. That might be one reason why you convert an array-like or iterable object to a real array with array.from because then on that real array, you can use splice.

* Now how does splice work? It takes at least two arguments there but there also is another version if you click on that down arrow here which takes more arguments. So there are two different versions of that function.

```js
hobbies.splice(1, 0, 'Good Food'); // hobbies.splice(starting index, number of items to remove, item to be added,...)
//.splice(start, delete, insert more than one comma separated);
console.log(hobbies);

const removedElements = hobbies.splice(-2, 1);
console.log(hobbies);
```

* Now in that version here, you specify a start index so that's zero based and then the amount of items you want to delete from that index on.

* Now we don't want to delete anything here right, so maybe we start with index zero for item 1 and then we add zero because I don't want to delete anything, instead and that's now where the second version comes into play, we can add more arguments and as soon as you add another comma, this automatically jumps to that other definition, now you can add as many other arguments as you want which will be items that are inserted in the place of these deleted values here.

```js
hobbies.splice(0);// [] -> empty array
```

* if you just specify splice like this by the way here, you will see that if I reload, the array is empty. So splice without an item count will delete all items from that index on, so for index 0, it will delete all items in the array,

* if you want to delete everything up from a specific element, you would take that index of that specific element and therefore now delete everything past

```js
hobbies.splice(2);// removes everything past index 2
```
* Now splice will also return something, it returns the removed elements so that they're not lost.

* splice also works with a negative index here, you can specify a -1 or -2 and what it will then do is it will go to the end of the array and look from the right,

```js
hobbies.splice(-2, 1); // look from the right
```
* so if I specify -1, it will basically go to the last element of this array and then delete one element

### Selecting Ranges & Creating Copies with slice()

* Now another method you but you can use which has a similar name but does something totally different is slice, so not splice but slice.

* The interesting thing slice does here though is it returns a brand new array and therefore this actually is also a nice way of copying an array, you remember? Arrays are in the end objects and therefore reference values.

*  So if you compare an array to an array that looks totally equal to us humans, it will return false if it's not exactly the same object.


```js
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];
// const storedResults = testResults // same array reference if you push to testResults will affect storedResults also
const storedResults = testResults.slice(2); // new array even its copied from testResults, alternation of testResults won't affect storedResults since slice create a brand new array

testResults.push(5.91);

console.log(storedResults, testResults);
```
* but what if you want to select two elements at the same time? So you want to select a part of your array, not the entire array but a part. Well slice helps you with that, you can use it like this and it will give you the full array but you can also specify a start and an end number and these are indexes of the array, start is included end is not

* start is included end is not !!!!

* Though what you can use are negative indexes but then both have to be negative. So you can start at the third last element, this has negative index 1 because there is no negative 0,

* so in the negative indexes the first element from the end has index 1, negative index 2, negative index 3 and then going up to 2, 

```js
const storedResults = testResults.slice(-3,-1); // 10.99, -5

const storedResults = testResults.slice(2); // 1.5, 10.99, -5, 10
```
* You can also just specify a single index here, negative or not, doesn't matter and not specify a second argument and this will then start at this index, so in this case at the element with index 2 which is of course this element and then select everything up from this element all the way to the end,

### Adding Arrays to Arrays with concat()

* There also is a useful method for adding elements to an array and returning a brand new array which again can be useful in situations where you want to create a copy of an array, maybe after adding new elements to it, The concat method allows you to concatenate, so to add elements at the end of an array.

```js
const testResults = [1, 5.3, 1.5, 10.99, -5, 10];

const storedResults = testResults.concat([3.99, 2]);

testResults.push(5.91);

console.log(storedResults, testResults);
```

* Now therefore it's of course a bit like push, with push you can also add items to an array, by the way also more than one item, they will be added at the end but concat actually here takes an array or multiple arrays but not individual numbers or items in general but arrays, one or more arrays and combines these arrays with this array.

* concat on the other hand will pull out all elements of the array you are passing here and add them as new elements to the existing array and it will then also return a brand new array. So it will create a copy of the array, add these items and return that copied brand new array, hence creating a new place in memory and a new address.

* concat returned a brand new array.

### Retrieving Indexes with indexOf() /& lastIndexOf()

```js
console.log(testResults.indexOf(1.5)); // Now as the name suggests, this returns the index of the value you're passing as an argument here.

console.log(testResults.indexOf(1.5, 2)); // you also have an optional second argument that would allow you to specify a starting index so that you for example only start searching on elements with index 2 or higher.

// there are a couple of important things to understand, for one if you have the same value more than once,this will stop after it found the first matching value

console.log(testResults.lastIndexOf(1.5)); // you can use that to search from the right 

const personData = [{ name: 'Max' }, { name: 'Manuel' }];
console.log(personData.indexOf({ name: 'Manuel' })); // -1
```
*  Another important gotcha regarding index of and last index of is that it works fine for primitive values but not for reference values.

* If we do this and I reload my page, we'll get -1 though, -1 is the return value of index of and last index of if it couldn't find any entry, then it always returns -1. So it doesn't throw an error,it doesn't return false or anything like that, it returns -1 if it didn't find anything.

* Objects are reference values and therefore in the end here, I'm creating a brand new object and I'm passing this to index of and behind the scenes, index of is of course comparing all values to the value I passed to index of and because two objects even if they look similar are never similar, it doesn't find any match and therefore it returns -1.

### Finding Stuff: find() and findIndex()

```js
const personData = [{ name: 'Max' }, { name: 'Manuel' }];
console.log(personData.indexOf({ name: 'Manuel' }));

const manuel = personData.find((person, idx, persons) => { // (single element of array, index, fullArray)
  return person.name === 'Manuel';
});
// find work with the same reference array
manuel.name = 'Anna'; // if we change here this will affect personData also

console.log(manuel, personData);

const maxIndex = personData.findIndex((person, idx, persons) => { // (single element of array, index, fullArray)
  return person.name === 'Max';
});

console.log(maxIndex);
```
* There also is another useful method for finding out whether is part of an array or not and that's the includes method though it's also most useful for primitive values because it also just checks values like index of does.

```js
console.log(testResults.includes(1.5)); // This will then return true or false 
```
* includes is a great choice if you're not interested in the index and also not interested in the value but just want to know whether it's part of the array or not. Though it's important to keep in mind that index of will return -1 if something wasn't found.

###  Alternative to for Loops: The forEach() Method

```js
const prices = [10.99, 5.99, 3.99, 6.59];
const tax = 0.19;
const taxAdjustedPrices = [];

// for (const price of prices) {
//   taxAdjustedPrices.push(price * (1 + tax));
// }

prices.forEach((price, idx, prices) => { // (single element of array, index, fullArray)
  const priceObj = { index: idx, taxAdjPrice: price * (1 + tax) };
  taxAdjustedPrices.push(priceObj);
});

console.log(taxAdjustedPrices);
```
### Transforming Data with map()

* now instead of ForEach, we can use map, another special method available on arrays. Now map has the job of taking an array, running a function which has this form on every item in that array and then and that's important, that function should now return a new element for every element in that array, a possibly transformed element.

```js
const prices = [10.99, 5.99, 3.99, 6.59];
const tax = 0.19;

const taxAdjustedPrices = prices.map((price, idx, prices) => {
  const priceObj = { index: idx, taxAdjPrice: price * (1 + tax) };
  return priceObj;
});

console.log(prices, taxAdjustedPrices);
```
* For map, this function which you pass to it has to do something, has to return something, it has to return the map that transformed the new element for that array.

### sort()ing and reverse()ing

* Well the thing is sort by default converts everything to a string and then it's simply sorts this in a string logic and there, one is smaller than three for example, this is how it sorts it, For strings, only the first character is compared by default, hence it's not "10" > "3". but "1"< "3".

```js
// sort logic
const sortedPrices = prices.sort((a, b) => {
  if (a > b) {
    return 1;
  } else if (a === b) {
    return 0;
  } else {
    return -1;
  }
});

// reverse logic
const sortedPrices = prices.sort((a, b) => {
  if (a > b) {
    return -1;
  } else if (a === b) {
    return 0;
  } else {
    return 1;
  }
});

// console.log(sortedPrices.reverse());
```

### Filtering Arrays with filter()

```js
// const filteredArray = prices.filter((price, idx, prices) => { 
//   return price  > 6;
// });

const filteredArray = prices.filter(p => p > 6);

console.log(filteredArray);
```
###  The Important reduce() Method

* So let's assume here with your prices, let's say the unfiltered, unchanged prices, you want to sum them up. Now of course what you can do is you can create for loop or you can use ForEach and you could create a sum here, let sum, set it to equal
and then you could go through your prices, let's say with ForEach where you have the value of each price and then here in the end you say sum plus equal price, right? And if you do that and you console log the sum thereafter, we should have the sum of all prices. So that's not too much code and if I reload this, this probably is to sum.

* Now of course that's not too difficult or not too problematic here, it's a quite short code snippet but we can actually write this with less code.

```js
const sum = prices.reduce((prevValue, curValue) => prevValue + curValue, 0); 
// prices.reduce((prevValue, curValue, currentIndex, prices) => prevValue + curValue, default initial value);
console.log(sum);
```
* Reduce, as the name suggests, reduces an array to a simpler value, for example it can reduce an array of numbers to the sum of these numbers. Of course that's not the only kind of reduction it can do, you can reduce any array to any value you need, so the idea always is that you reduce an array to a simpler value. Typically an array to a single number or a single string or whatever it is. Now the second argument you pass to reduce is the initial value with which you want to start,

### Chaining Methods in JavaScript

* With all these useful array methods you learned about, it's important to understand how you can combine them. Let's take map() and reduce() as an example:

```js
const originalArray = [{price: 10.99}, {price: 5.99}, {price: 29.99}];
const transformedArray = originalArray.map(obj => obj.price); // produces [10.99, 5.99, 29.99]
const sum = transformedArray.reduce((sumVal, curVal) => sumVal + curVal, 0); // => 46.97
```
* Of course, you could skip the map step and just add the extraction logic to reduce():

```js
const originalArray = [{price: 10.99}, {price: 5.99}, {price: 29.99}];
const sum = originalArray.reduce((sumVal, curVal) => sumVal + curVal.price, 0); // => 46.97
```
* But let's say you have a more complex extraction logic and hence want to split this into multiple method calls. Or you have a re-usable map function which you want to be able to use in different places of your app. Then you can still write the initial example in a more concise way if you leverage method chaining:

```js
const originalArray = [{price: 10.99}, {price: 5.99}, {price: 29.99}];
const sum = originalArray.map(obj => obj.price)
    .reduce((sumVal, curVal) => sumVal + curVal, 0); // => 46.97
```
* We call .reduce() directly on the result of map() (which produces an array, that's why this is possible). Hence we can avoid storing the mapped array in a separate constant or variable that we might not need in any other place.

### Arrays & Strings - split() and join()

```js
const data = 'new york;10.99;2000';

const transformedData = data.split(';');
transformedData[1] = +transformedData[1];
console.log(transformedData);

const nameFragements = ['Max', 'Schwarz'];
const name = nameFragements.join(' ');
console.log(name);
```
### The Spread Operator (...)

```js
const copiedNameFragments = [...nameFragments];
const persons = [{ name: 'Max', age: 30 }, { name: 'Manuel', age: 31 }];

const copiedPersons = [...persons]; // you
persons[0].age = 31 ; // this change will overwrite copiedPersons also..

// if you want a new reference array ... do like this
const copiedPersons = persons.map(person => ({
  name: person.name,
  age: person.age
}));

console.log(Math.min(...prices));
```
### Understanding Array Destructuring

```js
const nameData = ['Max', 'Schwarz', 'Mr', 30];

const [ firstName, lastName, ...otherInformation ] = nameData;
console.log(firstName, lastName, otherInformation); // 'Max', 'Schwarz', ['Mr', 30]
```

### Maps & Sets - Overview

* Refer : array2

* Let's create a new set of let's say IDs because that is a super example, IDs should be unique and therefore you might want to store them in a data structure where you can't have any duplicates.

```js
// const ids = new Set(); // empty set

// const ids = new Set([1,2,3]); // if you try to access with index it will throw undefined.
const ids = new Set(['Hi', 'from', 'set!']);
ids.add(2);
if (ids.has('Hi')) {
  ids.delete('Hi');
}
```
* and you can alternatively also initialize it by passing in an existing iterable, any iterable goes. So it can be an array, can be another set, can be a node list.

* to retrieve a value from the set, you can use one of these specific set methods which you see if you type a dot here and then you get this auto completion. You got add to add a new entry, clear to clear all entries, delete to delete a single entry entries which we'll have a look at in a second.

* Now what you don't have here is a method to get a value, there is no get method or anything like that.

* Well if you think about a set, since every value can only be stored once there, you typically don't want to retrieve a value from there, instead you can check if it has a certain value, if one is stored in there and if it is, well in your subsequent code that depends on this existence in the set, you can just continue with one, you can just use this value of one because you know it's in the set.

* this is how you work with a set, it's a data storage that basically tells you whether it contains something or not. 

* if you add a duplicate value lets say ids.add(2);, even after try to add a duplicate entry only one entry will be there in the set.

* So if I now check if it has two and I reload, I get true

* Now if you would want to go through all elements in a set, you can do that with ids.entries. ids.entries, entries is a method which you can execute and it returns, as you can see, an iterable, which means you can use it in a for loop,

```js
for (const entry of ids.entries()) {
  console.log(entry); // this will console values twice
}
```
* why would you have it twice? Why would it give you these arrays where you have the same value twice? Well this is there to be in line with the entries method on the map which we'll also later see where you also get two values but there, they will be different.

* Alternatively use values() instead of entries(). this returns an iterable that only yields the set values once.

```js
for (const value of ids.values()) {
  console.log(value); 
}
```

* While delete if you try to delete a value of set which is not the part of the set you won't get any errors its just ignored

* So to summarize, sets are a data structure or are data structures which help you manage unique values and in some cases, that can be useful. Now as I mentioned earlier already, the array is the most important data structure, so you will probably not use sets everywhere but if you have an application, a more complex application where you want to manage let's say ID which are already in use by logged in users, then you could use a set to keep track of these IDs and you might want to use a set instead of an array because you want to ensure that a single ID can't be part of the set more than once so that it doesn't get too big, doesn't consume too much memory or leads to other logical errors you might have in your code. So sets are great if you need uniqueness amongst your data.

### Working with Maps

* With a map where you stored the extra information, the object itself can be the key and therefore you don't have to add any ID or extract that or create arbitrary string keys, instead you can use the object as a key and retrieve values by just using that object as a key and hence if you save that and you reload here, you indeed get this object output there, so this works just fine.

```js
const person1 = { name: 'Max' };
const person2 = { name: 'Manuel' };

const personData = new Map([[person1, [{ date: 'yesterday', price: 10 }]]]); // So here you could have key and then some value and the key can be of any kind, doesn't have to be a string, can be a number, can be another object and values can also be of any kind.

personData.set(person2, [{ date: 'two weeks ago', price: 100 }]); // set person2 data

console.log(personData);
console.log(personData.get(person1)); // get data

for (const [key, value] of personData.entries()) {
  console.log(key, value);
}

for (const key of personData.keys()) {
  console.log(key);
}

for (const value of personData.values()) {
  console.log(value);
}

console.log(personData.size);
```

* we log an array essentially with always exactly two elements, where the first element is the key and the second element is the value. So for a map, it makes sense that this is an array of exactly two elements because we can then have key and value combined in one value, in one array here.

* Refer : maps-sets-objects.pdf

### Understanding WeakSet

* WeakSet similar like normal set but way less methods available like add ,delete and has. There is no method to get all the entries for example and the reason for that is that weak set,internally works such that it can only store objects so that it can actually clear these objects for you, release them to garbage collection if you don't work with a certain piece of data anymore.

* Conveniently if we console log persons here, the browser dev tools console still allows me to look inside of that

```js
let person = {name: 'Max'};
const persons = new WeakSet();
persons.add(person);
person = null 
```
* Now you might wonder, why would I need this strange thing? Well if you have an application where you store data, let's say in a set, object data in a set or other arrays, where you eventually will let go of data, then you want to make sure that this data can be garbage collected. Now if we work with that person here let's say and then we do some operations with it and we needed it in the set for these operations and we still need the set thereafter, maybe for other operations but this person is not really required anymore, then we can set person equal to null. This means we set this variable to null, so this object no longer has anyone who is interested in it. This person variable, which stored its address is now reset to null, so the address is released from there and the Javascript engine then is able to pick this up and eventually clear this object from the heap, that's what garbage collection does, it clears that data from memory.

* Now if you would use a normal set here which you can normally, then the person you created here would still be part of that set. So even if you cleared your variable here, you added that object, that pointer at this object to this set and Javascript thankfully detects that and will not clear the object because the set still holds a reference to it and that's good because Javascript would expect that you plan on working with that and deleting it therefore would be bad. Now let's say you eventually never end up working with that person again, then with a normal set, you have that unnecessary data stored in memory because you have that data in a set even though you don't need it anymore.

* Sure, you could have done persons delete and deleted that value but if you forgot that, well there you go. Now with weak set, if you reset all other places where you point that this object, the weak set will not hold onto it. So the weak set allows garbage collection to delete items that are part of the set as long as no other part of your code uses these items,


* So weak set is an interesting option in cases where you store object data in a set and you eventually release some of that data and you want to make sure that this thing can be garbage collected. Obviously super advanced, not something you'll use in many applications, not something we need here in the majority of this course to be honest but something to be aware of, especially as you grow as a developer and you will encounter more and more advanced programs, advanced code snippets where something like that might get used.

###  Understanding WeakMap

* Now with weak map, we have a similar idea as with weak set.

* if the map is a normal map and we have person as a key there, the map will hold onto it and not release it for garbage collection, might be what you want but if you know you don't need that, you could use a weak map instead.

* Now on a weak map, you can of course also call a couple of methods, way less than before because again you can't loop through the entries because the entries, the amount of entries and so on is simply not guaranteed which is why the size property is also not available. Javascript can't really tell what's in the map because it can look at it if you ask it with has and get if something is in there but other than that, it doesn't know if something has been released yet

* and now here, keys have to be objects so that this garbage collection thing makes sense and any value, the value does not have to be an object, that could just be a string or whatever you want here.

```js
let person = {name: 'Max'};
const personData = new WeakMap();
personData.set(person, 'Extra info!');

console.log(personData);

person = null // Now with that same logic as before, as soon as we let go of that person object in all other places, like here where I set my variable to null 

console.log(personData); // still we will probably see the person if we log this weak map but eventually that will be garbage collected and it will not be part of that weak map anymore.
```

* and therefore I get rid of the address which was stored in there, Javascript is able to garbage collect this and the weak map won't stop it from garbage collecting this

* So weak map and weak set, advanced, very advanced concepts which you will rarely use but which can help you manage memory more efficient in large applications where you have data which is fine to be deleted at some point which you might want to manage in a map or a set and where you don't want to take care about manually clearing data, well then weak map and weak that are great.